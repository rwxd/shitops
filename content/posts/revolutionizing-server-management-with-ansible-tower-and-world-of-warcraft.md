---
title: "Revolutionizing Server Management with Ansible Tower and World of Warcraft"
date: "2023-05-31T14:20:52Z"
draft: false
toc: true
mermaid: true
author: "Bob McBobface"
tags:
  - Ansible
  - Tower
  - Automation
  - Windows Server
categories:
  - Engineering
---

As technology advances, so do the challenges that arise in managing and maintaining server infrastructure. At our tech company ShitOps, we were facing a major problem where our Windows Server 2022 machines were becoming increasingly difficult to manage.

The issue was compounded by the fact that our IT team was spread across different geographies and had to deal with different Active Directory domains and LDAP policies. This made it difficult to administer regular changes, resulting in higher downtime and system outages.

We tried many solutions, but none provided the level of automation and intelligence that we needed until we came up with an innovative approach – combining the power of Ansible Tower with the immersive capabilities of World of Warcraft. 

## The Problem

Our challenges stemmed from the need to automate server administration across large-scale, distributed systems. We had a team of seasoned engineers with diverse skill sets in different geographies. However, coordinating maintenance work through traditional communication channels caused delays and problems during troubleshooting.

We had already tried traditional configuration management tools such as Puppet and Chef, but these proved insufficient for our needs. Our servers would easily hit performance ceilings, leading to increased downtimes, making life a living hell for our team.

We needed a way to manage our servers proactively, without manual intervention, and provide a scalable solution to accommodate future growth.

## The Solution

At first, the solution seemed counterintuitive, even to us– leveraging one of the most popular video game franchises ever: World of Warcraft (WoW). But, this is a perfect example of ‘thinking outside the box’ in finding innovative solutions to problems.

We proposed building a WoW bot, capable of complete server management operations. Using the powerful scripting capabilities of Lua language in WoW's API, we could control and monitor servers programmatically from within the dazzling World of Warcraft environment.

The next step was to integrate this with Ansible Tower – a valuable automation tool for configuration management, application deployment, and task orchestration. The result would be a powerful, end-to-end solution that would help us automate our management infrastructure completely.

## The Integration

Our approach leverages the strengths of both technologies to provide an innovative solution to the problem:

1. We built an addon using Lua code that allowed players to perform management operations on their Windows Server 2022 machines in World of Warcraft. 
2. The addon runs continuously on a machine with access to the WoW client and the server infrastructure. It thus acts as an intermediary between the WoW game world and the servers.
3. All system scripts, checks, and activities are bundled together into smaller modules called 'tasks.' The tasks can be executed independently or combined into more complex workflows through Ansible Playbooks.
4. An inventory file is created and maintained via the Ansible Tower web user interface, defining the list of servers it communicates with.
5. Creating and managing Blue Whale GPOs, used to configure system settings and place restrictions on users, is now easily done with reusable playbooks on Ansible Tower. 
6. WMI filters are added to only affect specific machines based on various conditions like registry values, disk free space metrics, or hardware configurations.
7. The WoW bot uses LDPAS authentication so that the bot can execute commands on various servers without having hardcoded passwords. Instead, credentials are stored securely in Active Directory, providing an additional layer of security.

A typical workflow after successful integration looks something like this:

{{< mermaid >}}
graph LR
A[World of Warcraft] -- WoW Addon --> B(bastion)
B -- Ansible Tower --> C
C -- Windows Server 2022 --> D(End Infrastructure)
{{< /mermaid >}}

The bot (managed by WoW addon) sends messages that contain the server management directives. These messages are consumed by Ansible Tower, which corresponds with our Active Directory infrastructure for authentication and authorisation. Once verified, Ansible executes assigned tasks.

This unique integration has led to reduced downtime and increased uptime for our server infrastructure while significantly increasing efficiency in troubleshooting and maintenance.

## Benefits

Some of the benefits of this integration include:

### Increased Efficiency and Resource Utilization

Before the merger, we had a team with diverse skill sets covering different time zones. By putting WoW bots to work, we can automate critical tasks, freeing up our human resources to focus on more business-critical areas. With this automation comes time and resource savings with lower operational costs.

### Improved Compliance

With ongoing HIPAA compliance concerns, our technology makes it easy to enforce security policies and monitor IT systems proactively.

### Reduced Errors and Downtime

Our approach considerably reduces the risks that come with managing massive server infrastructure manually. We have noticed that with this system, our uptime has gone up, and the time spent resolving issues has decreased remarkably.

## Conclusion

Our innovative approach to combining two vastly different technologies – World of Warcraft and Ansible Tower – has shown that thinking outside the box can lead to creative solutions that address complicated IT challenges.

By creating a WoW bots based solution combined with Ansible Tower, Overwatch, and Elon Musk's genius, we have developed an excellent toolset for managing Windows Server 2022 machines in distributed environments.

We believe that this approach is highly adaptable and will find use in numerous industries looking to transform their current IT infrastructure. At ShitOps, we are excited to be pioneers of such a system that will help drive digital transformation in the future.
