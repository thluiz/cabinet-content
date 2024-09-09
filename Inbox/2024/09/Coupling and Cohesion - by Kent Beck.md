---
created: 2024-08-12T14:08:59 (UTC -03:00)
tags: []
source: https://tidyfirst.substack.com/p/coupling-and-cohesion?ref=dailydev
author: Kent Beck
---

# Coupling and Cohesion - by Kent Beck

> ## Excerpt
> This is from 2009, a mere 4 years into my study of software design.

---
> This is from 2009, a mere 4 years into my study of software design. Note that the description of cohesion is nowhere near as crisp as in _Tidy First?_. Being able to discuss cohesion clearly was what took me the next 10 years. The only way to be able to describe something well is to describe it badly 100 times.

I just finished a week of training at SKB Kontur in Ekaterinburg, Russia. We covered a lot of ground during the week–TDD, social principles of development, habits for agility. We ended the week talking about software design. During design day we tried to identify the best-designed software there (I recommend this exercise). Because the day was so experiential, we didn’t get to talk about all the design concepts I wanted to discuss. In particular, coupling and cohesion play a central role in the value of software design. As a kind of parting gift to the great group of programmers in the workshop (and because it will bug me if I don’t write it down), here is an introduction to coupling and cohesion.

Yourdon and Constantine in their classic [Structured Design](https://web.archive.org/web/20140415191114/http://www.amazon.com/Structured-Design-Fundamentals-Discipline-Computer/dp/0138544719) identify cost minimization as the goal of software design. The cost of software is dominated by the cost of maintenance, the cost of maintenance is dominated by the cost of changes the ripple through the system, and effective software design minimizes the chance of changes propagating. Changes that touch a single element cost less and are more predictable than changes to one element that require changes to two more, and then three… The expected cost of change can be reduced by paying careful attention to two factors: coupling between elements and cohesion within elements.

(Software design also has a role in increasing or accelerating revenue, but revenue isn’t directly connected to coupling and cohesion so I will deal with this role later.)

> Describing Net Present Value is my eventual response to this.

Two elements are coupled to the degree that changes to one tend to require changes in another. For example, parallel class hierarchies are coupled if a class is added to one, because the other hierarchy will also require another class. Two systems communicating via a wire protocol are coupled with respect to protocol changes–if one system requires a change to the protocol the other will need to change as well. Coupling between elements is a conductor of change.

[

![Coupling propogates change](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd6131c95-ebe3-4497-9dda-1facf0fd8836_223x110.jpeg "coupling-and-cohesion-1")

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd6131c95-ebe3-4497-9dda-1facf0fd8836_223x110.jpeg)

Coupling propagates change

I talk about coupling (and cohesion) in terms of particular changes. This is not the standard definition. Coupling is generally described as a static property: these two elements are temporally coupled, for example, if the calling sequence between them is constrained. These static properties are only potential cost. If nothing ever triggers the coupling, the cost is never realized.

A system could be coupled to a particular vendor’s database by using vendor-specific features. A change to the database would require changes to the system. If the database never changes, though, then the coupling remains potential. Evaluating the cost of coupling precisely requires evaluating the set of changes that are actually required of the system. This can only be done a posteriori. Evaluating the cost prospectively requires estimating the probabilities of the kinds of change that would propagate across a relationship.

The relationship between coupling and change cost goes both ways. Changes that are likely to be expensive are less likely to be chosen. Breaking a coupling can open up the possibility for new kinds of change.

There is much more to say about the various kinds of coupling and the kinds of change that propagate across them, but that detail will have to await another post. The fundamental concept is that elements in a design should not be coupled with respect to the changes that actually take place. This keeps the cost of a change contained.

Coupling measures the spread of a change across elements. Cohesion measures the cost of a change within an element. An element is cohesive to the degree that the entire element changes when the system needs to change.

An element can lack cohesion either by being too large or too small. An element that is too small, solving only part of a problem, will have to be coupled to elements solving the other parts of the problem. Changing the solution will require changing all the elements. An element that solves several problems will only be partly changed. This is riskier and more expensive than changing a whole element because first you need to figure out what part of the element should be changed and then you need to prove that the unchanged part of the element is truly unchanged. Cohesive elements, replaced in total, don’t incur these costs.

[

![Cohesion](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F018b3f6c-f600-44b3-b896-a95c657f716e_398x149.jpeg "coupling-and-cohesion002")

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F018b3f6c-f600-44b3-b896-a95c657f716e_398x149.jpeg)

Cohesion

(The strategy of isolating change is a way of inducing cohesion before making a change, for example extracting the part of a method that needs to change into its own method before making the change.)

## **Balance**

One of the things about design that makes it such a joy is that it requires balance. If elements are too large, each change will be more expensive than it needs to be. If elements are too small, changes will ripple across elements. And optimizing the design takes place against the backdrop of an unpredictable stream of changes.

Elements that are too large tend to multiply the cost of change: N \* C. Change that ripples across elements can potentially be much more expensive: C ^ N. (This math is simplistic and not intended to be taken literally.) This suggests that the most care should be spent on reducing coupling. This is a bit puzzling as my practice is generally to make more smaller pieces. Perhaps I’m just confident of my ability reduce coupling.

One challenge in design is to cheaply reduce coupling. If one element can be insulated from a likely change in another at reasonable cost, then it’s worth doing sooner rather than later. Breaking other forms of coupling will be more expensive and might be better defered until just before a triggering change. Again, it’s the imprecision of this analysis that makes design fun.

The unpredictability of changes renders it impossible to statically determine the “best” design for a system. There will always be some changes that ripple through the system. I speculate that the number of elements changed per change to the system follows a power law distribution. Careful attention to coupling can reduce the slope of the line describing the number of changes but cannot eliminate the distribution. This hypothesis needs experimental backing.

## **Further topics**

When I write about design, I notice that I tend to assume prerequisites that I haven’t yet written about and point to corollaries that I likewise haven’t covered yet. Here is a list of topics I should write about implied by this post:

-   Isolate change
    
-   Design is beneficially relating elements
    
-   Forms of coupling
    
-   Cost and benefit of design (in particular the points of diminishing and reversing returns)
    
-   Design fitness and its evolution
    
-   Design to maximize revenue
    
-   Design timing
    

### Subscribe to Software Design: Tidy First?

Software design is an exercise in human relationships. So are all the other techniques we use to develop software. How can we geeks get better at technique as one way of getting better at relationships?
