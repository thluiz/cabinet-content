---
created: 2024-07-11T21:05:56 (UTC -03:00)
tags: []
source: https://microservices.io/post/architecture/2024/07/10/microservices-rules-what-good-looks-like.html?ref=dailydev
author: 
---

# Microservices rules: what good looks like - July 2024 edition

> ## Excerpt
> microservice architecture
 




    
    architecting
 




    
    team topologies
 




    
    software delivery metrics
 




    
    microservices rules

---
## Microservices rules: what good looks like - July 2024 edition

[microservice architecture](https://microservices.io/tags/microservice%20architecture)   [architecting](https://microservices.io/tags/architecting)   [team topologies](https://microservices.io/tags/team%20topologies)   [software delivery metrics](https://microservices.io/tags/software%20delivery%20metrics)   [microservices rules](https://microservices.io/tags/microservices%20rules)  

___

New public workshop: Architecting for fast, sustainable flow - enabling DevOps and Team Topologies thru architecture. [Learn more and enroll.](https://chrisrichardson.net/training-architecting-for-fast-flow.html)

___

Yesterday, I gave this presentation at a company’s internal meetup. It’s the latest version of my _Microservices Rules_ talk. These 11 rules are a great checklist that engineering leaders can use to assess the state of their organization, its delivery practices and its application’s architecture.

![](https://microservices.io/i/microservices-rules-images/microservices-rules-2024.png)

## About the rules

As I [previously described](https://microservices.io/post/architecture/2024/01/23/microservices-rules-what-good-looks-like.html):

> The microservice architecture has become increasingly popular over the past decade. Its key benefits include significantly improving the developer experience and accelerating software delivery. Sadly, however, microservices have often been widely misunderstood and used inappropriately. As a result, many organizations have struggled to benefit from their adoption. I’ve had numerous conversations where developers have complained that their new microservices-based applications are difficult to change.
> 
> In this presentation, I describe 11 development and architecture rules (a.k.a. best practices) that should prevent an organization from making a mess of its microservice architecture. I say _should_ because given semantic diffusion and incredible ability of many organizations to misinterpret and misapply rules, there are no guarantees. However, I believe that if an organization follows these rules when adopting microservices, it will increase their chance of success.

## The July 2024 version of the rules

Here’s the current list of rules:

1.  Practice continuous delivery/deployment
2.  [Implement fast, automated deployment pipelines](https://microservices.io/post/architecture/2024/02/27/microservices-rules-2-fast-deployment-pipeline-premium.html)
3.  **Apply Team Topologies: organizing for fast flow** (was: Organize into small, cross-functional, loosely coupled teams)
4.  Provide a great developer experience (DevEx)
5.  Use a rational design process
6.  Design independently deployable services
7.  Design loosely coupled services
8.  Design testable services
9.  Develop observable services
10.  **Big/risky change => smaller/safer and (ideally easily) reversible changes** (was: Incrementally migrate from a monolith to microservices)
11.  Track and improve software metrics and KPIs

The motivations for the two changes are as follows:

-   Rule #3 - It’s best to simply use [Team Topologies](https://teamtopologies.com/) as the guide for organizing teams for fast flow.
-   Rule #10 - I generalized this rule since there’s a tendency for organizations to [make big, risky changes in all kinds of situations](https://microservices.io/post/architecture/2024/06/27/stop-hurting-yourself-by-doing-big-bang-modernizations.html), not just when migrating from a monolith to microservices. An organization might, for example, spend months reimplementing a subsystem and then spend a stress filled weekend migrating all users to that new subsystem. A better approach is to incrementally develop the new subsystem and then incrementally migrate users to it.

## Slides

## Speaking at your company

I’d be delighted to deliver this talk at your company. See here for [more details](https://chrisrichardson.net/speaking.html).

## Need help with accelerating software delivery?

I’m available to help your organization improve agility and competitiveness through better software architecture: training workshops, architecture reviews, etc.

[Learn more about how I can help](https://chrisrichardson.net/)

___

[microservice architecture](https://microservices.io/tags/microservice%20architecture)   [architecting](https://microservices.io/tags/architecting)   [team topologies](https://microservices.io/tags/team%20topologies)   [software delivery metrics](https://microservices.io/tags/software%20delivery%20metrics)   [microservices rules](https://microservices.io/tags/microservices%20rules)  

___

Copyright © 2024 Chris Richardson • All rights reserved • Supported by [Kong](https://konghq.com/).
