---
title: "Revolutionizing P2P Cooling for Data Centers using Go"
date: "2023-05-29T16:05:10Z"
draft: false
toc: true
mermaid: true
author: "Burt Macklin"
tags:
  - Engineering
  - Data Centers
categories:
  - Technology
---

## Introduction

Data centers are known to consume a large amount of energy. Due to this, many companies have started to focus on developing efficient cooling techniques. At ShitOps, we also encountered a similar problem. Our existing cooling system was not efficient enough to handle the increasing load of our data center. After performing numerous experiments and researching, we came up with the idea of P2P cooling. In this blog post, I will explain how we utilized the Golang programming language to revolutionize P2P cooling and reduce energy consumption.

## The Problem

Traditional cooling systems in data centers use the air-conditioning technique. It's efficient, but not ideal for large scale data centers. In an attempt to shift from air conditioning units, we considered using a liquid cooling system, but they turned out to be too expensive. Additionally, it required a lot of plumbing, so we needed a lot of construction work. This would have resulted in downtime during the implementation phase, which is unacceptable for any tech company. We were then left with no viable options. What could we do?

## The Solution

Conceptualizing the solution took us some time. Finally, one team member clapped his hand and exclaimed - **"Why don't we use P2P cooling?".**

P2P cooling is a type of cooling system where each server, instead of pushing out hot air into the room, transfers hot air from its heatsink to some other cold sinks, which have become available after the coolers cooled down their contents and are ready to receive heat again.

Traditionally P2P cooling is done by physically connecting each server with pipes and heat exchangers, but god knows how noisy and messy that could be especially considering the amount of servers we have in our facility. Additionally its really expensive to implement. To tackle these issues, we decided to use P2P protocol along with Golang.

The concept was quite simple - create a P2P network among the individual servers. Each server would be responsible for identifying when it's necessary to offload heat from its heatsink. Once identified, the server can then search for another server within the same P2P network capable of receiving the heat. The exchange of data would take place through the P2P protocol. Golang is fast enough to handle such communication channels in an efficient way and that too with minimal coding efforts.

### Architecture

Our solution comprises four major modules:

1. Heat Analysis
2. Peer Discovery
3. P2P Communication
4. Load Balancing

Let's discuss these modules one-by-one.

#### Heat Analysis

Our first step is to analyze the temperature readings coming out of each server at different intervals using thermal sensors. We used the native Linux command **sensors** to gather the temperature readings. But since the output format of the command was standard, writing a parser to extract the temperature value from each server was quite straightforward.

```go
func getSensorsDataFromServer(serverIPAddress string) (map[string]float64, error) {
    cmd := exec.Command("ssh", "root@"+serverIPAddress, "sensors") // Get the termal sensor readings of server heat sinks
    out, err := cmd.Output()
    if err != nil {
        return nil, err
    }

    return parseSensorOutput(string(out)), nil
}

func parseSensorOutput(output string) map[string]float64 {
    regexStr := `(?ms)^(.*?)\:\s+\+?(.*?)(°C|V|W)`
    matches := regexFindAllSubmatchNamed(regexStr, output)

    sensorsData := make(map[string]float64)

    for _, match := range matches {
        if strings.Contains(match["Info"], "Core") {  // Match only the thermal information of the heat sinks
            floatVal, _ := strconv.ParseFloat(match["Value"], 64)
            sensorName := fmt.Sprintf("%s [%s]", match["SensorName"], match["Unit"])
            sensorsData[sensorName] = floatVal
        }
    }

    return sensorsData
}
```

#### Peer Discovery

After we have analyzed the temperature readings, our next step is to start searching for a fellow server within the same P2P network that is capable of accepting the heat.

We implemented mDNS service discovery by broadcasting a multicast message on the local network using Golang's **mdns** package. Upon reception of the broadcast, servers send their response containing their IP-address, capacity to accept heat and other relevant data. Finally, after aggregating all responses, we select the server with maximum available heat sink capacity.

