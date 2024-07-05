---
created: 2024-06-28T17:47:05 (UTC -03:00)
tags: []
source: chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html
author: Raul Junco
---

# 

> ## Excerpt
> Databases form the backbone of 90% of the software we create. They're essential for storing customer details, orders, and transactions.

---
## Normalization is not enough anymore.

### Databases form the backbone of 90% of the software we create. They're essential for storing customer details, orders, and transactions.

[![](https://substackcdn.com/image/fetch/w_80,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F45a92f5e-1e2e-4dfa-9ff3-45fc5ad0c57e_612x612.png)](https://substack.com/profile/98661477-raul-junco)

We spent years learning:

-   How to name the tables.
    

-   How to know which column is below each table?
    

-   How to make sure I don’t repeat my data?
    

-   How to store my data in a more optimal way.
    

This is what we call **Normalization,** a foundational principle in database design.

Normalization reduces redundancy and improves data integrity.

The idea is to organize data into tables to decrease duplication. Also, define relationships between those tables.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdd6b68f7-127f-413a-8958-9773b01130d1_2118x956.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdd6b68f7-127f-413a-8958-9773b01130d1_2118x956.png)

### But the world has changed.

The volume of the data we have now is amazing. Look around your house; how many devices do you have now that produce data you can access?

_Yes, even your vacuum cleaner._

The speed has also changed. You want to get information for that data, make decisions based on that information, and have it at the click of a button.

This has challenged the boundaries of traditional database design.

Normalization is no longer the be-all and end-all of database design. Normalization is critical, don't get me wrong, but the point is that it is not enough anymore.

### Normalization can fall short at:

-   If you need real-time data processing and low latency
    
-   With microservices architectures, databases are now often distributed and decentralized.
    
-   Data Analytical process.
    

### Generally speaking, Normalization can introduce Performance Overhead.

Since normalization involves splitting data into many tables to avoid redundancy, you are causing a side effect problem: now you have to join multiple tables.

This works fine in small-medium datasets, but In large datasets, so many joins degrade performance.

### The solution is to break some of the Normalization rules. And just because it is the opposite, we call it De-normalization.

The goal is to reduce the joins that degrade performance. As an additional result, you reduce the query complexity, too.

By de-normalizing a database, you can make certain operations more efficient.

### **How you de-normalize?**

-   If the data doesn't change often. You can duplicate the information in many tables to avoid joins.
    
-   If two tables are often joined, you can combine them into a single table. But make sure the merged table maintains all necessary information.
    
-   If you run reporting and analytical queries, look for opportunities to Pre-calculate aggregates. Pre-computed aggregates will save a lot of processing time.
    
-   Use **Materialized Views** to store the result of a complex query. This form of logical de-normalization doesn't need to change the underlying table structures.
    

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd8d20835-ee17-49e8-9e36-047791cd2414_1973x1717.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd8d20835-ee17-49e8-9e36-047791cd2414_1973x1717.png)

### **When do you need de-normalization?**

1.  In data warehousing and business intelligence scenarios. When the focus is on complex queries and data aggregation for reporting.
    

2.  For applications where read performance is critical and writes are relatively infrequent.
    

3.  In a Microservices architecture. Individual services may opt for de-normalization to optimize performance and reduce data aggregation. 
    
4.  In cases where you have hierarchical data. Hierarchical data is hard to query.
    

### De-normalization is real-life

The Name of a product doesn't change often. We can modify the OrderDetails Table to include ProductName.

This will save us that expensive join not touching the Products table.

The same approach applies to the customer’s name but to the Order’s table.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F06757a28-64ff-43a3-847c-e9c826b3f6a4_2086x2236.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F06757a28-64ff-43a3-847c-e9c826b3f6a4_2086x2236.png)

You have the tool now; what are you waiting to make that report faster? 

### **TL;DR:**

De-normalization optimizes database read performance. However, it introduces redundancy, increases storage needs, and complicates data maintenance.

De-normalization speeds up data retrieval by reducing the need for complex joins and aggregations at query time. However, it requires careful handling to avoid data inconsistencies.

In a de-normalized schema, every insert, update, or delete operation potentially affects multiple copies of the data.

De-normalization is a strategic decision. This decision should be a balance between performance improvements and data integrity.

De-normalization is especially beneficial for read-heavy applications where performance is critical.

Storing aggregated data, such as counts, sums, or averages, eliminates the need for calculations.

A combination of normalized and de-normalized schemas can be your sweet spot.

### Articles I enjoyed this week

[Load Balancers vs API Gateways vs BFFs](https://newsletter.systemdesigncodex.com/p/load-balancers-vs-api-gateways-vs) by

[How to Scale an App to 10 Million Users on AWS](https://newsletter.systemdesign.one/p/aws-scale) by

[How I Mastered Data Structures and Algorithms](https://blog.algomaster.io/p/how-i-mastered-data-structures-and-algorithms) by

[Building a real-time AI assistant (with voice and vision)](https://underfitted.svpino.com/p/building-a-real-time-ai-assistant) by
