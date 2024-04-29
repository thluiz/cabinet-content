---
created: 2023-10-10T20:10:30 (UTC -03:00)
tags: []
source: https://thenewstack.io/the-basics-of-event-driven-architectures/?ref=dailydev
author: Jim Moffitt
---

# The Basics of Event-Driven Architectures - The New Stack

> ## Excerpt
> This blog post explores the emergence of event-driven architectures (EDAs) and the components that make up event-driven systems. It also presents two fundamental design patterns that help illustrate how the components of event-driven architectures work together.

---
Real-time data and event-driven systems have enabled a wide range of use cases responsible for making what the internet is today. From [real-time personalization](https://www.tinybird.co/blog-posts/real-time-personalization), [fraud detection](https://www.tinybird.co/blog-posts/how-to-build-a-real-time-fraud-detection-system), [content recommendation engines](https://www.tinybird.co/blog-posts/real-time-recommendation-system) and inventory management systems, to advancements in customer service, environmental monitoring and communication, event-driven systems power much of the web innovation we’ve seen over the past decade.

As you build and evolve your own products, features and fundamental data handling, it is very likely that its design will benefit from the application of event-driven architectural concepts.

We live in an exciting time to be building event-driven systems. So let’s explore what event-driven architectures are and how they make it easier to work with high volumes of real-time data.

## What Is Event-Driven Architecture?

Event-driven architectures (EDAs) are a way of designing and building systems that are based on the exchange of events. Events are notifications of some change in state or data, and they are typically published by one component and consumed by another in real time.

EDAs are becoming increasingly popular because they offer a number of advantages over traditional, request-response architectures. For example, EDAs can:

-   Decouple systems that can be independently scaled and updated.
-   Handle high volumes of data with low latency.
-   Support real-time processing and analytics.
-   Be more scalable and resilient to failures.

First, what does real-time data mean and what is an event?

By real-time data, we mean data that is published as it is generated, with no known end of that data being published. The data just keeps coming and is available as soon as it is created. These data are commonly described as traveling in streams or queues that are built to handle large volumes and velocities of data.

Events are data that describe some action or fact. For example, events are generated when a user clicks on a product, when an IoT device is sampled or when a vehicle moves to a new location.

Events are represented as objects with one or more attributes. For example, a webstore event object would include event timestamps (likely with multiple timestamps that document its journey along a data pipeline) and event-type attributes (such as `view`, `cart` and `purchase` events). These event objects are commonly rendered as JSON objects, a popular format for encoding and packaging data.

Next, let’s break down the term “event-driven architecture.” Here, we are building real-time data pipelines where events drive the system, where an action triggers a set of components to deliver the event data to something listening for it. You subscribe to an action of interest, and it’s made available to you as soon as it happens.

By “architecture” we are referring to how software and hardware components are arranged and connected to build services needed to provide real-time communication and message exchange. These components include things like databases, communication and message channels, brokers that manage those channels, and methods for publishing and sharing with other systems. To support real-time data use cases, these components need to be designed from the ground up to handle and process data with low latency and high concurrency.

At the core of event-driven systems is the component that acts as an event or message broker. This component receives source data, publishes it on source-specific topics and enables listeners to subscribe to topics of interest. Often this component is referred to as an “event bus,” where `bus` is an analogy for something that moves things from place to place while picking up new passengers.

## How Are Event-Driven Architectures Built?

Event-driven architectures are made up of components that have specific roles in transmitting and processing data. In general, there are the components that make data available, components that publish data onto streams or queues, components that implement a message broker or “event bus,” and one or more “components” that listen for and consume the data.

This data may be consumed to perform analysis and generate [real-time visualizations](https://www.tinybird.co/blog-posts/real-time-data-visualization). The data may be stored in a [real-time database](https://thenewstack.io/real-time-databases-who-is-using-them-and-why/) and used to generate responses to API endpoint requests. Often the data is ingested into a [real-time data platform](https://www.tinybird.co/blog-posts/real-time-data-platforms) used to serve all these needs. Real-time data platforms are built with real-time databases, provide ways to filter, sort, and aggregate data, and offer ways to publish and share the data.

Let’s explore these components in more detail.

![](https://cdn.thenewstack.io/media/2023/09/2560180a-tiny-bird-01.png)

### Data sources

Data sources encapsulate the reasons you want to build an event-driven system. They contain the information and data you want and need to share with other stakeholders, such as other internal systems and customers. These sources may take the form of webstore customer actions, IoT networks providing weather and logistics information, financial data, logging systems or any other data that is being generated in real time.

Often this data lives in legacy storage systems that were not built to support the low latency and high concurrency that event-driven systems demand. Commonly, this data is stored in traditional databases such as MySQL, or even as files behind a network server. The good news starts with the fact that many tools and techniques exist to integrate these sources into real-time systems. If you have legacy databases and file systems with data you’d like to share with other systems in real time, you can use [change data capture](https://thenewstack.io/change-data-capture-for-real-time-access-to-backend-databases/) (CDC). Also, see [this blog post](https://www.tinybird.co/blog-posts/event-driven-architecture-best-practices-for-databases-and-files) for an overview of architectural best practices for integrating databases and files.

### Event generators

These components read your source data and publish it on a stream or queue. Generators write data to a message broker or event bus, making the data available to event listeners.

This component can range from a CDC component that writes database data, to custom code that writes data objects, to the event bus. For example, Debezium and its cloud-hosted variants are popular tools for implementing CDC-based sources. Also, here is an [example Python script](https://github.com/tinybirdco/pubsub-website-events-poc/blob/main/data-generator/pubsub_json_generator.py) that writes data directly to an event bus.

### Event buses

An event bus is where events are loaded and where events are offloaded. This component is responsible for [real-time data ingestion](https://www.tinybird.co/blog-posts/real-time-data-ingestion) from the generators and for making that data available to components listening for the data.

There are many forms of event buses, often referred to as streams or queues. Options for streaming components include Apache Kafka, Amazon Kinesis, Google Pub/Sub, Confluent Cloud and [Redpanda](https://redpanda.com/?utm_content=inline-mention). Likewise, there are many excellent choices for implementing queues, such as Amazon SQS, RabbitMQ and Redis.

These components are based on classic publish/subscribe concepts and offer a range of features and capabilities to meet the diverse needs of organizations.

Event buses can be configured to retain a custom window of data, enabling data consumers to get data on their own schedule. The advent of event bus architectures has been a boon for anyone who requires full-fidelity and reliable systems. If a consumer fails, it can reconnect and start where it left off.

### Event Listeners/Subscribers

These components consume the data, and there is commonly more than one listener. While most consumers are designed to read data only, it is possible to have consumers that remove data from the event bus or queue.

Listeners are designed to access data as soon as it is available by continuously polling the event bus for topics and messages of interest. In some cases, it may be possible to implement triggered polling, where some other signal is given to start the continuous polling process.

Listeners also have the responsibility to do something with the incoming data — to write the data somewhere. This could be to databases, data lakes or even additional downstream streams and queues.

### Real-Time Databases

One common destination that listeners write to is a database. To handle the data volumes and velocities typically associated with EDAs, it’s important to use a [real-time database](https://thenewstack.io/real-time-databases-who-is-using-them-and-why/). There are several options here, including the open source Apache Druid, Apache Pinot and [ClickHouse](https://thenewstack.io/why-clickhouse-should-be-your-next-database). These open source database packages are all also available via a cloud-hosted service.

These databases can be primary or secondary sources of data. Primary sources are the original and authoritative source of data, while secondary sources are copies of data from multiple sources. This compilation of data from multiple sources is a common motivation for building EDAs.

Like traditional databases, these databases support SQL for filtering, sorting, aggregating and joining data. So it’s good news that these cutting-edge data storage tools also support a querying language widely used across a large range of technical roles. Chances are that you and your colleagues are well-equipped to start analyzing and integrating these new data streams.

In addition, these databases may support real-time, [incremental materialized views](https://www.tinybird.co/blog-posts/what-are-materialized-views-and-why-do-they-matter-for-realtime), which auto-populate query results into new table views as event-driven data is ingested in real time.

### Publication Layer

In most cases, event-driven systems are built to make real-time data available to a variety of consumers and stakeholders. These stakeholders may include data scientists and analysts performing ad hoc analysis, dashboards and report generators, web and mobile application features driven by real-time data or automated control systems that take actions without human intervention.

While this data availability may be implemented with a wide range of methods, ranging from webhook events to generating flat files, the most common method is building API endpoints for consumers to request data from. These API endpoints have the advantage of being extremely flexible, since they are able to serve customized data content to their consumers.

### Real-Time Data Platforms

[Real-time data platforms](https://www.tinybird.co/blog-posts/real-time-data-platforms) combine many of the components that EDAs are built with. These platforms include native data connectors for both streaming and batch data sources. In the case of streaming sources, these platforms provide ways to seamlessly consume from a variety of event buses such as Apache Kafka and others implementing the publisher/subscriber model. In addition, the platforms typically provide an endpoint for streaming data into it.

The platforms also manage data storage of the incoming data by integrating real-time databases. The systems are typically built on top of open source real-time databases, which enable them to manage and process high volumes and velocities of data.

Along with these integrated databases comes the ability to perform data analysis with SQL. The platforms commonly provide user interfaces for writing and designing queries for filtering and aggregating data and joining from multiple data sources.

Finally, the platforms integrate methods for publishing and sharing data. In some cases, they are used to publish data to streams or export data in a batch process. Most advanced platforms make it possible to serve data via low-latency and high-concurrency data APIs.

The advantage of building a real-time data platform into your event-driven architecture is that by combining fundamental EDA components, they remove many of the complexities of using separate components and ‘gluing’ them together. In particular, self-hosting real-time databases and building APIs from scratch demand experience and expertise that is abstracted away by real-time data platforms.

## Event-Driven System Design Patterns

To demonstrate how these components fit together, here are two reference architectures (For additional architecture examples, see [this blog post](https://www.tinybird.co/blog-posts/real-time-streaming-data-architectures-that-scale)).

First, we have a fundamental pattern that focuses on data storage. Here incoming events are immediately stored in three different types of storage:

-   Transactional database — may power functionality for user-facing applications and typically persist fundamental state information key.
-   Data warehouse — built for large datasets, long-term storage and building historical archives.
-   Real-time database — may power real-time analytics and a publication layer. Built to support low-latency data retrieval and high concurrency and is well-suited to serve data consumers at scale.

Here we have three listeners and destinations for new event data. With this design, the data warehouse makes requests for new data directly from the real-time database.

![](https://cdn.thenewstack.io/media/2023/09/c11d9619-tiny-bird-02.png)

We can extend that design by adding a [real-time data analytics](https://www.tinybird.co/blog-posts/real-time-analytics-a-definitive-guide) platform, along with a publication layer. With this type of design, the data analytics platform encapsulates several EDA components.

For example, here the real-time data platform includes a “connector” that listens for events and consumes them, provides a real-time database and analytical tools, and is able to host APIs for sharing data and analysis.

![](https://cdn.thenewstack.io/media/2023/09/4e20d046-tiny-bird-03.png)

## Designing Your Event-Driven Future

Whether you are designing for something new or revisiting designs implemented a long time ago, it’s worth exploring how event-driven architecture patterns can help. You can probably identify many cases where introducing real-time data would improve your product, system or user experience.

If you are in the business of building customer-facing apps, you probably already have a list of data-driven features that would delight your customers. At a minimum, there are probably a few existing pain points that are due for a tune up, along with lots of opportunities for small, quick performance improvements.

As you get started, there are three distinct areas to consider. First, identify where and how the data you want to build with is generated, stored and made available. Perhaps you have a data source that is already written to a stream or queue and all you have to do is add a new listener. Perhaps you have a backend database that you can integrate using [change data capture](https://thenewstack.io/change-data-capture-for-real-time-access-to-backend-databases/) techniques.

Second, decide what type of “event bus” to implement and start building the bridge from where events are generated to where you can listen for them. As mentioned above, there are many open source solutions available that can be self-hosted or cloud-hosted.

Third, with your data sources and event stream sorted out, it’s time to build data consumers.

Real-time data platforms are a common type of event data consumer. These platforms integrate many system components into a single package. For example, [Tinybird](https://www.tinybird.co/) is a real-time data platform that manages real-time event ingestion and storage, provides real-time data-processing and analysis tools, and hosts scalable, secure API endpoints.

For more information about Tinybird, check out the [documentation](https://www.tinybird.co/docs). For more resources on building event-driven or real-time architectures, you can read [this](https://www.tinybird.co/blog-posts/event-driven-architecture-best-practices-for-databases-and-files), [this](https://www.tinybird.co/blog-posts/real-time-streaming-data-architectures-that-scale) or [this](https://www.tinybird.co/blog-posts/real-time-change-data-capture).

Group Created with Sketch.
