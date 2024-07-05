---
created: 2024-06-28T17:14:18 (UTC -03:00)
tags: []
source: https://newsletter.systemdesigncodex.com/p/3-types-of-event-patterns-in-eda?ref=dailydev
author: Saurabh Dashora
---

# 3 Types of Event Patterns in EDA - by Saurabh Dashora

> ## Excerpt
> Each pattern has a specific use

---
**What’s the most important part of Event-Driven Architecture?**

Events, of course.

The core principle of EDA revolves around components sending and receiving events to talk to each other and exchange data.

**But what exactly is an event?**

An event represents an occurrence or a fact that has taken place within the context of the application. It contains information about a specific action or state change.

Some common examples of events include:

-   A customer signing up for your application
    
-   A payment is received for an order
    
-   A failed authentication attempt
    

There are mainly 3 different patterns I've come across while dealing with events:

-   Event Notifications
    
-   Event-Based State Transfer
    
-   Event Sourcing
    

## 1 - Event Notifications

In the event notification pattern, the primary purpose of an event is to inform interested parties that a specific occurrence has taken place.

The event carries minimal state information, often just enough to identify the entity or action associated with the event.

For example, when a new payment is made in an e-commerce system, an event notification may include only the payment ID. By keeping the event payload minimal, the amount of data transmitted over the network is reduced.

Upon receiving an event notification, the consuming components have different options for processing the information:

-   Some components may simply record the event details for auditing or logging purposes, without taking any immediate action.
    
-   Other components may use the event as a trigger to fetch additional information about the occurrence from the source component. This allows them to retrieve more detailed data on-demand, rather than receiving a large payload upfront.
    

Event notifications are well-suited for scenarios where broadcasting information is required. They provide a lightweight mechanism for notifying multiple components without imposing strict requirements on how those components should react to the event.

## 2 - Event-Based State Transfer

Event-based state transfer is an asynchronous communication pattern that can be seen as the counterpart to the popular REST (Representational State Transfer) architecture. While REST follows an on-demand pull model, where clients request data from servers, event-based state transfer adopts a push approach.

In this pattern, data is sent out as events to be consumed by any interested components.

The event payload typically includes an identifier (ID) and the relevant information associated with the event.

Upon receiving the event, the consuming components can handle the data in different ways:

-   Some components may choose to create their local cached copies of the data for faster access and reduced dependency on the source component.
    
-   Other components may take specific actions based on the received data, such as triggering a workflow or updating their state.
    

One of the key advantages of event-based state transfer is that the consuming components do not need to make additional requests to the source component for more information.

The entire information is included within the event payload itself, making the communication self-contained. However, more data is transferred over the network.

## 3 - Event Sourcing

Event sourcing is not about event transmission but event storage.

In this pattern, each event represents a specific change or action that occurred on an entity. These events are stored in an append-only event store, which serves as a complete and immutable record of all the changes that have taken place over time.

By reading and processing the events from the event store, the application can replay the sequence of changes and derive the current state of the entity. This process is known as event replay or state reconstruction.

Event Sourcing provides several advantages such as:

-   **Audit Trail:** The event store acts as a comprehensive audit trail, allowing developers to track and analyze the history of changes made to an entity.
    
-   **Temporal Queries:** With event sourcing, it becomes possible to query the state of an entity at any point in the past. By replaying events up to a specific timestamp, the application can retrieve the historical state of an entity.
    

**👉 So - which event patterns have you used in your application?**

[Leave a comment](https://newsletter.systemdesigncodex.com/p/3-types-of-event-patterns-in-eda/comments)

## Shoutout

Here are some interesting articles I’ve read recently:

-   [I am not a fan of heroism in the engineering industry](https://newsletter.eng-leadership.com/p/i-am-not-a-fan-of-heroism-in-the?r=1m1f9z&utm_campaign=post&utm_medium=web) by
    
-   [How I write 1000s tests with little effort](https://craftbettersoftware.com/p/how-i-write-1000s-tests-with-little?r=1m1f9z&utm_campaign=post&utm_medium=web) by
    
-   [Normalization is not enough anymore](https://open.substack.com/pub/systemdesignclassroom/p/normalization-is-not-enough-anymore?r=1m1f9z&utm_campaign=post&utm_medium=web) by
    
-   [React Component Mental Models](https://thetshaped.dev/p/react-component-mental-models?r=1m1f9z&utm_campaign=post&utm_medium=web) by
    

**That’s it for today! ☀️**

Enjoyed this issue of the newsletter?

Share with your friends and colleagues.

[Share](https://newsletter.systemdesigncodex.com/p/database-caching-strategies?utm_source=substack&utm_medium=email&utm_content=share&action=share&token=eyJ1c2VyX2lkIjo5NzQ4NDE4MywicG9zdF9pZCI6MTM5MzgwNTM1LCJpYXQiOjE3MDYyMzg2NzEsImV4cCI6MTcwODgzMDY3MSwiaXNzIjoicHViLTIxNDgxMTEiLCJzdWIiOiJwb3N0LXJlYWN0aW9uIn0.g1BLNvN1bO0UbRH-bufX7vstlve_rQsqx7Iylcr-ENs)

_**See you later with another edition — Saurabh**_
