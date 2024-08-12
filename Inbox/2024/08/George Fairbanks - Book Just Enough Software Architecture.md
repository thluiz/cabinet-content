---
created: 2024-08-09T19:04:56 (UTC -03:00)
tags: []
source: https://www.georgefairbanks.com/book/?ref=dailydev
author: 
---

# George Fairbanks - Book: Just Enough Software Architecture

> ## Excerpt
> This is the book I wish I had when I started developing software. At
the time, there were books on languages and books on object-oriented
programming, but few books on design. Knowing the features of the C++
language does not mean you can design a good object-oriented system,
nor does knowing the Unified Modeling Language (UML) imply you can
design a good system architecture.

---
This is the book I wish I had when I started developing software. At the time, there were books on languages and books on object-oriented programming, but few books on design. Knowing the features of the C++ language does not mean you can design a good object-oriented system, nor does knowing the Unified Modeling Language (UML) imply you can design a good system architecture.

This book is different from other books about software architecture. Here is what sets it apart:

**It teaches risk-driven architecting.** There is no need for meticulous designs when risks are small, nor any excuse for sloppy designs when risks threaten your success. Many high-profile Agile software proponents suggest that some up-front design can be helpful, and this book describes a way to do just enough architecture. It avoids the “one size fits all” process tar pit with advice on how to tune your architecture and design effort based on the risks you face. The rigor of most techniques can be adjusted, from quick-and-dirty to meticulous.

**It democratizes architecture.** You may have software architects at your organization — indeed, you may be one of them. Every architect I have met wishes that all developers understood architecture. They complain that developers do not understand why constraints exist and how seemingly small changes can affect a system’s properties. This book seeks to make architecture relevant to all software developers, not just architects.

**It cultivates declarative knowledge.** There is a difference between being able to hit a tennis ball and knowing why you are able to hit it, what psychologists refer to as procedural knowledge and declarative knowledge. If you are already an expert at designing and building systems then you will have employed many of the techniques found here. This book will make you more aware of what you have been doing and provide names for the concepts. That declarative knowledge will improve your ability to mentor novice developers.

**It emphasizes the engineering.** People who design and build software systems have to do many things, including dealing with schedules, resource commitments, and stakeholder needs. Many books on software architecture already cover software development processes and organizational structures. This book, in contrast, focuses on the technical parts of software development and deals with what the developers do to ensure the system works — its engineering. It shows you how to build models and analyze architectures so that you can make principled design tradeoffs. It describes the techniques software designers use to reason about medium to large sized problems and points out where you can learn specialized techniques in more detail.

**It provides practical advice.** This book offers a practical treatment of architecture. Software architecture is a kind of software design, but design decisions influence the architecture and vice versa. What the best developers do is drill into obstacles in detail, understand them, then pop up to relate the nature of those obstacles to the architecture as a whole. The approach in this book embraces this drill-down/pop-up behavior by describing models that have various levels of abstraction, from architecture to data structure design.

## Chapters

