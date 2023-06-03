---
title: "Solving DNS Resolution Issues at Scale with Microsoft, GNMI, Juniper, Mainframe, Mesh, Self Hosting, Lambda Functions and Open Source"
date: "2023-06-03T11:35:47Z"
draft: false
toc: true
mermaid: true
author: "Bob the Great Engineer"
tags:
  - DNS
  - Microsoft
  - GNMI
  - Juniper
  - Mainframe
  - Mesh
  - Self Hosting
  - Lambda Functions
  - Open Source
categories:
  - Engineering

---

Listen to the interview with our engineer: {{< audio src="https://cdn.shitops.de/audio/solving-dns-resolution-issues-at-scale-with-microsoft-gnmi-juniper-mainframe-mesh-self-hosting-lambda-functions-and-open-source.mp3" class="audio" >}}

---

## Introduction

DNS resolution is a critical part of the network infrastructure for any tech company. It helps in resolving human-readable domain names into IP addresses and vice versa, but at the cost of adding latency to network requests, which can further impact the performance of applications that depend on them.

Recently, our tech company ShitOps faced a DNS resolution issue at scale, due to the increasing number of services added on the network infrastructure. We realized that the traditional approach of using a central DNS server was no longer sufficient to handle this scale.

In this blog post, I will describe how we solved this problem by designing a new architecture that combines Microsoft, GNMI, Juniper, Mainframe, Mesh, Self Hosting, Lambda Functions, and Open Source tools. For ease of understanding, I will break down the solution into five different stages:

1. Collecting data from all DNS resolution sources in the network.
2. Storing the collected data in a centralized database.
3. Configuring Juniper switches based on the stored data.
4. Implementing self-hosted mesh networks to optimize routing.
5. Dynamically deploying and managing the solution using open-source tools.

Let’s dive deep into each stage and understand the technical implementation of the solution.

## Stage 1: Collecting data from all DNS resolution sources in the network

In order to handle the DNS resolution issues at scale, we realized that it was essential to monitor all the DNS resolution sources in our network. These sources included:

- Legacy on-premise mainframes running proprietary DNS resolution systems.
- Legacy distributed DNS servers deployed across various data centers.
- Cloud-based DNS servers deployed on multiple cloud platforms.

We chose GNMI (gRPC Network Management Interface) to collect data from all these sources. GNMI is an interface that provides read and write access to configuration and state data within network devices using gRPC (Remote Procedure Calls over HTTP/2). It is open source, easily scalable, and supports a wide range of programming languages like Python, Java, and Go.

We built a custom script in Python, which used GNMI interface, to collect real-time DNS resolution information from all the sources. The collected data was then sent to a centralized database for further analysis.

{{< mermaid >}}
  sequenceDiagram
    participant DNS_Resolution_Source_1
    participant DNS_Resolution_Source_2
    participant DNS_Resolution_Source_3
    participant GNMI_Script
    participant Centralized_Database
    DNS_Resolution_Source_1 ->>+ GNMI_Script: Request DNS resolution info
    DNS_Resolution_Source_2 ->>+ GNMI_Script: Request DNS resolution info
    DNS_Resolution_Source_3 ->>+ GNMI_Script: Request DNS resolution info
    GNMI_Script ->>- Centralized_Database: Send DNS resolution info
{{< /mermaid >}}

## Stage 2: Storing the collected data in a centralized database

After collecting real-time DNS resolution information from all sources, the next step was to analyze and store it in a centralized database where it could be accessed by other components of the system.

We used Microsoft SQL Server as our centralized database due to its ability to handle large data volumes, high availability, and support for in-memory database structures.

We developed a custom Python script that read data from GNMI output and stored it in the SQL Server database for further processing. The stored data included information such as domain names, IP addresses, TTL values, and source servers.

