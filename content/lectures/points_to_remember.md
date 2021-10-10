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