You can download some chapters below. All the links point to the same PDF, which has [all the sample chapters in one file](https://www.georgefairbanks.com/assets/jesa/Just_Enough_Software_Architecture__Fairbanks_2010-demo.pdf). The demo chapters are [also available as an ePub e-book](https://www.georgefairbanks.com/assets/jesa/Just_Enough_Software_Architecture__Fairbanks_2010-demo.epub).

| Part I: Risk-Driven Software Architecture | Part II: Architecture Modeling |
| --- | --- |
| 1\. [Introduction](https://www.georgefairbanks.com/assets/jesa/Just_Enough_Software_Architecture__Fairbanks_2010-demo.pdf) | 6\. Engineers Use Models |
| 2\. Software Architecture | 7\. [Conceptual Model of Software Architecture](https://www.georgefairbanks.com/assets/jesa/Just_Enough_Software_Architecture__Fairbanks_2010-demo.pdf) |
| 3\. [Risk-Driven Model](https://www.georgefairbanks.com/assets/jesa/Just_Enough_Software_Architecture__Fairbanks_2010-demo.pdf) | 8\. The Domain Model |
| 4\. Example: Home Media System | 9\. The Design Model |
| 5\. Modeling Advice | 10\. The Code Model |
|  | 11\. Encapsulation and Partitioning |
|  | 12\. Model Elements |
|  | 13\. Model Relationships |
|  | 14\. Architectural Styles |
|  | 15\. Using Architecture Models |
|  | 16\. Conclusion |

## E-book and Hardback

-   **E-book:** The e-book is for sale now on [Google Play](https://www.georgefairbanks.com/e-book). It includes three DRM-free versions of the book (ePub, Mobi, and PDF formats) for $9.99.
    
-   **Hardback:** The hardback is available on [Amazon](http://www.amazon.com/dp/0984618104).
    

**Searchable:** Both [Google Books](http://books.google.com/books?id=ITsWdAAzVYMC) and [Amazon Search Inside](http://www.amazon.com/dp/0984618104) have full text, searchable versions. Also see the three chapters above.

## About this book

This book focuses on software architecture as it relates to the construction of software, and describes the techniques used to ensure that software satisfies its engineering demands. This book is largely process agnostic because the engineering techniques themselves are largely process agnostic. You will not find advice on management activities like the political responsibilities of architects, when to hold specific kinds of meetings, or how to gather requirements from stakeholders.

This book is divided into two parts. This first part introduces software architecture and the risk-driven approach. The second part helps you build up a mental conceptual model of software architecture and describes in detail the abstractions like components and connectors. What follows is short summaries of each part.

### Part I: Risk-driven software architecture

A definition of software architecture is difficult to pin down precisely, but several things about it are quite clear. Software developers, like engineers in other specialties, use abstraction and models to solve large and complex problems. Software architecture acts as the skeleton of a system, influences quality attributes, is orthogonal to functionality, and uses constraints to influence a system’s properties. Architecture is most important when the solution space is small, the failure risks are high, or you face difficult quality attribute demands. You can choose from architecture-indifferent design, architecture-focused design, or even architecture hoisting.

Risks can be used to guide you regarding which design and architecture techniques you should use and regarding how much design and architecture you should do. At its core, the risk-driven model is simple: (1) identify and prioritize risks, (2) select and apply a set of techniques, and (3) evaluate risk reduction.

To reveal the risk-driven model in practice, Chapter 4 provides an example of applying the risk-driven model to a Home Media Player system. The developers on that system have the challenges of team communication, integration of COTS components, and ensuring metadata consistency.

The first part of the book concludes with advice on using models and software architecture, including: use models to solve problems, add constraints judiciously, focus on risks, and distribute architecture skills throughout your team.

### Part II: Architecture modeling

The second part of the book helps you build a mental conceptual model of software architecture. It starts with the canonical model structure: the domain model, the design model, and the code model. The domain model corresponds to things in the real world, the design model is the design of the software you are building, and the code model corresponds to your source code. You can build additional models that show selected details, called views, and these views can be grouped into viewtypes.

Building encapsulation boundaries is a crucial skill in software architecture. Users of a component or module can often ignore how it works internally, freeing their minds to solve other hard problems. And the builders of an encapsulated component or module have the freedom to change the implementation without perturbing its users. Builders will only have that freedom if the encapsulation is effective, so you will learn techniques for ensuring that it is.

A great number of architectural abstractions and modeling techniques have been built up over the years. This book consolidates software architecture techniques found in a variety of other sources, integrating techniques emphasizing quality attributes as well as techniques emphasizing functionality. It also discusses pragmatic ways of building effective models and debugging them.

The second part of the book concludes with advice on how to use the models effectively. Any book that covered the advantages but not the pitfalls of a technology should not be trusted, so it also covers problems that you are likely to encounter. By the end of the second part, you should have built up a rich conceptual model of abstractions and relationships that will help you see software systems the way a coach sees a game.
