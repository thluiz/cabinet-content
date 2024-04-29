---
created: 2024-03-06T15:34:16 (UTC -03:00)
tags: [software,concept,software,coding,development,engineering,inclusive,community]
source: https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0
author: Alicia Marianne
---

# What you need to know about software requirements - DEV Community

> ## Excerpt
> Imagine that you have a client who asks you to create a web application that will provide him all the...

---
Imagine that you have a client who asks you to create a web application that will provide him all the demands of his customers. And that is all that your client will inform you, will you be able to build it? Is clear that you miss more information, and the requirements exists for it, but do you know what is a requirement? What is the consequences when we don't have them well defined? Which types of requirements exists? This article will try to give answer for some of this questions in a easy way.

## [](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#summary)Summary

-   [What is a requirement?](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#what-is-a-requirement)
-   [Why they matters and consequence of poorly-defined requirements](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#why-they-matters-and-consequence-of-poorly-defined-requirements)
-   [How we can have a good requirement](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#how-we-can-have-a-good-requirement)
-   [Levels of requirements](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#levels-of-requirements)
    -   [User requirements](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#user-requirements)
    -   [System requirements](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#system-requirements)
-   [Types of requirements](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#types-of-requirements)
    -   [Functional requirements](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#functional-requirements)
    -   [Non-functional requirements](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#non-functional-requirements)
-   [Conclusion](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#conclusion)

## [](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#what-is-a-requirement)What is a requirement?

First of all, we need to understand what is a requirement in the software development context.  
Shari Pleeger give us a good definition of it:

> System characteristic or a description of something the system is capable of accomplishing to achieve its objectives

In other words, a requirement is a statement that identifies a product or process operational, functional or design constraint or characteristic, which is **unambiguous**, **testable**, **measurable** and necessary for the acceptance of the product or process (by consumers or internal quality assurance guidelines).  
Every requirement is created based on an existent problem in the reality, the software is only one way to solve it.

## [](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#why-they-matters-and-consequence-of-poorlydefined-requirements)Why they matters and consequence of poorly-defined requirements

Now that we know what is a requirement, we must ask:

> "Why is so important for a person that works with IT know about it?"

And i will give you some of reasons of it. With a requirement we are able to:

-   Provide the basis for project planning
-   Essential for studying change requests
-   Allow risk management from the earliest stages of development
-   They are the basis for acceptance tests
-   Contract management

[![Image1](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fx13z95rmek1u9g1bhnu4.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fx13z95rmek1u9g1bhnu4.png)

What can happen if we have poorly-defined requirements?  
The first and logical consequence is the delay on the delivery of the project. When we start a project, or a sprint without having all the clear requirements for what is going to be developed, the probability of existing bugs is huge! consequently more work for the developers, the QA team, reducing the quality of life of them.  
If it appears in Production, we will have a lot of unsatisfied users, making the confidence of the product reduce what can cause the system fall into disuse. Other consequence is the cost to maintain the system.

## [](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#how-we-can-have-a-good-requirement)How we can have a good requirement

The way the requirement is written will vary from team to team, but what they should all have in common are the following three characteristics:

-   Unambiguous
-   Testable
-   Measurable

## [](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#levels-of-requirements)Levels of requirements

### [](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#user-requirements)User requirements

The user requirements are the definition of what our software needs to do. It can describe a problem that the user is facing through their necessities, expectations, restrictions, and interfaces.  
This type of requirement are written for the customers and we use natural language plus diagrams. One example for a user requirement that we can use, for a to do list app:

> The user should be able to insert a new task on the list

### [](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#system-requirements)System Requirements

System requirements are specifications that define the capabilities and constraints of the hardware, software, and network components needed for a system to operate effectively.  
These requirements are crucial for designing, implementing, and maintaining a system. They are responsible to describe what must be done, not how to do it.  
Using the same requirement that we already used, we can go into more details on it:

| User requirement | System Requirement |
| --- | --- |
| The user should be able to insert a new task on the list | \- The user should be able to add the task click on the button insert  
\- The task add should appear on the list when the user click on the button  
insert  
\- The access to the task list will be available only for logged users |

## [](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#types-of-requirements)Types of requirements

### [](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#functional-requirements)Functional requirements

Functional requirements describes the actions that one system should execute. They describe what the system should provide, how the system should react to particular inputs and how the system should behave in particular situations. They can describe as well what the system should not do.

A functional requirement can describe an user or system requirement.

### [](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#nonfunctional-requirements)Non-functional requirements

Non-functional requirements describe services or functions that the system or environment should provide. They are responsible to define the architecture of the system. We have different categories for non-functional requirements, they are:

1.  **Performance:**
    -   _Response Time:_ Specifies the maximum acceptable time for the system to respond to user input.
    -   _Throughput:_ Defines the number of transactions or operations the system can handle in a given time.
2.  **Usability:**
    -   _User Interface (UI) and User Experience (UX):_ Specifies criteria related to the design, ease of use, and overall user experience.
    -   _Accessibility:_ Ensures that the system is usable by individuals with disabilities.
3.  **Reliability:**
    -   _Availability:_ Specifies the percentage of time the system should be operational.
    -   _Fault Tolerance:_ Defines the system's ability to continue functioning in the presence of faults or errors.
4.  **Security:**
    -   _Authentication:_ Describes how users are identified and verified.
    -   _Authorization:_ Specifies the level of access granted to different users or roles.
    -   _Data Encryption:_ Requires the use of encryption to protect sensitive data.
5.  **Scalability:**
    -   _Horizontal Scalability:_ Describes how well the system can handle an increase in load by adding more hardware.
    -   _Vertical Scalability:_ Describes how well the system can handle an increase in load by increasing the capacity of existing hardware.
6.  **Compatibility:**
    -   _Hardware Compatibility:_ Ensures the system can run on specified hardware configurations.
    -   _Software Compatibility:_ Ensures the system can work with specified software components.
7.  **Maintainability:**
    -   _Modifiability:_ Describes the ease with which the system can be modified or updated.
    -   _Testability:_ Specifies the ease with which the system can be tested to ensure its correctness.
8.  **Portability:**
    -   _Platform Independence:_ Describes the ability of the system to run on different operating systems.
    -   _Data Portability:_ Ensures that data can be easily transferred between different systems.
9.  **Documentation:**
    -   _User Documentation:_ Specifies the requirements for user manuals and guides.
    -   _Technical Documentation:_ Describes the requirements for system architecture, API documentation, etc.
10.  **Regulatory and Compliance:**
    -   _Legal Requirements:_ Ensures that the system complies with relevant laws and regulations.
    -   _Industry Standards:_ Specifies adherence to industry-specific standards.

## [](https://dev.to/m4rri4nne/what-you-need-to-know-about-software-requirements-2hc0#conclusion)Conclusion

As we saw, the software requirements are really important for us and there is a bunch of different types of requirements, but the most important of all of it is to make sure that before start development, we have them well defined.  
I hope this content will be useful for you.   
If you have any questions, feel free to reach out to me!   
Bisous 💅🏼