{{< mermaid >}}
  flowchart LR
	DNS_Servers --> GNMI{Request DNS resolution info}
	GNMI --> PythonScript{Collect and Transform Data}
	PythonScript --> SQLServer{Store DNS resolution info}
	SQLServer --> ReadDataSQL{Read DNS resolution info}
	ReadDataSQL --> PythonScript
{{< /mermaid >}}

## Stage 3: Configuring Juniper switches based on the stored data

Juniper switches are widely used in tech companies due to their reliability, scalability, and security features. In this stage, we wrote a custom Python script that automated the Juniper switch configuration process based on the stored DNS resolution data to optimize the network routing.

The script read data from the Microsoft SQL server and configured Juniper switches using the Junos API. It optimized network routing by selecting the best route based on real-time traffic load, and it also ensured redundant paths were available in case of any network failures.

{{< mermaid >}}
  sequenceDiagram
	participant Juniper_switch_1
	participant Juniper_switch_2
	participant Python_script
	participant Centralized_Database
	Juniper_switch_1 ->>+ Python_script: Request DNS resolution data
	Juniper_switch_2 ->>+ Python_script: Request DNS resolution data
	Python_script ->>+ Centralized_Database: Read DNS resolution data
	Centralized_Database ->>+ Python_script: Send DNS resolution data
	Python_script ->>+ Juniper_switch_1: Update switch config
	Python_script ->>+ Juniper_switch_2: Update switch config
{{< /mermaid >}}

## Stage 4: Implementing self-hosted mesh networks to optimize routing

A Mesh network is a decentralized network infrastructure that dynamically connects devices without the need for a central controlling authority. We realized that implementing self-hosted mesh networks could further optimize the routing process by selecting the best route available based on the real-time traffic load.

We used open-source tools such as Envoy, Istio, and Kubernetes to implement a self-hosted mesh network infrastructure across our data centers. The mesh network ensured that maximum bandwidth was utilized, the latency was minimized, and the overall application performance was optimized.

{{< mermaid >}}
  sequenceDiagram
    participant Application_1
    participant Application_2
    participant Envoy_1
    participant Envoy_2
    participant Kubernetes
    participant Istio
    Application_1 ->>+ Envoy_1: Send request
    Application_2 ->>+ Envoy_2: Send request
    Envoy_1 ->>+ Istio: Request DNS resolution info
    Envoy_2 ->>+ Istio: Request DNS resolution info
    Istio ->>+ Kubernetes: Request updated routing info
    Kubernetes -->>- Istio: Send updated routing info
    Istio ->>- Envoy_1: Send updated routing info
    Istio ->>- Envoy_2: Send updated routing info
    Envoy_1 -->>- Application_1: Send response
    Envoy_2 -->>- Application_2: Send response
{{< /mermaid >}}

## Stage 5: Dynamically deploying and managing the solution using open-source tools

As a tech company, we always strive to use the latest and most innovative open-source tools in our work. For dynamic deployment and management of our DNS resolution system, we used a combination of Jenkins, Ansible, and GitLab.

We built a custom Jenkins pipeline, which used Ansible to deploy the solution to multiple data centers in parallel. The pipeline code was stored in GitLab and triggered automatically whenever we pushed a new change to the repository.

{{< mermaid >}}
  flowchart LR
    GitLabRepo -- Webhook --> Jenkins
    Jenkins -- Playbook --> Ansible 
    Ansible -- Deploy --> DataCenters
{{< /mermaid >}}

## Conclusion

In conclusion, we solved our DNS resolution issue at scale by building a complex architecture that combined Microsoft, GNMI, Juniper, Mainframe, Mesh, Self Hosting, Lambda Functions, and Open Source tools. We broke down the solution into five different stages and described the technical implementation of each stage.

Although this solution may seem over-engineered with a high level of complexity for some, we are confident that it is the optimal way to handle our network infrastructure's scaling issues, and we are proud of our innovation in addressing the problem.

We hope you have enjoyed reading this blog post and learned something new about how we solve problems at ShitOps. Stay tuned for more exciting updates from us!
