---
created: 2024-04-15T12:39:01 (UTC -03:00)
tags: []
source: https://microservices.io/post/architecture/2024/03/28/success-triangle-microservices-as-an-enabler.html?ref=dailydev
author: 
---

# The evolution of the success triangle: microservices as the enabler of DevOps and team topologies

> ## Excerpt
> architecting
 




    
    success triangle
 




    
    devops
 




    
    team topologies

---
[architecting](https://microservices.io/tags/architecting)   [success triangle](https://microservices.io/tags/success%20triangle)   [devops](https://microservices.io/tags/devops)   [team topologies](https://microservices.io/tags/team%20topologies)  

___

In my 2018 book [Microservices Patterns](https://microservices.io/book.html) I described the success triangle. It shows the relationship between the three ingredients necessary for the rapid, frequent and reliable delivery of software.

![](https://microservices.io/i/fast-flow/SuccessTriangle.png)

As I wrote in the book:

> For a large, complex application, the microservice architecture is usually the best choice. But in addition to having the right architecture, successful software development requires you to also have organization and development and delivery processes.

The delivery process is DevOps, which I define below. The organization structure is a loosely coupled network of small product teams. Six years later the success triangle is still very relevant. It’s a great way to introduce the motivations behind microservices.

In this article, I describe the evolution the success triangle and explain why the primary goal of using the microservice architecture is to enable DevOps and team topologies. I also briefly cover how to decide between the monolithic architecture and the microservice architecture for your application.

## Software is eating the VUCA world

One early enhancements to the success triangle was to include the two key trends that are driving the adoption of microservices. Here’s the success triangle from my recent [ExploreDDD 2024 presentation](https://microservices.io/post/architecture/2024/03/18/exploreddd-physical-design-principles-for-microservices.html):

![](https://microservices.io/i/fast-flow/success-triangle-march-2024.png)

It shows the trends: software is eating the world; and increasing volatility, uncertainty, complexity and ambiguity (VUCA). Let’s look at each one in turn stating with software is eating the world.

### Trend #1: Software is eating the world

The first trend, which is quite old, is the increasingly important role that software plays in the delivery of an enterprise’s products and services. This trend is captured by the phrase [“software is eating the world”](https://microservices.io/post/microservices/2020/02/18/why-microservices-part-1.html#trend-1---software-is-eating-the-world), which was coined by Mark Andreessen in 2011. It doesn’t matter what industry you’re in, software is important. The [Southwest Airlines meltdown during Christmas 2022](https://www.transportation.gov/briefing-room/dot-penalizes-southwest-airlines-140-million-2022-holiday-meltdown) is a great example of the importance of software.

### Trend #2: increasing volatility, uncertainty, complexity and ambiguity (VUCA)

The second trend is the [increasing volatility, uncertainty, complexity and ambiguity (VUCA)](https://microservices.io/post/microservices/2020/02/18/why-microservices-part-1.html#trend-2---the-marketplace-is-becoming-increasingly-volatile-uncertain-complex-and-ambiguous) of the business environment in particular, and the world in general. I used to introduce VUCA by talking about how, for example, changes to regulations meant that ancient banks suddenly needed to compete with fintech startups. But then the COVID pandemic happened. Not only was there terrible loss of life, but the pandemic also disrupted businesses and IT in ways that no one could have predicted. And, today other examples of VUCA include on-going wars and impact of climate change.

### IT needs to deliver those applications rapidly, frequently and reliably

A business must be much more nimble in order to thrive in today’s VUCA world. And since IT is responsible for the applications that power the business, it needs to deliver those applications much more rapidly, frequently and reliably.

## Team topologies and the DevOps handbook

Another enhancement to the success triangle was to include two books: [the DevOps Handbook and Team Topologies](https://microservices.io/post/microservices/2020/01/07/books-about-high-performance-software-delivery.html). Team Topologies is a great addition to the success triangle since it describes an organizational structure (aka. topology) that organizes people into teams that can deliver software quickly and reliably.

Also, to avoid any confusion about the term DevOps, I included the DevOps Handbook. Unfortunately, DevOps is frequently misinterpreted as merely a set of tools or a job title. The DevOps Handbook, however, describe how it’s a set of principles and practices that are essential for rapid and reliable software delivery.

## Architecture as the enabler of DevOps and team topologies

Initially, I merely characterized DevOps and Team Topologies as necessary components in the adoption of microservices. But at some point, a major shift occurred in how I thought about the relationship between process, organization and architecture. I started to see DevOps and team topologies as the goal and architecture as an enabler. In other words, embracing only DevOps and Team Topologies is not enough. Your application’s architecture must also support DevOps and Team Topologies.

## Architectural requirements for DevOps and Team topologies

DevOps and Team topologies require an architecture with the following characteristics:

-   [loosely (design-time) coupled](https://microservices.io/articles/dark-energy-dark-matter/dark-matter/minimize-design-time-coupling.html) - a fundamental property of good software. In particular, the ‘modules’ owned by different teams should be loosely coupled in order to minimize the need for coordination between teams.
-   testable - fast running automated tests that can be run locally and a fast deployment pipeline.
-   deployable - fast reliable, automated deployments in order to prevent the final step of the delivery pipeline from becoming a bottleneck.
-   observable - the production environment must emit a stream of telemetry - logs, metrics, traces - so that developers can understand both application and user behavior

An organization that wants to use DevOps and Team Topologies picks an architecture that has these characteristics. For some applications, that architecture can be a (modular) monolith and for others it can be microservices.

## Designing an architecture that supports DevOps and Team Topologies

The first step in designing an architecture is to decide between the [monolithic architecture](https://microservices.io/patterns/monolithic.html) and the [microservice architecture](https://microservices.io/patterns/microservices.html). The [dark energy forces](https://microservices.io/post/architecture/2023/03/26/dark-energy-dark-matter-force-descriptions.html) are a useful framework for making this decision. For example, see [this slide from a recent presentation](https://speakerdeck.com/cer/ddd-necessary-but-insufficient-physical-design-principles-for-microservices-c384bb76-6160-4440-9d94-455b9bebcccb?slide=37) that describes how your context determines the relevant dark energy forces to your application. The strength of these forces determines which architecture is the best choice.

### Designing a monolithic architecture

If you choose to use a monolithic architecture, it’s usually best to design a [modular monolith](https://microservices.io/post/architecture/2023/07/31/how-modular-can-your-monolith-go-part-1.html). When designing a monolith, it’s important to ensure that its modules are loosely coupled from both a design-time and build-time perspective.

Loose design time coupling improves developer productivity by ensuring that changes to one module don’t require changes to other modules. Loose build-time coupling accelerates the deployment pipeline by ensuring that changes to one module don’t require other modules to be built and tests.

### Designing a microservice architecture

If you choose to use the microservice architecture, you must design the [service architecture](https://microservices.io/post/architecture/2023/10/17/assemblage-step-3-defining-a-service-architecture.html). The [Assemblage design process](https://microservices.io/post/architecture/2023/02/09/assemblage-architecture-definition-process.html) is a great way to design a service architecture. It uses the dark energy and dark matter forces to shape the service architecture.

## Need help with accelerating software delivery?

I’m available to help your organization improve agility and competitiveness through better software architecture: training workshops, architecture reviews, etc.

[Learn more about how I can help](https://chrisrichardson.net/)
