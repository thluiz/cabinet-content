---
created: 2024-07-15T14:08:36 (UTC -03:00)
tags: []
source: https://www.wikiwand.com/en/PACELC_theorem?ref=dailydev
author: 
---

# PACELC theorem - Wikiwand

> ## Excerpt
> In database theory, the PACELC theorem is an extension to the CAP theorem. It states that in case of network partitioning (P) in a distributed computer system, one has to choose between availability (A) and consistency (C), but else (E), even when the system is running normally in the absence of partitions, one has to choose between latency (L) and loss of consistency (C).

---
In [database theory](https://www.wikiwand.com/en/Database_theory "Database theory"), the **PACELC theorem** is an extension to the [CAP theorem](https://www.wikiwand.com/en/CAP_theorem "CAP theorem"). It states that in case of network partitioning (P) in a distributed computer system, one has to choose between availability (A) and consistency (C) (as per the CAP theorem), but else (E), even when the system is running normally in the absence of partitions, one has to choose between latency (L) and loss of consistency (C).

[

![Thumb image](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3c/PACELC_theorem.png/640px-PACELC_theorem.png)

](https://www.wikiwand.com/en/File:PACELC_theorem.png)

The tradeoff between availability, consistency and latency, as described by the PACELC theorem.

PACELC builds on the [CAP theorem](https://www.wikiwand.com/en/CAP_theorem "CAP theorem"). Both theorems describe how distributed databases have limitations and tradeoffs regarding consistency, availability, and partition tolerance. PACELC goes further and states that an additional [trade-off](https://www.wikiwand.com/en/Trade-off "Trade-off") exists: between latency and loss of consistency, even in absence of partitions, thus providing a more complete portrayal of the potential consistency trade-offs for distributed systems.<sup id="cite_ref-Abadi_1-0"><a href="https://www.wikiwand.com/en/PACELC_theorem#cite_note-Abadi-1"></a></sup>

A high availability requirement implies that the system must replicate data. As soon as a distributed system replicates data, a trade-off between consistency and latency arises.

The PACELC theorem was first described by [Daniel Abadi](https://www.wikiwand.com/en/Daniel_Abadi "Daniel Abadi") from [Yale University](https://www.wikiwand.com/en/Yale_University "Yale University") in 2010 in a blog post,<sup id="cite_ref-2"><a href="https://www.wikiwand.com/en/PACELC_theorem#cite_note-2"></a></sup> which he later clarified in a paper in 2012.<sup id="cite_ref-Abadi_1-1"><a href="https://www.wikiwand.com/en/PACELC_theorem#cite_note-Abadi-1"></a></sup> The purpose of PACELC is to address his thesis that "Ignoring the consistency/latency trade-off of replicated systems is a major oversight \[in CAP\], as it is present at all times during system operation, whereas CAP is only relevant in the arguably rare case of a network partition." The PACELC theorem was proved formally in 2018 in a SIGACT News article.<sup id="cite_ref-Golab_3-0"><a href="https://www.wikiwand.com/en/PACELC_theorem#cite_note-Golab-3"></a></sup>

<sup id="cite_ref-Abadi_1-2"><a href="https://www.wikiwand.com/en/PACELC_theorem#cite_note-Abadi-1"></a></sup>Original database PACELC ratings are from.<sup id="cite_ref-ctmddsd_4-0"><a href="https://www.wikiwand.com/en/PACELC_theorem#cite_note-ctmddsd-4"></a></sup> Subsequent updates contributed by wikipedia community.

-   The default versions of [Amazon's early (internal) Dynamo](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf), [Cassandra](https://www.wikiwand.com/en/Apache_Cassandra "Apache Cassandra"), [Riak](https://www.wikiwand.com/en/Riak "Riak"), and [Cosmos DB](https://www.wikiwand.com/en/Cosmos_DB "Cosmos DB") are PA/EL systems: if a partition occurs, they give up consistency for availability, and under normal operation they give up consistency for lower latency.
-   Fully ACID systems such as [VoltDB](https://www.wikiwand.com/en/VoltDB "VoltDB")/H-Store, Megastore, [MySQL Cluster](https://www.wikiwand.com/en/MySQL_Cluster "MySQL Cluster"), and [PostgreSQL](https://www.wikiwand.com/en/PostgreSQL "PostgreSQL") are PC/EC: they refuse to give up consistency, and will pay the availability and latency costs to achieve it. [Bigtable](https://www.wikiwand.com/en/Bigtable "Bigtable") and related systems such as [HBase](https://www.wikiwand.com/en/Apache_HBase "Apache HBase") are also PC/EC.
-   Amazon DynamoDB (launched January 2012) is quite different from the [early (Amazon internal) Dynamo](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf) which was considered for the PACELC paper.<sup id="cite_ref-ctmddsd_4-1"><a href="https://www.wikiwand.com/en/PACELC_theorem#cite_note-ctmddsd-4"></a></sup> DynamoDB follows a strong leader model, where every write is strictly serialized (and conditional writes carry no penalty) and supports read-after-write consistency. This guarantee does not apply to "Global Tables<sup id="cite_ref-5"><a href="https://www.wikiwand.com/en/PACELC_theorem#cite_note-5"></a></sup>" across regions. The DynamoDB SDKs use eventually consistent reads by default (improved availability and throughput), but when a consistent read is requested the service will return either a current view to the item or an error.
-   Couchbase provides a range of consistency and availability options during a partition, and equally a range of latency and consistency options with no partition. Unlike most other databases, Couchbase doesn't have a single API set nor does it scale/replicate all data services homogeneously. For writes, Couchbase favors Consistency over Availability making it formally CP, but on read there is more user-controlled variability depending on index replication, desired consistency level and type of access (single document lookup vs range scan vs full-text search, etc.). On top of that, there is then further variability depending on cross-datacenter-replication (XDCR) which takes multiple CP clusters and connects them with asynchronous replication and Couchbase Lite which is an embedded database and creates a fully multi-master (with revision tracking) distributed topology.
-   [Cosmos DB](https://www.wikiwand.com/en/Cosmos_DB "Cosmos DB") supports five tunable consistency levels that allow for tradeoffs between C/A during P, and L/C during E. [Cosmos DB](https://www.wikiwand.com/en/Cosmos_DB "Cosmos DB") never violates the specified consistency level, so it's formally CP.
-   [MongoDB](https://www.wikiwand.com/en/MongoDB "MongoDB") can be classified as a PA/EC system. In the baseline case, the system guarantees reads and writes to be consistent.
-   PNUTS is a PC/EL system.
-   Hazelcast IMDG and indeed most in-memory data grids are an implementation of a PA/EC system; Hazelcast can be configured to be EL rather than EC.<sup id="cite_ref-dbmsmusings201710_6-0"><a href="https://www.wikiwand.com/en/PACELC_theorem#cite_note-dbmsmusings201710-6"></a></sup> Concurrency primitives (Lock, AtomicReference, CountDownLatch, etc.) can be either PC/EC or PA/EC.<sup id="cite_ref-:0_7-0"><a href="https://www.wikiwand.com/en/PACELC_theorem#cite_note-:0-7"></a></sup>
-   FaunaDB implements Calvin, a transaction protocol created by Dr. Daniel Abadi, the author<sup id="cite_ref-Abadi_1-3"><a href="https://www.wikiwand.com/en/PACELC_theorem#cite_note-Abadi-1"></a></sup> of the PACELC theorem, and offers users adjustable controls for LC tradeoff. It is PC/EC for strictly serializable transactions, and EL for serializable reads.

**More information** DDBS, P+A ...

Close

-   [CAP theorem](https://www.wikiwand.com/en/CAP_theorem "CAP theorem")
-   [Consistency model](https://www.wikiwand.com/en/Consistency_model "Consistency model")
-   [Fallacies of distributed computing](https://www.wikiwand.com/en/Fallacies_of_distributed_computing "Fallacies of distributed computing")
-   [Lambda architecture](https://www.wikiwand.com/en/Lambda_architecture "Lambda architecture") (solution)
-   [Paxos (computer science)](https://www.wikiwand.com/en/Paxos_(computer_science) "Paxos (computer science)")
-   [Project management triangle](https://www.wikiwand.com/en/Project_management_triangle "Project management triangle")
-   [Raft (algorithm)](https://www.wikiwand.com/en/Raft_(algorithm) "Raft (algorithm)")
-   [Trilemma](https://www.wikiwand.com/en/Trilemma#In_computing_and_technology "Trilemma")

1.  Dynamo, Cassandra, and Riak have user-adjustable settings to control the LC tradeoff.<sup id="cite_ref-ctmddsd_4-2"><a href="https://www.wikiwand.com/en/PACELC_theorem#cite_note-ctmddsd-4"></a></sup>
    
2.  Cosmos DB has five selectable consistency levels to control the LC tradeoff.<sup id="cite_ref-10"><a href="https://www.wikiwand.com/en/PACELC_theorem#cite_note-10"></a></sup>