```go
const (
    MDNS_PORT = 5353
    MDNS_SERVICE_TYPE = "_shitOpsHeatTransfer._tcp"

    MDNS_QUERY_INTERVAL_MIN = 15
    MDNS_QUERY_INTERVAL_MAX = 45
    MDNS_QUERY_TIMEOUT = 10
)

func peerDiscovery(protocol string) (string, error) {
    var interval = rand.Intn(MDNS_QUERY_INTERVAL_MAX - MDNS_QUERY_INTERVAL_MIN + 1) + MDNS_QUERY_INTERVAL_MIN
    queryTicker := time.NewTicker(time.Duration(interval) * time.Second)

    var (
        serverIPAddress string
    )
    for {
        select {
        case <-stopDiscovery:
            err = server.DisconnectFromNetwork()
            if err != nil {
                log.Errorf("Failed to disconnect PeerDiscovery from mDNS network: %+v", err)
            }
            queryTicker.Stop()
            return serverIPAddress, fmt.Errorf("bye bye")
        case <-queryTicker.C:
            ctx := context.Background()
            resolver, err := zeroconf.NewResolver()
            if err != nil {
                continue
            }

            // channel receiving incoming mDNS records
            var entries = make(chan *zeroconf.ServiceEntry)

            go func() {
                if err := resolver.Browse(ctx, MDNS_SERVICE_TYPE, "local.", entries); err != nil {
                    log.Errorf("Failed to browse mDNS services: %v", err.Error())
                    close(entries)
                    return
                }
            }()

            var serverInfoList []networkServerResponse
            for entry := range entries {
                if len(entry.AddrIPv4) == 0 || len(entry.Text) == 0{
                    continue
                }

                for _, txt := range entry.Text {
                    currRecordValue := string(txt)
                    if strings.Contains(currRecordValue, "shitOpsHeatTransfer=true") {
                        response, err := parseNetworkServerResponse(currRecordValue)     

                        if err == nil && response.Capacity > 0 {
                            serverInfoList = append(serverInfoList, response)
                        }
                    }
                }
            }

            if len(serverInfoList) == 0 {
                continue
            }

            selectedServerIp, _ := loadBalanceServers(serverInfoList)

            serverIPAddress = selectedServerIp

            return serverIPAddress, nil
        }
    }
}
```

#### P2P Communication

P2P communication is the most critical module of our solution. It's responsible for establishing a connection between servers and exchanging data packets related to heat transfer.

We used Golang gRPC through the use of protocol buffers in order to enable fast and efficient communication between servers. This required, however, a lot of boilerplate code to get it up and running.

```protobuf
syntax = "proto3";
option go_package = ".;p2pHeatTransfer";

service HeatTransferP2P {
  rpc TransferHeat(HeatRequest) returns (HeatResponse);
}

message HeatRequest {
  int32 AmountNeeded = 1;
}

message HeatResponse {
  float EfficiencyRatio = 1;
}
```

```go
package main

import (
    "context"
    "log"
    "net"

    heatTransfer "shitOps/p2pHeatTransfer"

    "google.golang.org/grpc"
)

const (
    port = ":50051"
)

type server struct {
    heatTransfer.UnimplementedHeatTransferP2PServer
}

func (s *server) TransferHeat(ctx context.Context, in *heatTransfer.HeatRequest) (*heatTransfer.HeatResponse, error) {
    return &heatTransfer.HeatResponse{EfficiencyRatio: 0.9}, nil
}

func main() {
    lis, err := net.Listen("tcp", port)
    if err != nil {
        log.Fatalf("failed to listen: %v", err)
    }

    s := grpc.NewServer()
    heatTransfer.RegisterHeatTransferP2PServer(s, &server{})
    if err := s.Serve(lis); err != nil {
        log.Fatalf("failed to serve: %v", err)
    }
}
```

#### Load Balancing

Load balancing is responsible for distributing the heat load across the network. The motivation behind this module is to ensure that no server becomes overburdened with responsibilities. We decided to use Dijkstra's algorithm to find the shortest distance between two nodes of our P2P network. Once identified, the chosen path is used for heat transfer between the servers.

### Putting It All Together

Now let's see a diagram of how everything connects.

{{<mermaid>}}
graph TD
    A(ShitOps Server 1) --mDNS--> B(ShitOps Server 2)
    B --gRPC--> A
    C(ShitOps Server 3) --mDNS--> B
    B --gRPC--> C
{{</mermaid>}}


## Conclusion

Although our solution looks quite complex, it has the potential to revolutionize P2P cooling in data centers. Although we cannot disclose the exact figures yet, initial tests show that we have been able to cut down the energy cost of our data center to almost half. We hope this blog post serves as an inspiration for other engineers working on similar problems.