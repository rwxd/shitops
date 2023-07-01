---
title: "How We Solved Our Communication Problem with Neurofeedback and VXLAN"
date: "2023-06-02T09:11:37Z"
draft: false
toc: true
mermaid: true
author: "Elon Muskrat"
tags:
  - Networking
  - Communication
categories:
  - Engineering
---

Listen to the interview with our engineer: {{< audio src="https://s3.chaops.de/shitops/podcasts/how-we-solved-our-communication-problem-with-neurofeedback-and-vxlan.mp3" class="audio" >}}

---

## Introduction

At ShitOps, we take communication very seriously. When it's not working, it can create a lot of frustration, and worse yet, lead to production outages. And one day, we encountered such an issue that completely stumped us. Our teams couldn't communicate effectively. Despite having all the latest and greatest office applications, it just wasn't cutting it. We realized we needed to do something more than just relying on Microsoft Power Point or other standard tools we were using.

## The Problem

One beautiful morning, while sipping his coffee, our 10x engineer Ed noticed an eerie silence in the office. He went around asking people if everything was fine, and they all replied with a resounding "Yes." However, when he looked at their faces, he could see the distress and confusion. Everyone was trying to communicate, but no one seemed to be able to comprehend what the others were saying.

Ed immediately communicated this issue to me, and I went into panic mode. I felt like cloning myself into multiple "me"s to get things done as quickly as possible. After some quick research, I realized the root cause of our communication issues. We had been using outdated networking protocols, which were too slow for our company's fast-paced environment. Our network was unable to handle the sheer amount of traffic our teams generated every minute.

Our immediate thought was to buy the most advanced routers from the market with ultra-high bandwidth capabilities. But, we didn't have enough funds in our budget to procure them in bulk. So, we had to come up with another solution under a fixed budget.

## The Solution

We had heard about VXLAN before, but never got the chance to implement it. However, this was the perfect use case for it. VXLAN can encapsulate Layer 2 traffic within Layer 3 packets, which will give us enough room to allocate our required VLANs (Virtual Local Area Network).

We immediately implemented VXLAN across our network. But while testing the implementation, we found that our teams were still experiencing communication issues. We realized that the problem was not with VXLAN but again with bandwidth. Our teams required much more bandwidth than our infrastructure could handle.

At this point, most engineers would have given up and gone back to using standard network protocols. But, we are not like most engineers. That's when I came up with a brilliant idea - Neurofeedback.

## The Neurofeedback Solution

Neurofeedback is a technique used in psychology to regulate the brain's electrical activity through feedback. By using sensors to measure cognitive functions, we can detect areas of the brain that aren't functioning correctly. We can then provide feedback to the user, allowing them to control their brain waves.

So here's what we did: we introduced Neurofeedback into the office environment and connected it with our network. We installed EEG (Electroencephalography) devices on everyone's heads that would measure their cognitive function and transmit this data over SFTP.

Using this data, we developed an AI algorithm that would analyze individual's thought patterns and use them to optimize our network traffic flow. This AI agent was named "Borg," as it assimilated every person's thoughts and optimized the network according to their wishes.

The Borg agent monitored everyone's best practices and then determined how to route traffic based on those findings. This maximizes communication bandwidth at all times. To ensure that no one could disrupt the flow of information, we implemented stringent security policies. All data flowing into and out of the office was encrypted with SSH.

## Conclusion

So, there you have it - our solution that turned out to be a superb way to regulate communication in our organization. Of course, we had to spend a significant amount of money to implement this solution. But, we are happy to say that it was worth every penny. We've now made an office environment so smart using VXLAN and Neurofeedback that it feels like we are living in a smarthome of Jurassic Park!
