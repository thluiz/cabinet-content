---
created: 2023-11-02T18:32:50 (UTC -03:00)
tags: []
source: https://www.kamilgrzybek.com/blog/posts/modular-monolith-primer
author: Kamil Grzybek
---

# Modular Monolith: A Primer — Kamil Grzybek

> ## Excerpt
> Kamil Grzybek Personal Site

---
## Modular Monolith: A Primer

-   ![](https://www.kamilgrzybek.com/images/blog/posts/modular-monolith-primer/Modular_Monolith_a_Primer-825x510.jpg)

## Introduction

Many years have passed since the rise of the popularity of microservice architecture and it is still one of the main topics discussed in the context of the system architecture. The popularity of cloud solutions, containerization and advanced tools supporting the development and maintenance of distributed systems (such as Kubernetes) is even more conducive to this phenomenon.

Observing what is happening in the community, companies and during conversations with programmers, it can be concluded that most of the new projects are implemented using the microservice architecture. Moreover, some legacy systems are also moving towards this approach.

Ok, the subject of the post is _Modular Monolith_ and I dwell on microservices, the question is why? Namely, because I think that as an IT industry we have made a false start adopting microservice architecture to such an extent. Instead of **focusing on architectural drivers**, we believed that microservices are medicine for all the evil that sits in monolithic applications. If you have participated in the development of a system that consists of more than one deployment unit, you already know that this is not the case. **Each architecture has its pros and cons** - microservices are no exception. They solve some problems by generating others in return.

With this entry, I would like to start a series of articles on the architecture of _Modular Monolith_. I do it for several reasons.

First of all, I would like to refute the myth that you cannot make a high-class system in monolithic architecture. Secondly, I would like to dispel doubts about the definition of this architecture and its appearance - many people interpret it differently. Thirdly, I treat this series of posts as an extension and addition to my implementation of [Modular Monolith with DDD](https://github.com/kgrzybek/modular-monolith-with-ddd) architecture, which I shared a few months ago on GitHub and which was very well received (1k stars a month after publication).

In this introductory post, I will focus on the definition of a _Modular Monolith_ architecture.

## What is Modular Monolith?

I always try to be precise when I talk or write about technical and business issues, especially when it comes to architecture. I believe that a clear and coherent message is very important. That is why I would like to clearly define what the architecture of the Modular Monolith means to me and how I perceive it.

Let’s start with the simpler concept, what is _Monolith_?

### Monolith

[Wikipedia](https://en.wikipedia.org/wiki/Monolithic_architecture) describes _“monolithic architecture”_ in terms of building construction and not computer science as follows:

> Monolithic architecture describes buildings which are carved, cast or excavated from a single piece of material, historically from rock.

In terms of computer science, building is the system and the material is our executable code. So in _Monolith Architecture_, our **system consists of exactly one piece of executable code** and nothing more.

Let’s see 2 technical definitions: first one about [Monolith System](https://en.wikipedia.org/wiki/Monolithic_system):

> A software system is called “monolithic” if it has a monolithic architecture, in which functionally distinguishable aspects (for example data input and output, data processing, error handling, and the user interface) are all interwoven, rather than containing architecturally separate components.

Second one about [Monolithic Architecture](https://whatis.techtarget.com/definition/monolithic-architecture):

> A monolithic architecture is the traditional unified model for the design of a software program. Monolithic, in this context, means composed all in one piece. Monolithic software is designed to be self-contained; components of the program are interconnected and interdependent rather than loosely coupled as is the case with modular software programs

These 2 definitions above (one of the first results in Google) have 2 shared assumptions.

First, they define that this architecture assumes that all parts of the system form one deployment unit - I will agree with that. The second shared assumption of these definitions is that they assume a lack of modularity in such architecture and I will definitely disagree with that. The phrases _“interwoven, rather than containing architecturally separate components”_ and _“components of the program are interconnected and interdependent rather than loosely coupled”_ very negatively characterize this architecture, assuming that everything is mixed in them. It may be so, but it doesn’t have to be. It is not the ultimate attribute of the Monolith.

To sum up, _Monolith_ is nothing more than a **system that has exactly one deployment unit**. No less no more.

### Modularization

I’ve defined what _Monolith_ means, let’s get to second aspect: _Modularity_.

What does it mean that something is modular according to the [English Dictionary](https://dictionary.cambridge.org/dictionary/english/modular)?

> Consisting of separate parts that, when combined, form a complete whole/made from a set of separate parts that can be joined together to form a larger object

and _Modularization_ itself:

> The design or production of something in separate sections

Because it is a general definition, it is not enough for the programming world. Let’s use a more specific technical one about [Modular programming](https://en.wikipedia.org/wiki/Modular_programming):

> Modular programming is a software design technique that emphasizes separating the functionality of a program into independent, interchangeable modules, such that each contains everything necessary to execute only one aspect of the desired functionality. A module interface expresses the elements that are provided and required by the module. The elements defined in the interface are detectable by other modules. The implementation contains the working code that corresponds to the elements declared in the interface.

Several important issues have been raised here. In order to have modular architecture, you must have modules and these modules:

-   a) must be independent and interchangeable and
-   b) must have everything necessary to provide desired functionality and
-   c) must have defined interface

Let’s see what these assumptions mean.

#### Module must be independent and interchangeable

For the module to meet these assumptions, as the name implies, it should be independent. Of course, it is impossible for it to be completely independent because then it means that it does not integrate with other modules. The module will always depend on something, but dependencies should be kept to a minimum. According to the principle: [Loose Coupling, Strong Cohesion](http://www.kamilgrzybek.com/design/grasp-explained/).

In the diagram below on the left we have a module that has a lot of dependencies and you can definitely not say that it is independent. On the other hand, on the right, the situation is the opposite - the module contains a minimum of dependencies and they are more loose, it is finally more independent:

![Module independence](https://www.kamilgrzybek.com/images/blog/posts/modular-monolith-primer/Module_independence-1024x420.jpg)

_Module independence_

However, the number of dependencies is just one measure of how well our module is independent. The second measure is how strong the dependency is. In other words, do we call it very often using multiple methods or occasionally using one or a few methods?

![Strong/Weak dependency](https://www.kamilgrzybek.com/images/blog/posts/modular-monolith-primer/Module_independence_strongweak-1024x420.jpg)

_Strong/Weak dependency_

In the first case, it is possible that we have defined the boundaries of our modules incorrectly and we should **merge both modules if they are closely related**:

![Modules merged](https://www.kamilgrzybek.com/images/blog/posts/modular-monolith-primer/Module_indpendence_merge-1024x420.jpg)

_Modules merged_

The last attribute affecting the independence of the module is **the frequency of changes of the modules on which it depends on**. As you can guess - the less often they are changed, the more the module is independent. On the other hand, if changes are frequent - we must change our module often and it loses its independence:

![Modules stability_](https://www.kamilgrzybek.com/images/blog/posts/modular-monolith-primer/Module_independence_stability-1024x474.jpg)

_Module stability_

To sum up, the module’s independence is determined by three main factors:

-   number of dependencies
-   strength of dependenies
-   stability of the modules on which the module depends on

#### Module must have everything necessary to provide desired functionality

The module is a very overloaded word and can be used in many contexts with different meanings. A common case here is to call logical layers as modules, e.g. GUI module, application logic module, database access module. Yes, in this context these are also modules but **they provide technical, not business functionality**.

Thinking about a module in a technical context, only technical changes cause exactly one module to change:

![Technical modules and technical change](https://www.kamilgrzybek.com/images/blog/posts/modular-monolith-primer/TechnicalModules_technicalChange-1024x474.jpg)

_Technical modules and technical change_

Adding or changing business functionality usually goes through all layers **causing changes in each technical module**:

![Technical modules - new/change business feature](https://www.kamilgrzybek.com/images/blog/posts/modular-monolith-primer/TechnicalModule_FeatureChange-1024x421.jpg)

_Technical modules - new/change business feature_

The question we have to ask ourselves is: do we more often make changes related to the technical part of our system or changes in business functionality? In my opinion - definitely more often the latter. We rarely exchange the database access layer, logging library or GUI framework. For this reason, the module in the _Modular Monolith_ is a business module that **is able to fully provide a set of desired features**. This kind of design is called _“Vertical Slices”_ and we group these slices in the module:

![Business modules and vertical slices](https://www.kamilgrzybek.com/images/blog/posts/modular-monolith-primer/BusinessModules_VerticalSlices-1-1024x403.jpg)

_Business modules and vertical slices_

In this way, frequent changes **affect only one module** - it becomes more independent, autonomous and is able to provide functionality by itself.

#### Module must have defined interface

The last attribute of modularity is a **well-defined interface**. We can’t talk about modular architecture if our modules don’t have a _Contract_:

![Modules without contract (interface)](https://www.kamilgrzybek.com/images/blog/posts/modular-monolith-primer/Modules_without_contract-1024x548.jpg)

_Modules without contract (interface)_

A _Contract_ is what we make available outside so it is very important. It is an _“entry point”_ to our module. Good _Contract_ should be unambiguous and contain only what clients of a given contract need. We should keep it stable (to not break our clients) and hide everything else behind it ([Encapsulation](https://en.wikipedia.org/wiki/Encapsulation_(computer_programming))):

![Modules with contract](https://www.kamilgrzybek.com/images/blog/posts/modular-monolith-primer/Modules_with_contract-1024x548.jpg)

_Modules with contract_

As you can see in the diagram above, the contract of our module can take different forms. Sometimes it is some kind of [facade](https://en.wikipedia.org/wiki/Facade_pattern) for synchronous calls (e.g. public method or REST service), sometimes it can be an published event for asynchronous communication. In any case, everything that we share outside **becomes the public API of the module**. Therefore, **encapsulation is an inseparable element of modularity**.

## Summary

1.  Monolith is a system that has exactly one deployment unit.
2.  Monolith architecture does not imply that the system is poor designed, not modular or bad. It does not say anything about quality.
3.  Modular Monolith architecture is a explicit name for a Monolith system designed in a modular way.
4.  To achieve a high level of modularization each module must be independent, has everything necessary to provide desired functionality (separation by business area), encapsulated and have a well-defined interface/contract.

In the next post I will discuss the pros and cons of _Modular Monolith_ architecture comparing it to the microservices.

## Additional resources

1.  [Modular Monoliths Video - Simon Brown](https://www.youtube.com/watch?v=5OjqD-ow8GE).
2.  [Majestic Modular Monliths - Axel Fontaine](https://www.youtube.com/watch?v=BOvxJaklcr0).
3.  [Modular programming - Wikipedia](https://en.wikipedia.org/wiki/Modular_programming).
4.  [Monolithic application - Wikipedia](https://en.wikipedia.org/wiki/Monolithic_application).
5.  [Modular Monolith with DDD - GitHub repository](https://github.com/kgrzybek/modular-monolith-with-ddd).
6.  [Vertical Slice Architecture - Jimmy Bogard](https://jimmybogard.com/vertical-slice-architecture/).

Image credits: [Magnasoma](https://magnasoma.com/)
