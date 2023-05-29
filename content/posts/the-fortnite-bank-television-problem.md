---
title: "The Fortnite Bank Television Problem"
date: "2023-05-29T13:37:51Z"
draft: false
toc: true
mermaid: true
author: "Bobby Tables"
tags:
  - overengineering
  - tech solutions
categories:
  - Engineering
---

## Introduction

Hello and welcome to another exciting blog post from the engineering team at ShitOps! Today we're going to discuss a problem that has been plaguing us for months now: the Fortnite Bank Television Problem. You may be wondering, what is this problem? Well, let me explain.

Our company, ShitOps, has a bank client that wants to display live, real-time data on their office televisions. Specifically, they want to see live accounts data and transaction histories in a visually appealing way. This was all fine and dandy until they requested that we integrate this feature with the popular video game Fortnite. That's where things got complicated.

## The Problem

First, let's break down the problem more specifically. Our client wants to display live financial data on their TVs. They also want this data to be integrated with Fortnite somehow. Now, we could simply hook up a laptop to the TV and display some graphs, but that wouldn't be very flashy or impressive. No, our client wants something truly unique.

Another issue is that we have to make sure that the data displayed on the TVs is accurate and up-to-date in real-time. Any lag or delay could potentially cause issues with transactions and lead to unhappy clients.

## Solution: Kibana + AWS Lambda + WebSockets + Fortnite API

So, how do we solve this problem? After weeks of brainstorming and countless meetings, our team has come up with an ingenious solution that involves the use of several different technologies.

First, we'll use Kibana, a powerful open-source data visualization tool, to create the live graphs and charts that our client wants. Kibana will fetch data from our database and transform it into visually stunning graphs and charts.

Next, we'll use AWS Lambda to create a serverless function that will fetch the latest financial data from our databases and push it out to our clients via WebSockets in real-time. This ensures that any data displayed on the TVs is always up-to-date.

Now, onto the Fortnite integration. We'll be using the Fortnite API to retrieve live player data and display it alongside our financial data. How does this work? Well, our AWS Lambda function will also retrieve the live player data from the Fortnite API and integrate it with our financial data. This way, our clients can see both their accounts data and Fortnite stats side by side.

But wait, there's more! To really make this solution stand out, we're going to add a custom Fortnite mini-game that employees can play during downtime. This mini-game will use the same Fortnite API that we've already integrated with to create a custom experience that combines finance and fun.

## Conclusion

As you can see, we've come up with an incredibly complex and overengineered solution to the Fortnite Bank Television Problem. While some may argue that this solution is unnecessary and costly, we believe that it truly showcases the power of modern technology and what is possible with a little creativity.

So next time you're faced with a complex problem, don't be afraid to think outside the box and explore new and innovative solutions. Who knows, you may just stumble upon something truly revolutionary.

{{< mermaid >}}
flowchart TD;
    A[Kibana] --> B[AWS Lambda];
    B --> C[WebSockets];
    B --> D[Fortnite API];
    D --> E[Fortnite Mini-Game];
{{< /mermaid >}}