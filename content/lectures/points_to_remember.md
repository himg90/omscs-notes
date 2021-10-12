---
title: "Points_to_remember"
geekdocDescription: ""
keywords: ["cs7210", "distributed computing", "omscs"]

date: 2021-10-10T16:32:34+05:30
draft: true
---

# Default Title

__What is a Distributed System?__

A collection of independent and autonomous processes(nodes) that interact by exchanging messages via interconnection network and appear to external users as a single coherent computing facility. 

__Consistency in Distributed System__

We want the distributed system to behave as if it had a single up-to-date copy of data. 

__High Availability vs. Fault Tolerance __

[source](https://www.linkedin.com/pulse/high-availability-vs-fault-tolerance-jon-bonso/)

> High Availability aims for your application to run 99.999% of the time. Its design ensures that the entire system can quickly recover if one of its components crashed. It has an ample number of redundant resources to allow failover to another resource if the other one fails. This concept accepts that a failure will occur but provides a way for the system to recover fast.
>
> Fault Tolerance, on the other hand, has the goal of keeping your application running with zero downtime. It has a more complex design, and higher redundancy to sustain any fault in one of its components. Think of it as an upgraded version of High Availability. As its name implies, it can tolerate any component fault to avoid any performance impact, data loss, or system crashes by having redundant resources beyond what is typically needed. The caveat for implementing a fault-tolerant system is its cost as companies have to shoulder the capital and operating expenses for running its required numerous resources. 
>
> A system can be highly available but not fault-tolerant, and it can be both. If an application is said to be fault-tolerant then it is also considered highly available. However, there are situations in which a highly available application is not considered fault-tolerant.



__Linearizability and Serializability__

source: [Herlihy, Maurice and Jeanette Wing: Linearizability: A Correctness Condition for Concurrent Objects](http://www.cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)

> Much work on databases and distributed systems uses serializability as the basic correctness condition for concurrent computations. In this model, a transaction is a thread of control that applies a finite sequence of primitive operations to a set of objects shared with other transactions. A history is serializable if it is equivalent to one in which transactions appear to execute sequentially, i.e., without interleaving. A (partial) precedence order can be defined on non-overlapping pairs of transactions in the obvious way. A history is strictly serializable if the transactions’ order in the sequential history is compatible with their precedence order...
>
> ...Linearizability can be viewed as a special case of strict serializability where transactions are restricted to consist of a single operation applied to a single object. Nevertheless, this single-operation restriction has far-reaching practical and formal consequences, giving linearizable computations a different flavor from their serializable counterparts. An immediate practical consequence is that concurrency control mechanisms appropriate for serializability are typically inappropriate for linearizability because they introduce unnecessary overhead and place unnecessary restrictions on concurrency.

__Consistency and Consensus in Data Replication__

Source: <a href="https://docs.hazelcast.com/imdg/4.2/consistency-and-replication/consistency.html" target="_blank">A Brief Overview of Consistency and Replication in Distributed Systems</a>

> Partitioning and replication are the two common techniques used together in distributed databases to achieve scalable, available and transparent data distribution. The data space is first divided into partitions then each partition is relicated among the cluster members. Each member is assigned to at most a single replica for a partition. In this setting, different replication techniques can be used to access the data and keep the replicas in sync on updates. The technique being used directly affects the guarantees and properties a distributed data store provides, due to the CAP (**C**onsistency, **A**vailability and **P**artition Tolerance) principle.
>
> One aspect of replication techniques is about where a replicated data set is accessed and updated. For instance, primary-copy systems first elect a replica, which can be called as primary, master, etc., and use that replica to access the data. Changes in the data on the primary replica are propagated to other replicas. This approach has different namings, such as *primary-copy*, *single-master*, *passive replication*. The primary-copy technique is a powerful model as it prevents conflicts, deadlocks among the replicas. However, primary replicas can become bottlenecks. On the other hand, we can have a different technique by eliminating the primary-copy and treating each replica as equal. These systems can achieve a higher level of availability as a data entry can be accessed and updated using any replica. However, it can become more difficult to keep the replicas in sync with each other.
>
> Replication techniques also differ in how updates are propagated among replicas. One option is to update each replica as part of a single atomic transaction, called as *eager replication* or *synchronous replication*. Consensus algorithms apply this approach to achieve strong consistency on a replicated data set. The main drawback is the amount of coordination and communication required while running the replication algorithm. CP systems implement consensus algorithms under the hood. Another option is the *lazy replication* technique, which is also called as *asynchronous replication*. Lazy replication algorithms execute updates on replicas with separate transactions. They generally work with best-effort. By this way, the amount of coordination among the replicas are degraded and data can be accessed in a more performant manner. Yet, it can happen that a particular update is executed on some replicas but not on others, which causes replicas to diverge. Such problems can be resolved with different approaches, such as *read-repair*, *write-repair*, *anti-entropy*. Lazy replication techniques are popular among AP systems.

Key Takeaways

* When data is replicated over multiple nodes various design choices have to be made
  * Synchronous vs Asynchronous replication
  * Strong vs Weak consistency
* __Question:__ Does synchronous replication imply strong consistency and asynchronous replication imply weak consistency?
* Consensus based replication algorithms are used to build ` CP ` systems i.e. it provides `strong` consistency under network partition.

Source: <a href="https://blog.yugabyte.com/how-does-consensus-based-replication-work-in-distributed-databases/" target="_blank">How Does Consensus-Based Replication Work in Distributed Databases?</a>

> Consensus protocols can be broadly classified into two categories: leader-based and leaderless. Paxos and Raft are the two most commonly used leader-based consensus protocols where the task of data updates and replication is handled by a “leader”. Strongly consistent distributed databases over the years have standardized onto one of these protocols. Leaderless consensus protocols are harder to implement but have higher availability than leader-based protocols. Application of leaderless protocols can be found in blockchain distributed ledgers. 
