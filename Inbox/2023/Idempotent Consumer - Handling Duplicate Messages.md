---
created: 2024-02-26T22:28:47 (UTC -03:00)
tags: []
source: https://www.milanjovanovic.tech/blog/idempotent-consumer-handling-duplicate-messages?utm_source=Twitter&utm_medium=social&utm_campaign=19.02.2024
author: Milan Jovanović
---

# Idempotent Consumer - Handling Duplicate Messages

> ## Excerpt
> What happens when a message is retried in an event-driven system?
It happens more often than you think.
The worst case scenario is that the message is processed twice, and the side effects can also be applied more than once.
Do you want your bank account to be double charged?
I'll assume the answer is no, of course.
You can use the Idempotent Consumer pattern to solve this problem.
In this week's issue I'll show you: - How the Idempotent Consumer pattern works - How to implement an Idempotent Consumer - The tradeoffs you need to consider
Let's see why the Idempotent Consumer pattern is valuable.

---
Thank you to our sponsors who keep this newsletter free to the reader:

Today's issue is sponsored by [**Hasura**](https://hasura.io/graphql/database/sql-server/?utm_source=newsletter&utm_medium=email&utm_campaign=milan-dotnet-weekly), an open-source engine that gives [**instant GraphQL and REST APIs on new or existing SQL Server**](https://hasura.io/graphql/database/sql-server/?utm_source=newsletter&utm_medium=email&utm_campaign=milan-dotnet-weekly), enabling teams to ship apps faster.

And by [**Rebus Pro.**](https://pro.rebus.fm/) Rebus is a free .NET "service bus", and [**Rebus Pro is the perfect one-up for serious Rebus users.**](https://pro.rebus.fm/) Use Fleet Manager to get Slack alerts when something fails and retry dead-lettered messages with a click of the mouse.

What happens when a **message is retried** in an event-driven system?

It happens more often than you think.

The **worst case scenario** is that the **message is processed twice**, and the **side effects** can also be applied more than once.

Do you want your bank account to be double charged?

I'll assume the answer is no, of course.

You can use the **Idempotent Consumer** pattern to solve this problem.

In this week's issue I will show you:

-   How the Idempotent Consumer pattern works
-   How to implement an Idempotent Consumer
-   The tradeoffs you need to consider

Let's see why the **Idempotent Consumer** pattern is valuable.

## [How The Idempotent Consumer Pattern Works](https://www.milanjovanovic.tech/blog/idempotent-consumer-handling-duplicate-messages?utm_source=Twitter&utm_medium=social&utm_campaign=19.02.2024#how-the-idempotent-consumer-pattern-works)

What's the idea behind the **Idempotent Consumer pattern**?

> An idempotent operation is one that has no additional effect if it is called more than once with the same input parameters.

We want to avoid handling the same message more than once.

This would require **Exactly-once** message delivery guarantees from our messaging system. And this is a really hard problem to solve in distributed systems.

A looser delivery guarantee is **At-least-once**, where we are aware that retries can happen and we can receive the same message more than once.

The **Idempotent Consumer** pattern works well with **At-least-once** message delivery, and solves the **problem of duplicate messages.**

Here's what the algorithm looks like from the moment we receive a message:

1.  Was the message already processed?
2.  If yes, it's a duplicate and there's nothing to do
3.  If not, we need to handle the message
4.  We also need to store the message identifier

![](https://www.milanjovanovic.tech/blogs/mnw_034/idempotent_consumer_algorithm.png?imwidth=828)

We need a **unique identifier** for every **message** we receive, and a table in the database to store processed messages.

However, it's interesting how we choose the implement the message handling and storing of the processed message identifier.

You can implement the idempotent consumer as a decorator around a regular message handler.

I'll show you two implementations:

-   Lazy idempotent consumer
-   Eager idempotent consumer

## [Lazy Idempotent Consumer](https://www.milanjovanovic.tech/blog/idempotent-consumer-handling-duplicate-messages?utm_source=Twitter&utm_medium=social&utm_campaign=19.02.2024#lazy-idempotent-consumer)

The **lazy idempotent consumer** matches the flow shown in the algorithm above.

Lazy refers to how we store the message identifer to mark it as processed.

In the happy path, we handle the message and store the message identifier.

If the message handler throws an exception, we never store the message identifier and the consumer can be executed again.

Here's what the implementation looks like:

```csharp
public class IdempotentConsumer<T> : IHandleMessages<T> where T : IMessage { private readonly IMessageRepository _messageRepository; private readonly IHandleMessages<T> _decorated; public IdempotentConsumer( IMessageRepository messageRepository, IHandleMessages<T> decorated) { _messageRepository = messageRepository; _decorated = decorated; } public async Task Handle(T message) { if (_messageRepository.IsProcessed(message.Id)) { return; } await _decorated.Handle(message); _messageRepository.Store(message.Id); } }
```

## [Eager Idempotent Consumer](https://www.milanjovanovic.tech/blog/idempotent-consumer-handling-duplicate-messages?utm_source=Twitter&utm_medium=social&utm_campaign=19.02.2024#eager-idempotent-consumer)

The **eager idempotent consumer** is slightly different from the lazy implementation, but the end result is the same.

In this version, we eagerly store the message identifier in the database and then continue to handle the message.

If the handler throws an exception, we need to perform cleanup in the database and remove the eagerly stored message identifier.

Otherwise, we risk leaving the system in an inconsistent state since the message was never handled correctly.

Here's what the implementation looks like:

```csharp
public class IdempotentConsumer<T> : IHandleMessages<T> where T : IMessage { private readonly IMessageRepository _messageRepository; private readonly IHandleMessages<T> _decorated; public IdempotentConsumer( IMessageRepository messageRepository, IHandleMessages<T> decorated) { _messageRepository = messageRepository; _decorated = decorated; } public async Task Handle(T message) { try { if (_messageRepository.IsProcessed(message.Id)) { return; } _messageRepository.Store(message.Id); await _decorated.Handle(message); } catch (Exception e) { _messageRepository.Remove(message.Id); throw; } } }
```

## [In Summary](https://www.milanjovanovic.tech/blog/idempotent-consumer-handling-duplicate-messages?utm_source=Twitter&utm_medium=social&utm_campaign=19.02.2024#in-summary)

Idempotency is an interesting problem to solve in a software system.

Some operations are **naturally idempotent**, and we don't need the overhead of the **Idempotent Consumer** pattern.

However, for those operations that aren't naturally idempotent, the **Idempotent Consumer** is a great solution.

The high-level algorithm is simple, and you can take two approaches in the implementation:

-   Lazy storing of message identifiers
-   Eager storing of message identifiers

I prefer to use the **lazy approach**, and only **store the message identifier** in the database when the **handler completes successfully.**

It's easier to reason about and there is one less call to the database.

Thanks for reading.

Hope that was helpful.

___
