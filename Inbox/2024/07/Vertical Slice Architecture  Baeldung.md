---
created: 2024-07-06T18:24:19 (UTC -03:00)
tags: []
source: https://www.baeldung.com/java-vertical-slice-architecture?ref=dailydev
author: 
---

# Vertical Slice Architecture | Baeldung

> ## Excerpt
> A quick tutorial on the Vertical Slice Architecture along with several practical implementations.

---
## 1\. Overview[](https://www.baeldung.com/java-vertical-slice-architecture?ref=dailydev#overview)

In this tutorial, we’ll learn about Vertical Slice Architecture and how it attempts to solve the issues associated with layered designs. We’ll discuss structuring the code by business capabilities, which leads to a more expressive codebase organized into loosely coupled and cohesive modules. After that, we’ll explore this approach from a Domain Driven Design (DDD) perspective and discuss its flexibility.

## 2\. Layered Architecture[](https://www.baeldung.com/java-vertical-slice-architecture?ref=dailydev#layered-architecture)

Before exploring Vertical Slice Architecture, let’s first review the key features of its main counterpart, Layered Architecture. Layered designs are popular and widely used, with variations like [Hexagonal](https://www.baeldung.com/hexagonal-architecture-ddd-spring), Onion, Ports and Adapters, and [Clean Architecture](https://www.baeldung.com/spring-boot-clean-architecture).

**A Layered Architecture uses a series of stacked or concentric layers that protect the domain logic against external components and factors**. A key characteristic of these architectures is that all the dependencies point inwards, towards the domain:![](https://www.baeldung.com/wp-content/uploads/2024/06/hex-arch.png)

### 2.1. Grouping Components by Technical Concerns[](https://www.baeldung.com/java-vertical-slice-architecture?ref=dailydev#1-grouping-components-by-technical-concerns)

The layered approach solely focuses on grouping components by technical concerns, instead of their business capabilities.

For this article’s code examples, let’s assume we’re building the backend application of a blogging website. The application will support the following use cases:

-   Authors can publish and edit articles
-   Authors can see a dashboard with their article stats
-   Readers can read, like, and add comments to articles
-   Readers receive notifications with article recommendations

For example, **our package names reflect technical layers without conveying the true purpose of the project**:

[![](https://www.baeldung.com/wp-content/uploads/2024/06/hexagonal_ide_structure-1.png)](https://www.baeldung.com/wp-content/uploads/2024/06/hexagonal_ide_structure-1.png)

### 2.2. High Coupling[](https://www.baeldung.com/java-vertical-slice-architecture?ref=dailydev#2-high-coupling)

Moreover, **re-using the same domain services for unrelated business use cases can lead to tight coupling**. For instance, the _ArticleService_ currently depends on:

-   _ArticleRepository –_ to query the database
-   _UserService –_ to fetch data about the article authors
-   _RecommendationService –_ to update reader recommendations when a new article is published
-   _CommentService_ – to manage article comments

Consequently, we risk interfering with unrelated flows when we add or modify a use case. Furthermore, this highly coupled approach often leads to cluttered tests full of mocks.

### 2.3. Low Cohesion[](https://www.baeldung.com/java-vertical-slice-architecture?ref=dailydev#3-low-cohesion)

Finally, this code structure tends to have low cohesion within components. **Since the code for a business use case is scattered in various packages across the project, any minor change will require us to modify files from each layer.**

Let’s add a _slug_ field to the _Article_ entity. If we want to allow the clients to use the new field to query the database, we’ll have to change many files throughout the layers:

[![](https://www.baeldung.com/wp-content/uploads/2024/06/hexagonal-cohesion.png)](https://www.baeldung.com/wp-content/uploads/2024/06/hexagonal-cohesion.png)

Even a simple modification causes changes in almost every package of our application. The fact that the classes that change together don’t live together indicates a low cohesion.

Vertical Slice Architecture was developed to tackle some issues associated with Layered Architecture by organizing the code by business capabilities. **Following this approach, our components mirror business use cases and span over multiple layers.**

As a result, instead of grouping all the controllers in a common package, they will be moved to packages associated with their respective slices:

[![](https://www.baeldung.com/wp-content/uploads/2024/06/vsa_diagram.png)](https://www.baeldung.com/wp-content/uploads/2024/06/vsa_diagram.png)

Additionally, **we can group related use cases into cohesive slices that align with the business domains.** Let’s reorganize our example with the _author, reader,_ and _recommendation_ domains:

[![](https://www.baeldung.com/wp-content/uploads/2024/06/vsa_ide_structure-1.png)](https://www.baeldung.com/wp-content/uploads/2024/06/vsa_ide_structure-1.png)

Dividing the project into vertical slices allows us to use the default, package-private access modifiers for most of the classes. This ensures that **unexpected dependencies don’t cross the domain boundary**.

Lastly, it enables someone unfamiliar with the codebase to understand what the application does by looking at the file structure. **Robert C. Martin, the author of Clean Code, refers to this as “[Screaming Architecture](https://blog.cleancoder.com/uncle-bob/2011/09/30/Screaming-Architecture.html)“**. He argues that a software project’s design should communicate its purpose clearly, much like architectural blueprints of a building reveal its function.

## 4\. Coupling and Cohesion[](https://www.baeldung.com/java-vertical-slice-architecture?ref=dailydev#coupling-and-cohesion)

As previously discussed, opting for the Vertical Slice Architecture over the Onion Architecture offers improved management of coupling and cohesion.

### 4.1. Looser Coupling Through Application Events[](https://www.baeldung.com/java-vertical-slice-architecture?ref=dailydev#1-looser-coupling-through-application-events)

Instead of eliminating the coupling between slices, let’s focus on defining the right interfaces for communication across boundaries. **Using application events is a powerful technique that allows us to maintain loose coupling while facilitating cross-boundary interactions**.

In the Layered Architecture approach unrelated services depend on each other to complete a business feature. Specifically, _ArticleService_ depends on _RecommendationService_ to notify it about new articles. Instead, the _recommendation_ flow can be executed asynchronously, and react to the main flow by listening to an application event.

Since we use the Spring Framework for our code example, let’s publish a [Spring Event](https://www.baeldung.com/spring-events) when a new article is created:

```java
@Component class CreateArticleUseCase { private final ApplicationEventPublisher eventPublisher; // constructor void createArticle(CreateArticleRequest article) { saveToDatabase(article); var event = new ArticleCreatedEvent(article.slug(), article.name(), article.category()); eventPublisher.publishEvent(event); } private void saveToDatabase(CreateArticleRequest aticle) { /* ... */ } // ... } }
```

Now, the _SendArticleRecommendationUseCase_ can use an _@EventListener_ to react to _ArticleCreatedEvent_s and execute its logic:

```java
@Component class SendArticleRecommendationUseCase { @EventListener void onArticleRecommendation(ArticleCreatedEvent article) { findTopicFollowers(article.name(), article.category()); .forEach(follower -> sendArticleViaEmail(article.slug(), article.name(), follower)); } private void sendArticleViaEmail(String slug, String name, TopicFollower follower) { // ... } private List<TopicFollower> findTopicFollowers(String articleName, String topic) { // ... } record TopicFollower(Long userId, String email, String name) {} }
```

As we can see, the modules operate independently and don’t directly depend on each other. Additionally, any components interested in newly created articles only need to listen for the _ArticleCreatedEvent_.

### 4.2. Higher Cohesion[](https://www.baeldung.com/java-vertical-slice-architecture?ref=dailydev#2-higher-cohesion)

Finding the right boundaries results in cohesive slices and use cases. **A use case class should typically have a single public method and a single reason to change, adhering to the [Single Responsibility Principle](https://www.baeldung.com/java-single-responsibility-principle)**.

Let’s add a _slug_ field to the _Article_ class in our Vertical Slice Architecture and create an endpoint to find articles by _slug_. This time, the changes are scoped to a single package. We’ll create a _SearchArticleUseCase_ that uses _JdbcClient_ to query the database and return a projection of an _Article_. As a result, we’ll only modify two files in one package:

[![](https://www.baeldung.com/wp-content/uploads/2024/06/vsa-git-status.png)](https://www.baeldung.com/wp-content/uploads/2024/06/vsa-git-status.png)

We created one use case and modified the _ReaderController_ to expose the new endpoint. The fact that both files are located in the same package indicates higher cohesion within the project.

## 5\. Design Flexibility[](https://www.baeldung.com/java-vertical-slice-architecture?ref=dailydev#design-flexibility)

Vertical Slice Architecture allows us to customize approaches for each component, and **determine the most effective way to organize the code for each use case**. In other words, we can utilize various tools, patterns, or paradigms without enforcing a specific coding style or dependencies across the entire application.

Moreover, this flexibility facilitates approaches such as [Domain-Driven Design (DDD)](https://www.baeldung.com/java-modules-ddd-bounded-contexts) and [CQRS](https://www.baeldung.com/cqrs-event-sourcing-java#2-cqrs). While not mandatory, they are well-suited for a vertically sliced application.

### 5.1. Modeling the Domain With DDD[](https://www.baeldung.com/java-vertical-slice-architecture?ref=dailydev#1-modeling-the-domain-with-ddd)

Domain-Driven Design is an approach that emphasizes modeling software based on the core business domain and its logic. In DDD, the code must use terms and language familiar to business people and clients, to align technical and business perspectives.

In Vertical Slice Architecture, we might encounter the problem of code duplication between use cases. For slices that expand, we can decide to extract general business rules and use DDD to create a domain model, specific to them:

[![](https://www.baeldung.com/wp-content/uploads/2024/06/vsa_domain.png)](https://www.baeldung.com/wp-content/uploads/2024/06/vsa_domain.png)

Moreover, **DDD uses Bounded Contexts to define specific boundaries**, **ensuring clear distinctions between different parts of the system.**

Let’s revisit a project that follows the layered approach. We’ll notice that we interact with the system’s users through objects like _UserService, UserRepository,_ and the _User_ entity. In contrast, in a vertically-sliced project, the concept of a user varies across different bounded contexts. **Each slice has its own representation of a user, referring to them as a “reader”, an “author”, or a “topic follower”**, reflecting the specific role they play within that context.

### 5.2. Bypassing the Domain for Simple Use Cases[](https://www.baeldung.com/java-vertical-slice-architecture?ref=dailydev#2-bypassing-the-domain-for-simple-use-cases)

Another drawback of strictly following a layered architecture is that it can result in methods that simply pass calls to the next layer, without adding value. This is also known as the “middle man” antipattern, and it leads to tightly coupled layers.

For example, when finding an article by _slug_, the controller calls the service, which then calls the repository. Even though the service doesn’t add any value in this case, the strict rules of layered architecture prevent us from bypassing the domain and directly accessing the persistence layer.

In contrast, a vertically sliced application offers the flexibility to choose which layers are needed for each specific use case. This allows us to **bypass the domain layer for simple use cases, and directly query the database for projections:**

[![](https://www.baeldung.com/wp-content/uploads/2024/06/vsa_use_cases-1.png)](https://www.baeldung.com/wp-content/uploads/2024/06/vsa_use_cases-1.png)

Let’s simplify the use case of viewing an article by _slug_, using the Vertical Slice Architecture to bypass the domain layer:

```java
@Component class ViewArticleUseCase { private static final String FIND_BY_SLUG_SQL = """ SELECT id, name, slug, content, authorid FROM articles WHERE slug = ? """; private final JdbcClient jdbcClient; // constructor public Optional<ViewArticleProjection> view(String slug) { return jdbcClient.sql(FIND_BY_SLUG_SQL) .param(slug) .query(this::mapArticleProjection) .optional(); } record ViewArticleProjection(String name, String slug, String content, Long authorId) { } private ViewArticleProjection mapArticleProjection(ResultSet rs, int rowNum) throws SQLException { // ... } }
```

As we can see, _ViewArticleUseCase_ directly queries the database using a _JdbcClient_. Moreover, it defines its own projection of an article, instead of reusing a common DTO, which would’ve coupled this use case to other components. As a result, **unrelated use cases aren’t forced into the same structure and we eliminate unnecessary dependencies**.

## 6\. Conclusion[](https://www.baeldung.com/java-vertical-slice-architecture?ref=dailydev#conclusion)

In this article, we’ve learned about Vertical Slice Architecture and compared it to its Layered Architecture counterpart. We learned how to create cohesive components and avoid coupling between unrelated business use cases.

We discussed bounded contexts and how they help us define different projections specific to a part of the system. Finally, we discovered that this approach offers increased flexibility when designing each vertical slice.

As always, the complete source code can be found [over on GitHub](https://github.com/eugenp/tutorials/tree/master/patterns-modules/vertical-slice-architecture).
