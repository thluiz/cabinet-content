---
created: 2024-05-03T20:15:16 (UTC -03:00)
tags: []
source: https://levelup.gitconnected.com/system-design-interview-question-design-url-shortener-c3278a99fc35
author: Hayk Simonyan
---

# System Design Interview Question: Design URL Shortener | by Hayk Simonyan | Level Up Coding

> ## Excerpt
> Designing a scalable and secure URL shortener service like TinyURL or Bitly, planning the system architecture, API design, and key strategies for high availability and security.

---
[

![Hayk Simonyan](https://miro.medium.com/v2/resize:fill:88:88/1*shFuXtkBu9viAG261Fg2iA.png)



](https://hayk-simonyan.medium.com/?source=post_page-----c3278a99fc35--------------------------------)[

![Level Up Coding](https://miro.medium.com/v2/resize:fill:48:48/1*5D9oYBd58pyjMkV_5-zXXQ.jpeg)



](https://levelup.gitconnected.com/?source=post_page-----c3278a99fc35--------------------------------)

![](https://miro.medium.com/v2/resize:fit:1400/1*xOHz3T_iSShM2rRIT2bljA.png)

This is a system design interview question, which is to design a URL shortener tool like TinyURL or Bitly from the ground up. We'll cover everything from design requirements, architecture, and component design to scaling for high-performance and security best practices.

## **Defining the Scope:** Functional and Non-functional Requirements

First, we need to define this system's functional and non-functional requirements.

We have two functional requirements:

1.  When given a long URL, we have to create a short URL
2.  When given a short URL, we must redirect the user to the long URL.

![](https://miro.medium.com/v2/resize:fit:1400/1*jAM1f_jvXWxmPVRjiweF0g.png)

The non-functional requirements for our service are to prioritize **low latency** (fast responses) and **high availability** (always online).

![](https://miro.medium.com/v2/resize:fit:1400/1*jNP-rIMTYsFG6wq8Hu4cnA.png)

## Clarifying Questions **for the Interviewer**

Here are some clarifying questions that we might ask the interviewer to ensure we're aligned on the system's scale:

-   **Usage:** Estimate the number of URLs we'll need to create each second (let's say it's 1000).
-   **Characters:** Can we use numbers and letters (alphanumeric) or other symbols? (We'll assume alphanumeric characters).
-   **Uniqueness:** Do we generate a unique short URL each time, even if multiple users submit the same long URL? (For this design, we will).

## Estimation: Data Math

With this information, we need to calculate **how long the URL should be** after shortening. Of course, we're trying to make it as short as possible, but we need to keep in mind the number of created URLs each year.

![](https://miro.medium.com/v2/resize:fit:1400/1*KSImBkGNZ7RzwWQRZ22uNA.png)

First, let's estimate the number of unique URLs needed for a significant period. A common approach is to plan for at least a few years of operation. For simplicity, let's calculate for 10 years.

-   **Seconds in a Year:** Seconds in a year: 60 seconds/minute × 60 minutes/hour × 24 hours/day × 365 days/year = 31.536 million seconds
-   **Total Seconds in 10 Years:** 31.536 million × 10= 315.36 million seconds
-   **Total URLs in 10 Years:** 1,000 × 315.36 million = 315.36 billion unique URLs

This means our database will handle 1000 writes per second and every year, we will have **1000** × **60** × **60** × **24** × **365 = 31.5B URLs created**. If we assume that we will typically have 10x more reads than writes, that means that we'll be getting more than **10** × **1000 = 10000 reads per second**.

Now, we need to figure out how many characters will give us enough unique short URLs for our 10-year volume. Given that the character set size is 62, the length of the URL identifier can be calculated as follows:

-   62¹ = 62 unique URLs (1 character)
-   62² = 3844 unique URLs (2 characters)
-   …and so on.

![](https://miro.medium.com/v2/resize:fit:1400/1*v92u6EyCjrSdC2G9dfdkzQ.png)

Continuing this calculation, we see that 62⁷ (around 3.5 trillion) is the first value larger than our projected 315 billion URLs needed.

Therefore, to support our projected growth over the next ten years, our shortened URLs need a minimum of 7 characters.

## **High-Level Architecture**

Our system will have these key components:

**Users:** We will have users who send their long URLs to be shortened or who send us a short URL, and we need to redirect them to the long URL.

**Load Balancer:** All of these requests go through a load balancer, which distributes the traffic across multiple web server instances to ensure high availability and balance of the load.

**Web Servers:** These server replicas are responsible for handling the incoming HTTP requests.

**URL Shortener Service:** We also need a URL shortener service that contains the core logic for generating short URLs, storing URL mappings, and retrieving the original URLs for redirection.

**Database:** Stores the connection between short URLs and their long counterparts. Before designing the database, we need to consider the potential storage requirements for our shortened URLs.

Each URL will include the unique identifier (roughly 7 bytes), the long URL (up to 100 bytes), and user metadata (estimated at 500 bytes). This means we need up to 1000 bytes per URL. Over ten years, with our projected volume, this translates to approximately 315 terabytes of data.

![](https://miro.medium.com/v2/resize:fit:1400/1*mMbzPSeZtYyVyjcmi2rqVw.png)

Before moving any further, let's first think about the API Design of our single web server.

## API Design

Let's define the basic API operations for our service. As stated in our functional requirements, we will use a REST API, and we need two endpoints.

**1\. Create a Short URL (POST** `**/urls**`**)**

Input: JSON payload containing the long URL `{“longUrl”: “[https://example.com/very-long-url](https://example.com/very-long-url)"}`

Output: JSON payload with the shortened URL `{“shortUrl”: “[https://tiny.url/3ad32p9](https://tiny.url/3ad32p9)"}` and `201 Created` status code.

If the request is invalid or malformed, we'll return `400 Bad Request` response, and in case the requested URL already exists in the system, we'll respond with `409 Conflict`.

**2\. Redirect to Long URL (GET** `**/urls/{shortUrlId}**`**)**

Input: `shortUrlId` path parameter

Output: Response with `301 Moved Permanently` and the newly created short URL in the response body as JSON `{ "shortUrl": "https://tiny.url/3ad32p9" }`

![](https://miro.medium.com/v2/resize:fit:1400/1*wNufw9wtsv-tV0G20eDuAA.png)

301 status code instructs the browser to cache the information, which means that the next time the user types in the short URL, the browser will automatically redirect to the long URL without reaching the server.

However, if you want to track each request's analytics and ensure it goes through your system, use the 302 status code instead.

## Database: Storing the shortened URLs

The next part is the Database layer. This layer stores the mapping between short URLs and long URLs. It should be optimized for fast read and write operations.

The schema could be simple: a primary key for the short URL id and fields for the long URL and possibly creation metadata.

```
<span id="0d8c" data-selectable-paragraph=""><span>{</span><br>  <span>"shortUrlId"</span><span>:</span> <span>"3ad32p9"</span><span>,</span><br>  <span>"longUrl"</span><span>:</span> <span>"https://example.com/very-long-url"</span><span>,</span><br>  <span>"creationDate"</span><span>:</span> <span>"2024-03-08T12:00:00Z"</span><span>,</span><br>  <span>"userId"</span><span>:</span> <span>"user123"</span><span>,</span><br>  <span>"clicks"</span><span>:</span> <span>1023</span><span>,</span><br>  <span>"metadata"</span><span>:</span> <span>{</span><br>    <span>"title"</span><span>:</span> <span>"Example Web Page"</span><span>,</span><br>    <span>"tags"</span><span>:</span> <span>[</span><span>"example"</span><span>,</span> <span>"web"</span><span>,</span> <span>"url shortener"</span><span>]</span><span>,</span><br>    <span>"expireDate"</span><span>:</span> <span>"2025-03-08T12:00:00Z"</span><br>  <span>}</span><span>,</span><br>  <span>"isActive"</span><span>:</span> <span><span>true</span></span><br><span>}</span></span>
```

Here, we mostly need to think about the reads we will get to the database. If we typically get 1000 Writes per second, then we can assume that we at least get 10–100.000 Reads per second.

In this case, we need to use a high-performance database that supports fast reads and writes. That means we need to go with a NoSQL database (like a document store like MongoDB, a wide-column store like Cassandra, or a key-value store like DynamoDB) since they're specifically designed to handle a large amount of scale.

![](https://miro.medium.com/v2/resize:fit:1400/1*qDll9-tFr2I2OxhWO3wSHg.png)

It won't be ACID compliant, but we are not concerned about it because we're not going to do a bunch of JOINs or complex queries, and we don't need those ACID rules and atomic transactions here.

## URL Shortener Service

One of the core parts of this system is the URL Shortener Service. This service generates short URLs without introducing collisions between different long URLs pointing to the same short URL.

There are various methods to implement this service; here are some of them:

-   Hashing: Generate a hash of the long URL and use a part of it as the identifier. However, hashing can lead to collisions.
-   Auto-increment IDs: Use a database auto-increment ID and encode it to a short string. This ensures uniqueness but might be predictable.
-   Custom Algorithm: Design a custom algorithm to generate unique IDs with a mix of characters to ensure uniqueness and non-predictability.

For example, to avoid collisions, there's a very simple way—we can generate all the possible keys with 7 characters and store them in a database as keys, where the key is the generated URL and the value is boolean; if it's true, then this URL is already used, and if it's false, then we can use this URL to create a new map.

So, whenever a user requests to generate a key, we can find a URL from this database that is not currently in use and map it to the long URL in the request body.

Do you think we will use an SQL or NoSQL database in this case? Consider a scenario where two users asked to shorten their long URLs, and they both got mapped to the same key from this database.

![](https://miro.medium.com/v2/resize:fit:1400/1*b0c8CnuESVxCE2-Ux7QPqA.png)

In this case, the URL will be mapped to one of their requests, and the other one will be broken. So, we will use SQL because it comes with ACID properties. We can create a transaction for each session here to perform these steps in isolation, and we won't have such issues in this case.

## High Availability & Low Latency

Our current system obviously won't be able to handle 1000 URLs per second of traffic.

![](https://miro.medium.com/v2/resize:fit:1400/1*eN5OY1eSi9SnGHqgVgF9bg.png)

## Caching

To make this more scalable, we first need a Caching layer (e.g., Redis) to cache popular URLs for quick retrieval in an in-memory cache with tools like Redis.

Given that some URLs might be accessed much more frequently than others, we need an eviction policy that prioritizes frequently accessed items. Two suitable caching eviction policies for this scenario are:

-   The **LRU eviction policy** removes the least recently accessed items first. This policy is effective for a URL shortener because it ensures that the cache keeps the most recently and frequently accessed URLs available, which can significantly reduce access times for popular links.
-   Or a Time To Live TTL-based eviction policy assigns a fixed time-to-live to each cache entry. Once an entry's TTL expires, it is removed from the cache. This policy can be useful for URL shortening services to handle the caching of URLs that are only popular for a short duration.

![](https://miro.medium.com/v2/resize:fit:1400/1*oDV7pndeZqmTmRrthz5a3Q.png)

TTL can also help us automatically refresh the cache contents and can be combined with other policies like LRU for more effective cache management.

## Database Scaling: Combining Replication and Sharding

We need to implement replication and sharding strategies to ensure that the database supports high availability, fault tolerance, and scalability.

Considering that our 7-character set has 3.5T unique URLs, we can use key-based sharding to distribute the URL records evenly across multiple shards.

Let's say we distribute it across 3 shards, and each shard will store approximately 1.16T URLs. This ensures scalability as the number of URLs grows.

We can also implement master-slave replication within each shard to ensure high availability and fault tolerance. This setup allows for quick failover and recovery in case of node failures.

![](https://miro.medium.com/v2/resize:fit:1400/1*68H1bEHS3eYotv9dBy9hmg.png)

Optionally, if the service targets a global user base, we can consider geographical sharding and replication to minimize latency and improve user experience across different regions.

This combination allows the service to handle a large volume of URL shortenings and redirections with minimal downtime and fast response times.

![](https://miro.medium.com/v2/resize:fit:1400/1*XHmu-wQLQwyKAHztn3zIeA.png)

## Security Considerations

Here are some security considerations to keep in mind for our service:

-   **Input Validation:** Let's sanitize every URL a user submits. We must check for valid protocols (HTTP, HTTPS, etc.) and ensure the URL is well-formed. This helps prevent injection attacks.
-   **Rate Limiting:** We can protect our service from DDoS attacks by limiting the number of requests a single source can make. Consider a token bucket algorithm for this.
-   **Monitoring and Logging:** A robust logging system (like the ELK stack) is essential. It lets us analyze logs to find bottlenecks and suspicious activity and ensure overall system health.
-   **Obfuscation:** We don't want easily predicted short URLs. To deter attackers from guessing valid links, let's add randomness to our generation algorithm.
-   **Link Expiration:** Optionally, we can consider allowing users to set expiration dates on their shortened URLs. This limits the lifespan of potentially malicious links.
