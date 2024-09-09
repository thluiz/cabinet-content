---
created: 2024-08-11T22:50:05 (UTC -03:00)
tags: []
source: https://medium.com/simpplr-technology/microservices-architecture-the-hard-parts-trap-of-distributed-monolith-7d707858aa32
author: Naresh Waswani
---

# Microservices Architecture, The Hard Parts : Trap of Distributed Monolith | by Naresh Waswani | Simpplr Technology | Medium

> ## Excerpt
> When conversing with seasoned Senior Software Engineers who leverage Microservices Architecture for their product development, they frequently express initial enthusiasm. However, over time, they…

---
[

![Naresh Waswani](https://miro.medium.com/v2/resize:fill:88:88/1*n_sKd7nbYusb_Pqf1Clwsw.jpeg)



](https://medium.com/@waswani?source=post_page-----7d707858aa32--------------------------------)[

![Simpplr Technology](https://miro.medium.com/v2/resize:fill:48:48/1*ibd1kQQO66cJJ8ynbb6oxw.png)



](https://medium.com/simpplr-technology?source=post_page-----7d707858aa32--------------------------------)

When conversing with seasoned Senior Software Engineers who leverage Microservices Architecture for their product development, they frequently express initial enthusiasm. However, over time, they often encounter escalating difficulties and challenges. Here’s a narrative that illustrates this prevalent journey —

Previously, I felt a surge of excitement when our Architect proposed implementing our product using Microservices Architecture. Importantly, **this decision** **wasn’t driven by a desire to boost our resumes**; rather, there were clear, compelling reasons for us to adopt Microservices —

1.  We knew our business domain well
2.  We had different software characteristics for the services we wanted to offer — some needed 4 9s of availability and some were OK with 2 9s
3.  Scalability and Elasticity requirements were different for individual services
4.  High rate of change in some of the services — independent deployability capability needed for these services
5.  Plus a few more…

In conclusion, the initial decision appeared sound. However, as time passed, we encountered **operational hurdles regarding performance and latency**.

Moreover, **releasing new features became a challenge**; each change necessitated adjustments across multiple services, mandating simultaneous releases. **_Upon reflection, we recognized that we had fallen into the pitfalls of — Distributed Microservices Architecture._**

## A Distributed Monolith occurs when an architecture exhibits unfavorable traits of both Distributed System Architecture and Monolithic Architecture. It entails excessive interdependencies among services to fulfill a business use case and mandates simultaneous deployment of multiple services to introduce new features or address complex bugs.

![](https://miro.medium.com/v2/resize:fit:651/1*4NxfvflT8ntLtXXfvX-qfw.jpeg)

Distributed Microservices — Too much of dependencies across services

In this blog post, I aim to outline the factors that can lead to falling into this trap and explore potential preventative measures. Let’s go —

## Factor #1 — Inadequate Service Boundary

Opting for a service model aligned with business domains or sub-domains is advisable. This approach ensures that a feature request triggers changes solely within the relevant domain service, reducing reliance on other services. Consequently, teams gain autonomy, enabling them to determine when to deploy the feature without the need for coordination with other teams or dependencies.

However, when service boundaries are poorly defined and overlap across multiple services, there’s a significant likelihood that all these services must be enhanced to support a business feature. This necessitates increased communication between services and substantial coordination efforts to release such a feature. Consequently, the principle of independent deployability in microservices is compromised. Moreover, if synchronous communication exists between these services, it can result in performance and latency issues, ultimately impacting overall throughput.

Therefore, it’s imperative to diligently adhere to Domain-Driven Design principles to accurately delineate service boundaries and construct services accordingly.

## Factor #2 — Too Much of Synchronous Communication

When microservices communicate synchronously, they encounter temporal coupling, implying that both services must be accessible to handle the request. If the relied-upon service is unavailable or responds slowly, it will affect either the availability of the feature or the overall performance of the service.

Furthermore, even if the dependent service responds adequately, there’s still an impact on the overall performance and latency due to the network hop involved in communication. Given the inherent unreliability of networks, it’s crucial to fortify your service design to withstand failures.

Additionally, you’ve compromised the scalability software characteristic of the service. Both involved services now require matching scalability software characteristics; otherwise, it affects the availability of the feature. If one service can scale efficiently but the dependent service cannot, it presents a challenging situation to address.

Therefore, aim to minimize communication between microservices. Whenever feasible, prioritize asynchronous communication over synchronous methods.

**_To learn more about Synchronous Vs Asynchronous Communication between Microservices, take a look at this blog —_**

## Factor #3 — Too Many Fine Grained Services

Designing microservices solely around entities or creating a microservice per use case may not be the optimal approach, as it could result in excessive interdependencies, performance degradation, and operational challenges.

In such a setup, even minor feature enhancements could necessitate modifications across multiple services, resulting in deployment coordination issues and potentially impacting the overall performance of the feature.

If you’re uncertain about the granularity of a service boundary, it’s advisable to err on the side of being coarse-grained rather than having numerous fine-grained services. It’s always possible to carve out a new microservice from a coarse-grained one if the need arises.

## Factor #4 — Service Coupling

Indeed, service coupling can occur in microservices if not implemented properly.

**A classic example of this is a shared database —** Service A requires data from Service B, but Service B Team, preoccupied with its feature delivery, grants access to its database to Service A. This action creates a technical debt item, scheduled for resolution later (though we know such tasks often linger unresolved). While both services may seem content, they’ve inadvertently introduced a point of coupling.

If Service A decides to modify the database schema, perhaps by consolidating two tables into one or altering column names, Service B must also adapt to these changes. Consequently, the independent deployability principle of microservices is compromised. Service A cannot unilaterally enact such alterations without consulting Service B, as they could potentially disrupt the functionality of Service B.

Therefore, exert significant effort to avoid introducing points of coupling between services.

## Factor #5 — Shared code without Versioning

In the context of microservices, it’s advised to refrain from code sharing. However, there may arise scenarios where you’ll need to share utility packages among services. If these shared libraries are modified without proper versioning, each change could necessitate testing all microservices consuming them, just to ensure compatibility and avoid potential disruptions.

In summary, increased testing across services and heightened coordination for redeployment can significantly impact the productivity of development teams.

If sharing code across microservices is necessary, it’s vital to establish a robust versioning strategy. Additionally, implement processes to ensure that consuming services transition to the latest version of these libraries gradually. This approach facilitates seamless modifications to shared code, enabling timely releases as required.

**_To learn more about using Library or a Service for sharing code between Microservices, take a look at this blog —_**

I trust this blog has provided ample insights into how to avoid common pitfalls in Microservices architecture.

Hope you enjoyed reading this blog. Do share this blog with your friends if this has helped you in any way.

Follow me here on **Medium** or on [**LinkedIn**](https://www.linkedin.com/in/nwaswani/) to read more of such blogs on Microservices, Event Driven Architecture, Kubernetes, DevOps and many more.

Happy Blogging…Cheers!!!

#Microservices #MicroservicesAntiPatterns #DistributedMicroservices
