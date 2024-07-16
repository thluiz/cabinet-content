---
created: 2024-07-08T16:45:32 (UTC -03:00)
tags: [programming,systemdeisgn,softwaredevelopment,development,software,coding,development,engineering,inclusive,community]
source: https://dev.to/somadevtoo/system-design-basics-caching-4fge?context=digest
author: 
---

# System Design Basics - Caching - DEV Community

> ## Excerpt
> The ultimate guide on caching for System Design interviews

---
_Disclosure: This post includes affiliate links; I may receive compensation if you purchase products or services from the different links provided in this article._

[![System Design Basics - Caching](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fex1vo79v2zmn77ujuyk7.png)](https://bit.ly/3pMiO8g)  
image\_credit - [DesignGuru.io](https://bit.ly/3pMiO8g)

Hello friends, Caching is not just an important topic for System design interviews, its also technique in software development, enabling faster data retrieval, reducing load times, and enhancing user experience.

For developers, mastering [caching concepts](https://dev.to/somadevtoo/9-caching-strategies-for-system-design-interviews-369g) is crucial as it can significantly optimize application performance and scalability.

In the past, I have talked about common system design questions like [API Gateway vs Load Balancer](https://dev.to/somadevtoo/difference-between-api-gateway-and-load-balancer-in-system-design-54dd) and [Horizontal vs Vertical Scaling](https://dev.to/somadevtoo/horizontal-scaling-vs-vertical-scaling-in-system-design-3n09), [Forward proxy vs reverse proxy](https://dev.to/somadevtoo/difference-between-forward-proxy-and-reverse-proxy-in-system-design-54g5) as well common [System Design problems](https://dev.to/somadevtoo/top-50-system-design-interview-questions-for-2024-5dbk) and in this article we will explore the fundamentals of caching in system design and learn different caching strategies that are essential knowledge for technical interviews.

It's also one of the [essential System design topics for interview](https://medium.com/javarevisited/top-10-system-design-concepts-every-programmer-should-learn-54375d8557a6) and you must prepare it well.

In this article, you will learn ten essential caching concepts, ranging from client-side and server-side strategies to more advanced techniques like distributed caching and cache replacement policies

So what are we waiting for? let's start

By the way, if you are preparing for System design interviews and want to learn System Design in depth then you can also checkout sites like [**ByteByteGo**](https://bit.ly/3P3eqMN), [**Design Guru**](https://bit.ly/3pMiO8g), [**Exponent**](https://bit.ly/3cNF0vw), [**Educative**](https://bit.ly/3Mnh6UR) and [**Udemy**](https://bit.ly/3vFNPid) which have many great System design courses and a System design interview template like this which you can use to answer any System Design question.

[![how to answer system design question](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F23jeu6ppweg5zt5prvhx.jpg)](https://bit.ly/3pMiO8g)

If you need more choices, you can also see this list of [best System Deisgn courses](https://www.linkedin.com/pulse/10-best-system-design-courses-beginners-experienced-2023-soma-sharma/), [books](https://www.linkedin.com/pulse/8-best-system-design-books-programmers-developers-soma-sharma/), and [websites](https://javarevisited.blogspot.com/2022/08/top-7-websites-to-learn-system-design.html)

_P.S. Keep reading until the end. I have a free bonus for you._

## [](https://dev.to/somadevtoo/system-design-basics-caching-4fge?context=digest#what-is-caching-which-data-to-cache-where-to-cache)What is Caching? Which data to Cache? Where to Cache?

While designing distributed system, caching should be strategically placed to optimize performance, reduce latency, and minimize load on backend services.

Caching can be implemented at multiple layers like

1.  **Client-Side Cache**  
    This involves storing frequently accessed data on the client device, reducing the need for repeated requests to the server. It is effective for data that doesn't change frequently and can significantly improve user experience by reducing latency.
    
2.  **Edge Cache (Content Delivery Network - CDN)**  
    CDNs cache content at the edge nodes closest to the end-users, which helps in delivering static content like images, videos, and stylesheets faster by serving them from geographically distributed servers.
    
3.  **Application-Level Cache**  
    This includes in-memory caches such as Redis or Memcached within the application layer. These caches store results of expensive database queries, session data, and other frequently accessed data to reduce the load on the database and improve application response times.
    
4.  **Database Cache**  
    Techniques such as query caching in the database layer store the results of frequent queries. This reduces the number of read operations on the database and speeds up data retrieval.
    
5.  **Distributed Cache**  
    In a distributed system, a distributed cache spans multiple nodes to provide high availability and scalability. It ensures that the cached data is consistent across the distributed environment and can handle the high throughput required by large-scale systems.
    

When designing a caching strategy, it's crucial to determine what data to cache by analyzing usage patterns, data volatility, and access frequency.

Implementing an appropriate cache eviction policy (such as LRU - Least Recently Used, or TTL - Time to Live) ensures that stale data is purged, maintaining the cache's relevance.

Moreover, considering consistency models and cache invalidation strategies is vital to ensure that cached data remains accurate and up-to-date across the system.

And, here is a nice diagram on caching from [DesignGuru.io](https://bit.ly/3pMiO8g) to illustrate what I just said.

[![System Design Caching cheat shet](https://res.cloudinary.com/practicaldev/image/fetch/s--kfHr85i3--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://miro.medium.com/v2/resize:fit:700/1%2AW9YvFj-rA65Y6xx2gcR52w.png)](https://bit.ly/3pMiO8g)

___

### [](https://dev.to/somadevtoo/system-design-basics-caching-4fge?context=digest#10-caching-basics-for-system-design-interview)10 Caching Basics for System Design Interview

Here are 10 essential caching related basics and concepts every programmer must know before going for any System design interview.

### [](https://dev.to/somadevtoo/system-design-basics-caching-4fge?context=digest#1-clientside-caching)1) client-side caching

**Client-side caching** is a fundamental technique where data is stored on the user's device to minimize server requests and improve load times. Two primary methods include:

-   **Browser Cache:** Stores resources like CSS, JavaScript, and images locally to reduce page load times on subsequent visits.
-   **Service Workers:** Enable offline access by caching responses, allowing applications to function without an internet connection.

In short:

-   browser cache: stores CSS, js, images to reduce load time\\
    -   service workers: enable offline access by caching response

Here is how client side caching looks like:

[![client side caching](https://res.cloudinary.com/practicaldev/image/fetch/s--LmeHNJmR--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://miro.medium.com/v2/resize:fit:700/1%2AVMfbWSd1dkBGDweiA79HnA.png)](https://medium.com/javarevisited/is-system-design-interview-roadmap-on-designguru-io-worth-it-review-55dc74f0d270)

___

### [](https://dev.to/somadevtoo/system-design-basics-caching-4fge?context=digest#2-serverside-caching)2) server-side caching

This is another type of caching which involves storing data on the server to expedite response times for user requests.

Key strategies include:

-   **Page Caching:** Saves entire web pages, allowing faster delivery on subsequent requests .
-   **Fragment Caching:**  
    Caches specific parts of a page, such as sidebars or navigation bars, to enhance loading efficiency.
    
-   **Object Caching:**  
    Stores expensive query results to prevent repeated calculations
    

In short:

-   page caching: cache the entire web page
    -   fragment caching: cache page components like sidebars, navigation bar\\
    -   object caching: cache expensive query results

Here is how server side caching looks like:

[![server side caching](https://res.cloudinary.com/practicaldev/image/fetch/s--SDOd5wAP--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://miro.medium.com/v2/resize:fit:700/1%2A7Sv4Mr_qObXQibZvxa4K0g.png)](https://bit.ly/3P3eqMN)

image\_credit --- ByteByteGo

___

### [](https://dev.to/somadevtoo/system-design-basics-caching-4fge?context=digest#3-database-caching)3) Database caching

**Database caching** is crucial for reducing database load and improving query performance. Important techniques include:

-   **Query Caching:**  
    Stores the results of database queries to quickly serve repeat requests.
    
-   **Row Level Caching:**  
    Caches frequently accessed rows to avoid repeated database fetches.
    

In short:

-   query caching: cache db query results to reduce load
    -   row level caching: cache popular rows to avoid repeated fetches

Here is an example of database caching on AWS:

[![database caching](https://res.cloudinary.com/practicaldev/image/fetch/s---4OeizDf--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://miro.medium.com/v2/resize:fit:700/1%2AmpdH_1Da3ru7KVEVIAtNvg.png)](https://dev.to/somadevtoo/what-is-database-sharding-how-to-scale-your-database-g89-temp-slug-1946377)

___

### [](https://dev.to/somadevtoo/system-design-basics-caching-4fge?context=digest#4-applicationlevel-caching)4) application-level caching

**Application-level caching** focuses on caching within the application to reduce computation and data retrieval times. Strategies include:

-   **Data Caching:** Stores specific data points or entire datasets for quick access.
-   **Computational Caching:** Caches the results of expensive computations to avoid repeated processing.

In short:

-   data caching: cache specific data points or entire datasets\\
    -   computational caching: cache expensive computation results to avoid recalculation

[![application-level caching](https://res.cloudinary.com/practicaldev/image/fetch/s--tExBccnp--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://miro.medium.com/v2/resize:fit:700/1%2Ak97saJYmMOxh7HGhmZ00_w.png)](https://www.linkedin.com/pulse/8-best-places-prepare-system-design-interview-2024-soma-sharma-ug4ic/)

___

### [](https://dev.to/somadevtoo/system-design-basics-caching-4fge?context=digest#5-distributed-caching)5) Distributed caching

**Distributed caching** enhances scalability by spreading cache data across multiple servers, allowing high availability and fault tolerance.

In short, this type of caching just spreads cache across many servers for scalability

Here is how a distributed cache with Redis looks like:

[![distributed cache with Redis](https://res.cloudinary.com/practicaldev/image/fetch/s--zT13VyEw--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://miro.medium.com/v2/resize:fit:700/1%2AVsnnRuObjjLTzVJgTbS9fA.jpeg)](https://res.cloudinary.com/practicaldev/image/fetch/s--zT13VyEw--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://miro.medium.com/v2/resize:fit:700/1%2AVsnnRuObjjLTzVJgTbS9fA.jpeg)

___

### [](https://dev.to/somadevtoo/system-design-basics-caching-4fge?context=digest#6-cdn)6) CDN

**Content Delivery Networks (CDNs)** are used to cache static files close to users via edge servers, significantly reducing latency and speeding up content delivery.

In short, CDN store static files near users using edge servers for low latency

Also, here is a nice diagram on how CDN Works by DeisgnGuru.io

[![how CDN Works](https://res.cloudinary.com/practicaldev/image/fetch/s--SuIC_S67--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://miro.medium.com/v2/resize:fit:700/1%2AfxlwIZcQvwDEI3trHYiq1g.png)](https://bit.ly/3pMiO8g)

___

### [](https://dev.to/somadevtoo/system-design-basics-caching-4fge?context=digest#7-cache-replacement-policies)7) cache replacement policies

**Cache replacement policies** determine how caches handle data eviction. Common policies include:

-   **Least Recently Used (LRU):** Evicts the least recently accessed items first.
-   **Most Recently Used (MRU):** Evicts the most recently accessed items first.
-   **Least Frequently Used (LFU):** Evicts items that are accessed least often.

In short:

```
- LRU: removes the least recently accessed items first\
- MRU: removes the most recently accessed items first\
- LFU: removes items that are accessed least often
```

[![cache replacement policies](https://res.cloudinary.com/practicaldev/image/fetch/s--eU3ox27f--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://miro.medium.com/v2/resize:fit:700/1%2AgpfJfsdt9n3y6L_gZFBVlg.jpeg)](https://www.linkedin.com/pulse/top-5-free-system-design-interview-courses-tutorials-2024-soma-sharma-v2prc/)

___

### [](https://dev.to/somadevtoo/system-design-basics-caching-4fge?context=digest#8-hierarchical-caching)8) hierarchical caching

**Hierarchical caching** involves multiple cache levels (e.g., L1, L2) to balance speed and storage capacity. This model is quit popular on CPU.

In short:

-   caching at many levels (L1, L2 caches) for speed and capacity

[![L1 and L2 Cache](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F0lno2ea5b123rsm2h589.png)](https://www.linkedin.com/pulse/top-20-system-design-interview-questions-answers-soma-sharma-g0pqc/)

___

### [](https://dev.to/somadevtoo/system-design-basics-caching-4fge?context=digest#9-cache-invalidation)9) cache invalidation

**Cache invalidation** ensures that stale data is removed from the cache. Methods include:

-   **Time-to-Live (TTL):** Sets an expiry time for cached data.
-   **Event-based Invalidation:** Triggers invalidation based on specific events or conditions.
-   **Manual Invalidation:** Allows developers to manually update the cache using tools.

In short:

```
- TTL: set expiry time\
- event based: invalidate based on events or conditions\
- manual: update cache using tools
```

Here is a nice System design cheat sheet about cache invalidation methods by [DesignGuru.io](https://bit.ly/3pMiO8g) to understand this concept better:

[![cache invalidation strategies](https://res.cloudinary.com/practicaldev/image/fetch/s--HtEEp61W--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://miro.medium.com/v2/resize:fit:700/1%2APaghEy2WoWeBkx3pKmOK-Q.jpeg)](https://bit.ly/3pMiO8g)

___

### [](https://dev.to/somadevtoo/system-design-basics-caching-4fge?context=digest#10-caching-patterns)10) caching patterns

Finally, **caching patterns** are strategies for synchronizing cache with the database. Common patterns include:

-   **Write-through:** Writes data to both the cache and the database simultaneously.
-   **Write-behind:** Writes data to the cache immediately and to the database asynchronously.
-   **Write-around:** Directly writes data to the database, bypassing the cache to avoid cache misses on subsequent reads.

In short:

```
- write-through: data is written to the cache and the database at once\
- write-behind: data is written to the cache and asynchronously to database\
- write-around: data is written directly to the database, bypassing the cache
```

Here is another great diagram to understand various caching strategies, courtesy [DesignGuru.io](https://bit.ly/3pMiO8g), one of the best place to learn System Design.

[![caching patterns](https://res.cloudinary.com/practicaldev/image/fetch/s--_4b7wgx3--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://miro.medium.com/v2/resize:fit:700/1%2ABrLOJOUnzJ5UCI4UqgWSZQ.png)](https://bit.ly/3pMiO8g)

### [](https://dev.to/somadevtoo/system-design-basics-caching-4fge?context=digest#conclusion)Conclusion

That's all about **10 essential Cache related concepts for System deisgn interview.** Caching can improve the performance and scalability of your application. So use it carefully. Understanding and implementing these caching concepts can significantly enhance application performance, scalability, and user satisfaction.

**Bonus**\\  
As promised, here is the bonus for you, a free book. I just found a new free book to learn Distributed System Design, you can also read it here on Microsoft --- [https://info.microsoft.com/rs/157-GQE-382/images/EN-CNTNT-eBook-DesigningDistributedSystems.pdf](https://info.microsoft.com/rs/157-GQE-382/images/EN-CNTNT-eBook-DesigningDistributedSystems.pdf)

[![](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fso5r1wv8x95i74nz6p89.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fso5r1wv8x95i74nz6p89.png)
