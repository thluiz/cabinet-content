---
created: 2023-08-11T10:51:05 (UTC -03:00)
tags: []
source: https://itnext.io/microservices-can-be-simple-if-we-let-them-a481d617a0a1
author: Andras Gerlits
---

# Microservices can be simple — if we let them | by Andras Gerlits | Aug, 2023 | ITNEXT

> ## Excerpt
> In our previous article, we talked about how our data-platform can help people integrate software. We discussed how the platform has better resilience guarantees than even its messaging platform and…

---
[

![Andras Gerlits](https://miro.medium.com/v2/resize:fill:88:88/0*QaTAvEqwTd5Yf0g4.)



](https://andrasgerlits.medium.com/?source=post_page-----a481d617a0a1--------------------------------)[

![ITNEXT](https://miro.medium.com/v2/resize:fill:48:48/1*yAqDFIFA5F_NXalOJKz4TA.png)



](https://itnext.io/?source=post_page-----a481d617a0a1--------------------------------)

[_In our previous article_](https://itnext.io/loosely-coupled-monoliths-and-where-to-find-them-4004fac8ecc1)_, we talked about how our data-platform can help people integrate software. We discussed how the platform has better resilience guarantees than even its messaging platform and how it can solve the distributed state problem by thinking differently about it._

_In this article, we’ll continue this story and dive deeper into how these promises can be kept and what benefits they provide to inter-connected systems, such as microservices or other kinds of integrated solutions. We’ll explain how our new data platform can both be seamless and radically improved by rethinking the way these services can be provided._

In the following, we’ll explain how we provide:

-   A radically simpler, cleaner operational model,
-   Auto-magic horizontal scaling,
-   Consistent, local-only follower reads without sacrificing up-to-dateness,
-   Network-originated latency-spike reduction,
-   Faster and more predictable commit-times

## Traditional, mutex-based approaches

In transactional, distributed systems, transactions are ordered internally and they serve both reads and writes from their internal storage. In other words, they are the central authority for what the system “knows” at any given time. They also replicate every write they accept (to as many copies as configured) internally, to keep the system running in case of a failure.

Any system providing consistency (including ours) needs to establish the relative order of transactions and make sure that each record’s update-history is the same for everyone.

The classic approach to achieving this is by using the following mechanism:

-   Client sends some data-changes
-   The system attempts to write these changes to their internal data-store
-   If the changes contains data that another transaction is currently updating, it must either make the transaction wait or Fail it
-   When the transaction can go ahead, the write is replicated
-   When all the writes are done, control is returned to the transaction and the client receives their “COMMIT OKAY” response

![](https://miro.medium.com/v2/resize:fit:700/0*qUq64csjxa3EoM8L)

A single transaction -travelling from a server to the database- must be replicated between many computers through the network

Such systems must figure out what happened to the received data, and then communicate this result back to the client. Since the system is made up of a number of shards and replicas communicating over a network and since the order in which these different computers receive their instructions can make the difference between a successful and a failed transaction, it must make sure that the results of these remote calls is agreed by all the affected processes before it can finalise a result.

In other words, these systems must be **synchronous** (blocking or push-based) to many subsystems while applying the client’s data-changes. Each time they need to communicate something, they need to **wait for an answer** from the other side, which might take a while, due to the other side or the network being busy. Blocking calls are also **inefficient**, since they reserve critical resources while waiting for an answer, in a sense it’s a form of [busy waiting](https://en.wikipedia.org/wiki/Busy_waiting).

## What we do

Our system is different. Since the client submitting new information must be kept waiting until their changes have been accepted or rejected, we must also make the client wait synchronously. Beyond this step however, we can be fully **non-blocking** (or pull-based).

We differentiate two kinds of **agents** in the system: **nodes** and **followers**. Nodes are deterministically reproducible agents, which act as both shards and replicas. We refer to all the nodes in a system as the **cloud**. Followers are the interfaces of the end-users to the platform. They act as a middle-man between the instructions emitted by nodes and the client-API (think SQL console or some similar data interface).

## Nodes

## Sharding and Replication

Nodes are

1.  single-threaded,
2.  have some internal state and
3.  produce instructions to other agents

A node only receives inputs through its single, ordered **instruction-stream**. This means that as long as:

-   Instructions are ordered and labelled with a unique, incrementing offset,
-   A node remembers what the last instruction it processed was
-   Instructions can be replayed, starting with the offset the node processed last
-   The instructions sent from these nodes are labelled with an **idempotent identifier**

When nodes and followers receive an instruction, they can check the [idempotent](https://en.wikipedia.org/wiki/Idempotence) identifier. If it’s the first time they see it, they process it, otherwise ignore it. If a node fails, we can take a copy we made earlier of it and start it up again. It will eventually catch up with the others in the system and until it does, its outputs will be ignored by other agents. Nodes emit instructions to other agents into different streams, with the attributes we just described. We call the streams from which followers consume instructions **event-streams**.

All this means that we can safely replicate them deterministically and we don’t need to worry about redundancy or fail-over.

## Followers

Followers make sense of the instructions received from nodes in event-streams and present them to the user in the format they expect. They also

-   accept data-changes from the client-API and
-   mediate them to the cloud and
-   communicate the result of the operation back to the client-API

Followers always update what they read. When a record is updated by the follower, they also send along the version they know for it. This might not be the last version accepted by the cloud, in which case the transaction will be rejected when it’s processed (by a node). In short, we use [Optimistic Concurrency Control](https://en.wikipedia.org/wiki/Optimistic_concurrency_control).

## Benefits

Let’s check how we can fulfill the promises we made in the first section.

## A radically simpler, cleaner operational model

Since the cloud can be replicated through its instruction-streams and since both these and event-streams can be processed in batches, this results in **better throughput and responsiveness** than push-based alternatives. We can also run RAM-only instances of nodes for system hotspots, and persistent storage-based ones as write-behind copies. Since agents will always pick up the first (fastest) published messages from a stream, this results in **faster response times**. Since nodes only hold the version-history (and not the value) of the record, **they remain small**.

## Auto-magic horizontal scaling

In our system, local copies of the data are the primary copies of them, since the clock is only ever reconstructed by the follower in their own time. Updates overwrite previous data and one follower’s reads will never impact another’s. Together, this means **painless horizontal scaling**.

## Consistent, local-only follower reads without sacrificing up-to-dateness

Followers pick up changes from the event-streams and apply them locally without any additional communication. Since nothing can be faster than one-way (latency-mitigated) streaming, we’re confident that this model provides the **fastest possible way for followers to receive consistent updates.** No more caches with complex invalidation. Adding more followers to the setup doesn’t slow down existing ones.

## Network-originated latency-spike reduction

Since there are always multiple agents (sources of information) sending messages, the chances of all of them being blocked due to a busy network or computer at the same time **reduces exponentially** with the number of replicas. If we define latency-events as P99 (meaning one in every hundred events would be delayed) and if we have 3 replicas running of the node, the chances of a latency-spike happening are reduced 4-folds. To put it differently, if we now send a message on average once every 5 milliseconds, a latency-event (on average) happens **twice a second**. With **3 replicas, this chance is reduced to 1 in a million**. Applying the same message-frequency, this reduces the chance of a latency spike to once in every 83 minutes. **With 5 replicas, to roughly once every year and a half.**

## Faster and more predictable commit-times

Since transactions never need to wait for other transactions to finish and since nodes never need to do more than one round of communication with other nodes to finalise a transaction, commit-times can be reliably predicted. If RTT means the round-trip time for messaging between two agents in the system (i.e.: the time a request-response message takes without any processing time), a single-node commit will take exactly:

1.  RTT (between client and node) +
2.  transaction processing time on node (single digit millisecond) +
3.  client-side transaction processing time

If a commit affects records held on multiple nodes:

1.  RTT (between client and node 1) +
2.  transaction processing time on node 1 (single digit millisecond) +
3.  RTT (back- and forth between nodes) +
4.  Processing time on node 2 (single digit millisecond) +
5.  client-side transaction processing time

Since nodes never block a transaction’s execution and because of latency-mitigation, the solution will keep **unprecedented stability for write-times**.

## Technical details and implementation

_For those curious about the technical details of the solution, we’re providing a simplified introduction to the_ [_science-paper_](https://www.researchgate.net/publication/359578461_Continuous_Integration_of_Data_Histories_into_Consistent_Namespaces)_. All concepts here are explained in meticulous detail there, but are kept short for broader accessibility to the concepts._

We are left with two problems, transaction ordering and collision-resolution.

## Ordering events in multiple streams

Since our system doesn’t use physical clocks (that would be a source of nondeterminism) our ordering can only be established through the instruction-streams. Let’s look at an example with 3 nodes, 1, 2 and 3.

![](https://miro.medium.com/v2/resize:fit:682/0*mzH8kpU5Lul7GYMf)

This figure shows the three instruction-streams of nodes 1, 2 and 3, working in parallel.This figure shows the three instruction-streams of nodes 1, 2 and 3, working in parallel.

Since we can only make sense of time in the context of messages being received by the event-log, we need to somehow demarcate the passing of time using messages. To achieve this, we send a special message called a “token” instruction from one node to another to signal the movement of time. They become an instruction like any other and their receival is handled the same way.

![](https://miro.medium.com/v2/resize:fit:682/0*ilVhouLVNUFRXYdQ)

This illustration continues on our earlier example, but the token instructions have been highlighted.

This circular passing of the token instruction establishes a batch of messages on each instruction-stream between each receival of such a message. We can say that the messages 1:4 and 1:5 “happened” between “times” 1 and 4. For node 1, this would be 7 after its last event, which was 4. This means that we know in advance the next value each node will use.

We also know that when a token-event with a value is received, each event that happened before its value on any of the nodes has already been processed.

![](https://miro.medium.com/v2/resize:fit:682/0*Ur_2dH9_boJxH6RR)

This figure shows which values are known to have been processed on node 1, after it received its second token-instruction. The instructions highlighted in green must have finished processing in the whole cluster.

As long as we know the number of nodes participating in the same group, we can also predict the next value the node will use when the next token-event arrives. We call the value which we received last the “closed version” and the one that will be received on the next receival, the “open version”. If transactions always write new information into the cluster using the currently open version (at the point when a transaction’s commit event has finished processing) and read information using the closed version, it will always read committed information and write using a version which will only be available to others atomically.

We can also establish hierarchies of such groups. A parent-group will have a number of child-groups assigned to it, from which it will receive association messages. These associations are immutable, so an algorithm can be established which deterministically tells the client whether a particular event happened before or after a specific version (or, since versions represent events, a specific event).

## Collision resolution mechanism

As explained above, the [Optimistic Concurrency Control](https://en.wikipedia.org/wiki/Optimistic_concurrency_control) is an important part of how we enforce a single, agreed history of updates of all records, but is not the whole story. What happens if we have two transactions, both coming in at the same time with an up-to-date set of changes? Our system must choose at most one of these two to succeed, or they would violate the promises we made earlier.

To fix this, we established a deterministic commit-algorithm which tests if there are conflicting updates happening on any of the affected nodes while working on this one. If there are, the commit algorithm chooses a winner. This is safe to do, since the algorithm can **transitively determine the winner of any two conflicting updates**.

If you’d like even more details, you can [consult our science-paper](https://www.researchgate.net/publication/359578461_Continuous_Integration_of_Data_Histories_into_Consistent_Namespaces), which fills the holes you might have spotted here.
