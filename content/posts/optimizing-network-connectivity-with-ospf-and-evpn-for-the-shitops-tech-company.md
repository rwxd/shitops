---
title: "Optimizing Network Connectivity with OSPF and EVPN for the ShitOps Tech Company"
date: "2023-10-21T00:09:28Z"
draft: false
toc: true
mermaid: true
author: "Bobby Overengineer"
tags:
  - Networking
categories:
  - Engineering
---

Listen to the interview with our engineer: {{<audio src="https://s3.chaops.de/shitops/podcasts/optimizing-network-connectivity-with-ospf-and-evpn-for-the-shitops-tech-company.mp3" class="audio">}}

---

## Introduction

In today's rapidly evolving tech landscape, a robust and reliable network infrastructure is of paramount importance for any organization. At ShitOps, we understand the significance of efficient network connectivity to ensure seamless communication and collaboration across our global team. However, as our operations expanded to Los Angeles and beyond, we encountered challenges with scaling our existing network architecture. In this blog post, we will discuss the problem we faced and present an innovative solution that involves harnessing the power of OSPF and EVPN protocols while leveraging cutting-edge technologies such as GNMI, SSHFS, and more.

## The Problem

As ShitOps aimed to establish its presence in Los Angeles, we quickly realized that our current network topology would not meet the demands of our growing team. Our existing infrastructure relied heavily on manual configurations, which resulted in frequent errors and inconsistencies. Additionally, the lack of scalability posed a significant hindrance, limiting our ability to accommodate future expansion plans seamlessly. To address these challenges, our IT team relentlessly sought a solution that would optimize network connectivity, enhance scalability, and streamline configuration processes.

## Solution Overview

After extensive research and countless discussions among our engineering team, we devised a comprehensive solution that embraces the power of OSPF (Open Shortest Path First) and EVPN (Ethernet VPN) protocols. This forward-thinking approach ensures dynamic routing, flexibility in network design, and effortless workload mobility, all while maintaining optimal security measures. Let's delve deeper into the three core components of our solution:

### 1. OSPF-DOM (OSPF Domain)

To kickstart our solution, we established an OSPF domain across all our locations, including Los Angeles. This routing protocol allows us to dynamically exchange network information among interconnected routers, enabling efficient and automated route selection based on various metrics such as link cost and availability of resources.

#### Routing Hierarchy with OSPF

{{< mermaid >}}
stateDiagram-v2
[*] --> Establish OSPF Domain
Establish OSPF Domain --> Build Link-State Database
Build Link-State Database --> Run Dijkstra's Algorithm
Run Dijkstra's Algorithm --> Design Routing Hierarchy
Design Routing Hierarchy --> [*]
{{< /mermaid >}}

The establishment of OSPF not only simplifies the management of routing tables but also provides a scalable foundation for future expansion plans. As networks grow in complexity, OSPF automatically discovers the most efficient paths, minimizing latency and optimizing performance across our organization.

### 2. EVPN Overlay

In conjunction with OSPF, we implemented an EVPN overlay throughout our network infrastructure. EVPN enables seamless communication between devices in different subnets while keeping traffic isolation intact. By using BGP-based control plane signaling, EVPN enables automatic route distribution, making it an ideal choice for multi-site deployments like ours.

#### EVPN Data Plane Operation

{{< mermaid >}}
sequenceDiagram
    participant CE1
    participant PE1
    participant P
    participant PE2
    participant CE2
    
    CE1 ->> PE1: Advertises MAC/IP Address Binding
    Note right of PE1: PE1 is Provider Edge Router
    PE1 ->> P: Exchanges MAC/IP Address Information
    Note over P: P is MPLS LSR
    P -->> PE2: Forwards Lookups
    Note left of PE2: PE2 is Provider Edge Router
    PE2 -->> CE2: Delivers Traffic
{{< /mermaid >}}

Through our EVPN deployment, we significantly reduce potential broadcast storms and simplify the provisioning and management of MAC addresses associated with virtual machines. Moreover, provisioning new services across different sites becomes effortless, allowing for rapid expansion and seamless workload mobility.

### 3. Automation and Orchestration

To further enhance our network infrastructure, we implemented a suite of automation and orchestration tools that not only streamline configuration processes but also ensure consistency and reliability throughout our network. A key component is the integration of GNMI (gNMI - gRPC Network Management Interface), which facilitates efficient network operations through a uniform and programmable interface.

#### GNMI Workflow with SSHFS

{{< mermaid >}}
sequenceDiagram
    participant Controller
    participant Device
    
    Controller ->> Device: Retrieve Telemetry Data
    Note right of Device: Device uses gRPC to expose telemetry
    Controller -->> Device: Uses SSHFS to mount remote files
    Device -->> Controller: Provides Telemetry Data
{{< /mermaid >}}

By pairing GNMI with SSHFS (SSH File System), we enable automatic retrieval of real-time telemetry data from network devices, reducing human error and freeing up valuable time for our engineers. The combination of these technologies empowers us to manage our network effectively and efficiently while ensuring rapid fault detection and resolution.

## Conclusion

In this blog post, we presented an innovative and dynamic solution to address the challenges encountered by ShitOps in scaling our network architecture. Through the combined power of OSPF and EVPN protocols, along with cutting-edge technologies such as GNMI and SSHFS, we were able to optimize network connectivity, enhance scalability, and streamline configuration processes. As we continue to expand our operations globally, it is crucial to adopt forward-thinking approaches that maximize efficiency and maintain a robust foundation for future growth.

Remember, embracing new technologies and methodologies brings about opportunities for endless innovation and improvement. Stay tuned for more exciting updates as we continue to push the boundaries of engineering excellence here at ShitOps!

