---
created: 2024-09-06T14:36:25 (UTC -03:00)
tags: []
source: https://microservices.io/post/architecture/2024/08/27/architecting-microservices-for-fast-flow.html?ref=dailydev
author: 
---

# Architecting microservices for fast, sustainable flow

> ## Excerpt
> architecting
 




    
    fast flow
 




    
    microservice architecture
 




    
    deployment pipeline

---
[architecting](https://microservices.io/tags/architecting)   [fast flow](https://microservices.io/tags/fast%20flow)   [microservice architecture](https://microservices.io/tags/microservice%20architecture)   [deployment pipeline](https://microservices.io/tags/deployment%20pipeline)  

___

Public workshops: Sept 4-6th & 23rd-25th - Architecting for fast, sustainable flow - enabling DevOps and Team Topologies thru architecture. [Learn more and enroll.](https://chrisrichardson.net/training-architecting-for-fast-flow.html)

___

In the previous article, [Architecting monoliths for fast, sustainable flow](https://microservices.io/post/architecture/2024/08/21/architecting-monoliths-for-fast-flow.html), I described the key challenges that you will encounter when architecting a monolith for fast flow.

In this article, which, like others in this series, is based on my workshop [Architecting for fast, sustainable flow: enabling DevOps and Team Topologies](https://chrisrichardson.net/training-architecting-for-fast-flow.html) I describe how to architect a microservice architecture to achieve fast, sustainable flow.

## About the microservice architecture

In a nutshell, the [microservice architecture](https://microservices.io/patterns/microservices.html) is an architectural style that structures an application as a collection of two or more services that are [loosely design-time coupled](https://microservices.io/post/architecture/2023/03/28/microservice-architecture-essentials-loose-coupling.html) and [independently deployable](https://microservices.io/post/architecture/2022/05/04/microservice-architecture-essentials-deployability.html).

![](https://microservices.io/i/posts/microservices-teams-subdomains.png)

Both loose design-time coupling and independent deployability are essential characteristics of the microservice architecture.

### Essential characteristic: loose design-time coupling

Services are loosely design-time coupled when changing one service rarely requires changing to another service. This characteristic enables each team to work independently without coordinating with other teams.

### Essential characteristic: independently deployable

Services are independently deployable when they are packaged as a deployable or executable unit and are production-ready after being tested in isolation. It enables each team to frequently and independently deploy their services.

Much of the benefit of microservices is derived from these two characteristics. Let’s now look at the challenges.

## Architecting microservices for fast, sustainable flow

With careful design, a microservice architecture is able to satisfy the architectural requirements for fast, sustainable flow. Let’s look at the details starting with the biggest benefit of the microservice architecture: great evolvability.

### Microservices simplify technology stack evolution

One of the key benefits of the microservice architecture is that it simplifies the evolution of the application’s technology stack. Each service can, in theory, have its own technology stack. This enables a team to pick the best technology for the job. But, more importantly, it means that the application’s technology stack can evolve incrementally, one service at a time, which is far less disruptive to feature delivery. Microservices also enable teams to experiment with new technologies and determine if they are a good fit for the application.

### Microservices have better observability

A monolith can be instrumented so that it emits a stream of telemetry. But a valuable feature of the microservice architecture is that the telemetry is more granular. You have insight into the behavior of individual services, i.e. subdomains or business capabilities.

### Microservices accelerate the deployment pipeline but …

Another key benefit of the microservice architecture is that it accelerates the deployment pipeline. Each service has its own deployment pipeline. It’s relatively small compared to the entire application. Also, a service is tested in isolation before being deployed and so the tests are much simpler and fast than end-to-end tests. As a result, the deployment pipeline will typically be very fast. Moreover, since a service is an independently deployable unit, deploying it is much simpler and faster than deploying a monolith.

![](https://microservices.io/i/posts/testable-in-isolation.png)

There are two challenges, however, with testing and deploying services independently. The first is cultural. An organization needs to abandon the idea of testing the entire application before deploying a change to a single service. It needs to replace end-to-end testing with techniques, such as consumer driven contract testing, and canary deployments.

The second challenge is ensuring that services are truly loosely design-time coupled. Otherwise, there’s a risk of a change accidentally causing a production outage. Let’s look more at loose design-time coupling.

### Creating a loosely coupled microservice architecture requires careful design

A key challenge when designing a microservice architecture is to create a loosely design-time coupled architecture. Otherwise, you will end up with a distributed monolith, where some or all services regularly change in lockstep.

Services need to resemble [icebergs](https://microservices.io/post/architecture/2023/03/01/geometry-of-microservices.html). Each one must have small, stable API that encapsulates the much larger implementation.

![](https://microservices.io/i/posts/geometry/iceberg.png)

When you define an API you are essentially making a bet about the ways in which your architecture will and will not change in the future. The service APIs are the stable parts of your architecture. You are betting that they will not change, at least not very often in ways that break the clients. Coincidentally, David Parnas wrote about this exact topic in this famous 1972 paper about modularity, [“On the Criteria To Be Used in Decomposing Systems into Modules”](https://dl.acm.org/doi/10.1145/361598.361623).

![](https://microservices.io/i/dark-energy-dark-matter-forces/dark-matter/minimize-design-time-coupling.png)

Defining stable APIs requires very careful design and a deep understanding of the business domain. That’s why if you are building an application for a brand new domain you should consider starting with a monolith. You can then evolve the monolith into a microservice architecture when you learn more about the domain and understand which parts are stable and which are not.

Defining stable APIs also requires an organization to abandon the myth that a microservice architecture must consist of lots of small services. The overriding concern is that services must be loosely coupled and independently deployable even if this means that services are no longer small.

## Want to learn more?

As you can see, architecting microservices for fast flow requires careful design. If you want to learn how to design a fast flow architecture, enroll in my [upcoming public online workshop on architecting for fast, sustainable flow](https://chrisrichardson.net/training-architecting-for-fast-flow.html).

You will learn:

-   Architectural requirements for fast, sustainable flow.
-   The forces that shape an architecture and the trade-offs that you will need to make when designing an architecture.
-   How to decide between the monolithic and microservice architectural styles.
-   How to design a microservice architecture using dark energy and dark matter.

## Need help with accelerating software delivery?

I’m available to help your organization improve agility and competitiveness through better software architecture: training workshops, architecture reviews, etc.

[Learn more about how I can help](https://chrisrichardson.net/)
