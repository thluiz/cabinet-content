---
created: 2024-04-17T11:17:30 (UTC -03:00)
tags: []
source: https://medium.com/design-microservices-architecture-with-patterns/microservices-killer-modular-monolithic-architecture-ac83814f6862
author: Mehmet Ozkaya
---

# Microservices Killer: Modular Monolithic Architecture | by Mehmet Ozkaya | Design Microservices Architecture with Patterns & Principles | Medium

> ## Excerpt
> In this article, we are going to learn Modular Monolithic Architecture and Best Practices when designing any software architecture for our projects. By this article, we are going to learn Modular…

---
[

![Mehmet Ozkaya](https://miro.medium.com/v2/resize:fill:88:88/1*pgjuS7q21HmvfnpB9LMxVw.png)



](https://medium.com/@mehmetozkaya?source=post_page-----ac83814f6862--------------------------------)[

![Design Microservices Architecture with Patterns & Principles](https://miro.medium.com/v2/resize:fill:48:48/1*-6BunGLdw_vgthL5WdFRmQ.jpeg)



](https://medium.com/design-microservices-architecture-with-patterns?source=post_page-----ac83814f6862--------------------------------)

In this article, we are going to learn **Modular Monolithic Architecture** and **Best Practices** when designing any software architecture for our projects.

![](https://miro.medium.com/v2/resize:fit:875/1*hehKueW8579G9s0kQ4nFjw.png)

Modular Monolithic Architecture

By this article, we are going to learn **Modular Monolithic Architecture**, **Benefits** and **Challenges** of **Modular Monolithic Architecture** and **Design** our **E-Commerce** **application** with **Modular Monolithic Architecture.**

![](https://miro.medium.com/v2/resize:fit:875/0*DcSNDT0EoJ60UNBx.png)

[**I have just published a new course — Design Microservices Architecture with Patterns & Principles.**](https://p2mvzd5ccntyrms4fhrpgxbuuq0gguxl.lambda-url.us-east-2.on.aws/)

## Architecture Design Journey

There are many approaches and patterns that evolved over decades of software development, and all have their own benefits and challenges.

![](https://miro.medium.com/v2/resize:fit:875/1*qNx2Uo9pmhqTbmDskCc_1Q.png)

Architecture Design Journey

[**In the last article, we have learned and designed e-commerce application with Clean Architecture style**.](https://medium.com/design-microservices-architecture-with-patterns/design-clean-architecture-for-e-commerce-applications-with-step-by-step-297b331cdf41) Now we will focus on how it can be better isolate layers with Modular Monolithic Architecture in order to provide isolation.

-   **Why we are learning Modular Monolithic Architecture ?**

Because [**according to our learning flow**](https://medium.com/design-microservices-architecture-with-patterns/software-architecture-way-of-learning-for-microservices-527c992b5b85), we ask ourselves

-   What’s wrong with this current architecture ?
-   How can we improve the current design ?

With these questions we created a [**problem about Clean architecture**](https://medium.com/design-microservices-architecture-with-patterns/the-problem-with-clean-architecture-vertical-slices-111537c0ffcb) and try to solve this problem during the article. **Problem is Lack of Agility of New Features, Split Agile Teams:**

![](https://miro.medium.com/v2/resize:fit:875/1*aWVeaWmQiXJonq2Dza9yNQ.png)

And as a solution we have decided to learn **Modular Monolithic Architecture** with the **Dependency Rule principle**. The idea is provide **better isolation** and **develop new features in encapsulated modules** in our current architecture. For that reason, we will learn

-   Modular Monolithic Architecture
-   When to use Modular Monolithic Architecture
-   Benefits and Challenges of Modular Monolithic Architecture

## What is Modular Monolithic Architecture ?

Modular Monolithic Architecture is a software architecture that combines the **benefits of modular design** with the **simplicity** of a **monolithic** architecture. It involves dividing the system into a set of **loosely-coupled modules**, each with a well-defined boundary and explicit dependencies on other modules.

The modular monolithic architecture **divided our application logic into modules** and each module will be **independent** and **isolated**. Then, each module should have its own business logic — and, if necessary, its database or schema.

![](https://miro.medium.com/v2/resize:fit:866/1*2rHuVEn-Jv4yKX1tp-rMzA.png)

Modular Monolithic Architecture

Also **each module** can **follow** their **own logical separations**, for example in the picture above, every **module** follows **layered architecture** style, but also they **can follow clean architecture** inside the module when organize logical layers. In that way you can build and modify the layers of each module without affecting the others.

**Modular Monolith architecture** breaks up the code into **independent modules**, and each module **encapsulates** their **own features** needed in your application. Each module only connect to other modules that specifically provides services that needs to it.

In Modular Monolith architecture, we **still build and deploy a single application** which is same with **traditional Monolith architecture**, but we build application with **breaking up the code into independent modules** for each of the features needed in our application. Modules are represents **Bounded Context** of our application domain and In Modules, we group features of **Domain contexts**. By this way, we reduces the dependencies of a module and we can develop or modify a module without it effecting other modules.

## Vertical Slices with Modular Monolithic Architecture

[As you remember that when we learning **Vertical Slice architecture**](https://medium.com/design-microservices-architecture-with-patterns/the-problem-with-clean-architecture-vertical-slices-111537c0ffcb), We said that instead of using layered architecture with **horizontal logical layers**, we can organize our code across **vertical slices of business functionality.**

These **slices** are determined **based on business demands**, rather than enforced by technical constraints. If we **organize** our **codes** as **vertical slices**, when we add or change a feature in an application, we can update the same set of layers as before, but this time our **changes** are **scoped** to the **area of business concern** not technical logical layers.

![](https://miro.medium.com/v2/resize:fit:758/1*cPBLVFOGwnruMwhxyPSB4w.png)

Vertical Slices

So with the **Modular monolith architecture**, We can organize our codes as a **vertical slices**, and as our system continues to grow, organizing our code around of **business functionalities** into **Modules**.

And this modules can be a **potential microservices** when need to independently deployed and scale in the future refactoring's our architecture. By this way, we can **gradually refactor** our architecture to the **microservice** **architecture** rather than jumping it from the beginning.

## Design E-Commerce App with Modular Monolithic Architecture

Let me give an example use cases of **vertical slices** with Modular monolith architecture:

-   **list products**
-   **add to basket**
-   **checkout order use cases.**

And these use case required to touch for all layers in our Monolithic architecture. But with Modular Monolithic Architecture, we can **encapsulate** these kind of **vertical use cases into Modules**.

![](https://miro.medium.com/v2/resize:fit:875/1*IrdqWFIe14dtFEee9Sp_bg.png)

E-Commerce App with Modular Monolithic Architecture

You can see the image of **Modular Monolithic Architecture** on image that we have developed **e-commerce application**. We have **3 modules**: **Product**, **Basket** and **Order**. So we have encapsulate vertical slide use cases into modules; See that use cases:

-   **listing products = Product Module**
-   **add to basket = Basket Module**
-   **checkout order = Order Module**

But all application still in Monolithic with in single application and single deployment unit.

## Benefits of Modular Monolithic Architecture

With this Modular Monolith approach, we gain a lot of benefits like:

-   **Encapsulate Business Logic**  
    The main benefit of the modular monolith is that the business logics are encapsulated in Modules and it enables high reusability, while data remains consistent and communication patterns simple.
-   **Reusable Codes, Easy to Refactor**  
    For large development teams, developing modular components of an application will increase reusability. Modular components can be reused that can help teams establish a single source of truth. This will lead to faster and more consistent development. Also changing a module has less or no effect on other modules.
-   **Better-Organized Dependencies**  
    With modular monoliths architecture, application dependencies will be more organized and visible. This will help developers to easily assess which parts of the application require which dependencies.
-   **Less-Complex than Microservices Architecture**  
    It is easier to manage a modular monolith rather than hundreds of microservices, because Modular Monolithic comes with basic underlying infrastructure and operational costs low.
-   **Better for teams**  
    Easier for developers to work on different parts of the code. As you remember that the problem comes from Business teams that needs to be agile for new features. So with Modular Monolithic architecture, we can divide our developer teams effectively and implement business requirements with minimum affect to each other.

## Challenges of Modular Monolithic Architecture

With this Modular Monolith approach, we have also drawbacks like:

-   **Can’t diversifying technology**  
    Modular monoliths don’t provide all benefits of microservices. If you need to diversifying technology and language choices, you can’t do it with Modular Monolithic Arcihtecture. These types of polyglot technology stacks can’t use with Modular Monolithic Arcihtecture.
-   **Can’t Scale and Deploy Independently**  
    Since the application is a single unit, it can’t be scale separated parts or deploy independently like microservices. And this kind of applications has to move microservices due to reaching out scalability limits and also performance issues.

## When to use Modular Monolithic Architecture

It is important to understand that when to use Modular Monolithic Architecture:

**Strict Consistency is Mandatory Cases**  
For many companies unable to make the move to microservices, due to their database and data not appreciate for distributed architecture. For example if your application store high important data like debit on bank account, then you need strong data consistency that means your data should be correct for every time, if you got any exception you have to rollback immediately.

For this kind of projects, Modular Monolithic is best option, because Modular Monolithic comes with some of the benefits of microservices  
like more manageable dependencies and increased code reusability — without requiring the time-consuming and complicated transition to microservices.

**Modernization**  
If you already have a big complex monolithic application running, the modular monolith is the perfect architecture to help you refactor your code to get ready for a potential microservices architecture.

Instead of jumping into microservices, you can move modular monolithic without effecting your business and get benefits like speed up with a well-factored modular monolith.

**Green Field Projects**  
A modular monolith will allow you to learn your domain and pivot your architecture much faster than a microservices architecture. You won’t have to worry about things like Kubernetes and a services mesh at Day 1. Your deployment topology will be drastically simplified, and you can move those problems until much later in your delivery perhaps even once you are profitable.

## From Kelsey Hightower

Lets continue to a tweet thread **from Kelsey Hightower.**

![](https://miro.medium.com/v2/resize:fit:875/0*UzNSItW0AiFDuTO2.png)

[https://twitter.com/kelseyhightower/status/1621184564956893189](https://twitter.com/kelseyhightower/status/1621184564956893189)

![](https://miro.medium.com/v2/resize:fit:875/0*1ICwNmvREO9XIyoP.png)

[https://twitter.com/kelseyhightower/status/1621184564956893189](https://twitter.com/kelseyhightower/status/1621184564956893189)

Here you can see that Kelsey go one more step and offers that if we follow **Modular Monolithic Architecture** and build on **Serverless** and **fully managed components**, this would be prefect architecture most of software projects.

I also strongly agree with Kelsey for most of cases, but of course in some cases **Microservices are un-avoidable chooses for large-scaled applications.** The idea from Kelsey is that if you start with that setup, it can evolve and scale very easily for future requirements.

It is important to note that a Modular monolithic architecture is **not a replacement for microservices architecture**. Both approaches have their **strengths** and **weaknesses**, and the choice of architecture depends on the specific requirements of the system being built. A modular monolithic architecture can be a **good starting point** for a system that may eventually need to [**evolve into a microservices architecture**](https://medium.com/design-microservices-architecture-with-patterns/macro-services-to-nano-services-evolution-of-software-architecture-424f927b63cb) as the system grows and complexity increases.

## Design Modular Monolithic Architecture — E-Commerce App

If we design **e-commerce application** with **Modular Monolithic architecture**, you can see the image below:

![](https://miro.medium.com/v2/resize:fit:875/0*fvQbRimA8vsTX4LH.png)

Modular Monolithic Architecture

According to architecture, we can encapsulate new feature developments into a module. And every module can implement its own architecture like Layered, Onion, Clean and so on.

## Design Microservice Architecture — E-Commerce App

If we design **e-commerce application** with **Microservice architecture**, you can see the image below:

![](https://miro.medium.com/v2/resize:fit:875/0*35dHxasGNaCPXa7g.png)

Microservices Architecture

Product microservice can use NoSQL document database Shopping Cart microservice can use NoSQL key-value pair database and Order microservice can use Relational database as per microservice data storage requirements.

## What’s Next ?

-   [**Clean Architecture Design with Dependency Rule**](https://medium.com/design-microservices-architecture-with-patterns/clean-architecture-with-dependency-rule-dff96d479a60)
-   [**Macro-services to Nano-services: Evolution of Software Architecture**](https://medium.com/design-microservices-architecture-with-patterns/macro-services-to-nano-services-evolution-of-software-architecture-424f927b63cb)
-   [**Microservices Architecture: Problems and Solutions with Pattern and Principles**](https://medium.com/design-microservices-architecture-with-patterns/microservices-architecture-problems-and-solutions-with-pattern-and-principles-b673f342dc10)

## Step by Step Design Architectures w/ Course

![](https://miro.medium.com/v2/resize:fit:875/0*NfifkiydK9iF4ShK.png)

[**I have just published a new course — Design Microservices Architecture with Patterns & Principles.**](https://p2mvzd5ccntyrms4fhrpgxbuuq0gguxl.lambda-url.us-east-2.on.aws/)

In this course, we’re going to learn **how to Design Microservices Architecture** with using **Design Patterns, Principles** and the **Best Practices.** We will start with designing **Monolithic** to **Event-Driven Microservices** step by step and together using the right architecture design patterns and techniques.
