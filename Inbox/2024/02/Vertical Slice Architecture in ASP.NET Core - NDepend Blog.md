---
created: 2023-10-24T15:37:59 (UTC -03:00)
tags: []
source: https://blog.ndepend.com/vertical-slice-architecture-in-asp-net-core/?ref=dailydev
author: NDepend
---

# Vertical Slice Architecture in ASP.NET Core - NDepend Blog

> ## Excerpt
> Vertical Slice Architecture is a design pattern that emphasizes high-cohesion and low-coupling at feature level

---
The organization of code in a solution is a subject of frequent debates. Currently, two prominent approaches have garnered attention: **Clean Architecture** **versus Vertical Slice**.

Clean Architecture emphasizes the use of **layers**, whereas Vertical Slice centers around **features**. This distinction is visually represented in the diagram below: layers are depicted as horizontal boxes, whereas features are represented as **vertical** boxes, hence the term _“Vertical Slice”._

[![.net-vertical-slice-architecture](https://blog.ndepend.com/wp-content/uploads/net-vertical-slice-architecture.png)](https://blog.ndepend.com/wp-content/uploads/net-vertical-slice-architecture.png)

## Challenges with the Layered Approach

Historically, the layered approach preceded the slice approach. The slice approach represents an effort to address the two primary challenges that developers consistently encounter when working with layers:

**Coupling between Layers**: The main constraint in a layered architecture arises from the tight coupling between its layers. Each layer must have an in-depth knowledge of the layers it depends on. This leads to a **rigid** architecture that becomes increasingly challenging to maintain over time.

**Feature Fragmentation:** Each unit of business functionality is distributed across multiple layers. This results in the inconvenience of navigating through multiples layers when introducing new features or improving existing ones.

It’s worth highlighting that the original Clean Architecture approach, [initially introduced by Robert C. Martin in 2012](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html), is an attempt to address the first issue (Layers Coupling) through [the Dependency Inversion Principle (DIP)](https://blog.ndepend.com/solid-design-the-dependency-inversion-principle-dip/). I explain it in this post [Clean Architecture in ASP.NET Core](https://blog.ndepend.com/clean-architecture-for-asp-net-core-solution/) based on the [Jason Taylor’s Clean Architecture .NET solution template available here on github](https://github.com/jasontaylordev/CleanArchitecture). This onion diagram provides a succinct summary of how layers should be coupled according to Uncle Bob (M.Martin) :

[![Clean-Architecture-Diagram-Asp-Net](https://blog.ndepend.com/wp-content/uploads/Clean-Architecture-Diagram-Asp-Net.png)](https://blog.ndepend.com/wp-content/uploads/Clean-Architecture-Diagram-Asp-Net.png)

The Application layer is able to use all sorts of infrastructure (DB, Date, file, user profile… ) thanks to abstractions whose implementations are injected from a higher layer, here from the Web UI layer. Doing so favors flexibility and reduces the overall rigidity of the solution. Coupling inversion is the gem of Clean Architecture. Yet the Feature Fragmentation problem still remains and is addressed by Vertical Slice.

## Benefits of Vertical Slice

Vertical Slice Architecture addresses Feature Fragmentation through **Code Locality**. It emphasizes the grouping of code associated with similar functionality, ideally within a single class, file, or package. This approach resonates with human preference for **neatly packaging related elements in one cohesive unit**.

[_Nadir Badnjevic_](https://nadirbad.dev/) undertook the task of refactoring Jason Taylor’s Clean Architecture .NET solution using Vertical Slices. [Nadir’s solution is available on github here](https://github.com/nadirbad/VerticalSliceArchitecture). The domain is left untouched. For the purposes of clarity the domain is minimal and only contains mostly one value object `Colour` and two entities: `TodoItem` and `TodoList`. If I search for implementations related to `TodoItem` in both solutions here is what I get:

[![](https://blog.ndepend.com/wp-content/uploads/net-clean-architecture-vs-vertical-slices-architecture.png)](https://blog.ndepend.com/wp-content/uploads/net-clean-architecture-vs-vertical-slices-architecture.png)

If I work on the `TodoItem` feature:

-   With the Clean Architecture approach, I will touch potentially 13 namespaces spawn in 4 projects.
-   With the Vertical Slice approach, I will touch potentially 6 namespaces all contained in a single project. Actually I will mostly work within the feature namespace `Application.Features.TodoItems` that contains Queries, Commands and Controllers related to this feature. This is the beauty of code locality!

**Vertical Slice Architecture centers around the idea that artifacts that change together should be grouped together.**

## Impact of the Vertical Slice Approach

One key aspect of Vertical Slices is to **maintain slices independent from one another as much as possible**. Here is a comment on [Jimmy Bogard’s 2018 post on Vertical Slice Architecture](https://www.jimmybogard.com/vertical-slice-architecture/) that is worth reading:

> Roger Zanelato “A problem that I face with this architecture, is that sometimes you really need a Domain, Behavior or even a Query from another feature (slice), and although I prefer practicality over dogmatism, it still leaves me with a bad taste in my mouth. How do you handle this situations?”
> 
> Jimmy Bogard “Refactor the common code to some shared service/location. It’s really not different in any other architecture. **Just don’t call one feature from another**. Refactor!”

See below a dependency graph ([obtained with NDepend](https://www.ndepend.com/docs/visual-studio-dependency-graph)) of the Nadir’s solution. We can see that in the central `Features.*` namespace, `TodoItem` and `TodoList` implementations are independent from each other. This graph also shows that classes within each feature namespace are cohesive enough. This solution applies the principle of **high-cohesion low-coupling** at feature level.

[![dependency-diagram-vertical-slices](https://blog.ndepend.com/wp-content/uploads/dependency-diagram-vertical-slices.png)](https://blog.ndepend.com/wp-content/uploads/dependency-diagram-vertical-slices.png)

The features share the same domain model, else you might question if they should belong to the same solution.  Nevertheless, the features do not inherently align with uniform infrastructure; each feature operates as an autonomous entity and possesses the freedom to leverage the most appropriate technologies and best practices. This **extra flexibility** is an important benefit of applying Vertical Slice. But it results in two troubling consequences:

-   Applying Vertical Slice may lead to **code duplication** _to some extent_. This preference for duplication arises from a desire to avoid **awkward abstractions** that attempt to accommodate all scenarios without excelling in any specific one (we all know this phenomenon, aren’t we?).
-   In Clean Architecture infrastructure’s abstractions are inherently designed, albeit possibly in an awkward manner_._ Such abstractions necessarily increase **testability**. When abiding by Vertical Slices these abstractions may be absent.
-   Vertical Slice may **degrade code consistency**. Vertical Slice is sometime compared to the **microservices** architecture, except that all services live in a shared solution. While the lack of code consistency between independent services is fine, it is annoying within a single solution because this **hinders the** **principle of least surprise**. Here is another interesting comment from the [Jimmy Bogard post](https://www.jimmybogard.com/vertical-slice-architecture/) that summarizes this point:

> “This is going to lead to more code bloat, more code duplication, more confusion among developers. Especially junior developers who are already easy enough to confuse when you don’t have all these swirling options floating around. Why are we solving the same kind of problem, in the same application in multiple different ways? Why are we spending so much time on duplication of similar functionality? Monolithic applications have a lot of problems, but “this code is just too damned consistent” isn’t one of them.”

## Conclusion

In this article, we have explored the concept of vertical slice architecture, which involves dividing the code into distinct _slices_, each representing a complete feature.

We’ve also discerned the contrast between vertical slice architecture and traditional layered architectural approaches that focuses on separating business rules from infrastructural concerns.

We’ve examined the advantages and disadvantages of both architecture styles. We can now conclude that there is no one-size-fits-all approach.

-   Layers lead to more coupling and rigidity which can be really problematic in the long term.
-   Slices lead to more duplication, reduced consistency and potential testability glitches.

You must decide whether to prioritize horizontality or verticality, acknowledging that you’ll need to address specific pain points accordingly. It’s about making choices regarding coupling and dependencies that won’t limit your future options.

To learn more about the Vertical Slice approach I recommend the work of my friend [Derek Comartin (CodeOpinion) who advocates for it through comprehensive videos and articles](https://www.youtube.com/watch?v=L2Wnq0ChAIA).

___

## Note on this implementation

The dependency diagram above contain two red-arrows that reveal that `Application.Common` and `Application.Infrastructure` both rely on `Feature` which I find somewhat frustrating. By double clicking both arrows we can obtain two [coupling graphs](https://www.ndepend.com/docs/visual-studio-dependency-graph#Coupling). Each of this graph sheds light on the fact that `CsvFileBuilder` consuming `TodoItemRecord` is the culprit in both cases. IMHO `CsvFileBuilder` should be abstracted from the features implementations.

Why `Application.Common` is relying on `Feature` :

[![vertical-slice-glitch-Common](https://blog.ndepend.com/wp-content/uploads/vertical-slice-glitch-Common.png)](https://blog.ndepend.com/wp-content/uploads/vertical-slice-glitch-Common.png)

Why `Application.Infrastructure` is relying `Feature` :

[![vertical-slice-glitch-Infrastructure](https://blog.ndepend.com/wp-content/uploads/vertical-slice-glitch-Infrastructure.png)](https://blog.ndepend.com/wp-content/uploads/vertical-slice-glitch-Infrastructure.png)

[![ndepend](https://blog.ndepend.com/wp-content/uploads/Logo_Big-150x150.png)](https://blog.ndepend.com/author/psmacchia/)

Do you like reading our blog posts about advanced .NET and C# topics? If so, you'll enjoy the experience of analyzing your codebase with NDepend. Simply download the NDepend trial from [https://www.ndepend.com/download](https://www.ndepend.com/download), and in a few minutes from now, you'll gain a whole new perspective on your .NET code.
