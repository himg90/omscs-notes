---
title: "Paxos And CAP"
geekdocDescription: ""
keywords: ["cs7210", "distributed computing", "omscs", "paxos", "CAP theorem"]

date: 2021-10-16T10:30:29+05:30
draft: false
---

# Paxos and CAP Theorem

CAP theorem seems to suggest that under network partition, both consistency and availability cannot be achieved simultaneously. However, Paxos can provide strong consistency even under a network partition. Does it mean that Paxos contradicts CAP theorem?

CAP theorem is known to be misunderstood. There are three points to note. One is that the tradeoff between consistency and availablity only arises under network partition. It means that under normal operation (when there is no network partition) both consistency and availability are achievable. Second is the definition of consistency. The CAP theorem is talking about strong consistency. The third point - and the  focus of this discussion - is the definition of availability.

In their paper, Gilber & Lynch define availability as:

> For a distributed system to be continuously available, every request received by a non-failing node in the system must result in a response.4 That is, any algorithm used by the service must eventually terminate. In some ways this is a weak definition of availability: it puts no bound on how long the algorithm may run before terminating, and therefore allows unbounded computation. On the other hand, when qualified by the need for partition tolerance, this can be seen as a strong definition of availability: even when severe network failures occur, every request must terminate.

Based on the above definition, Paxos is not available under network partition. In other words, Paxos is CP. This is because under network partition, nodes belonging to the minority partition cannot reach and consensus and make progress. Since a request received by these nodes cannot be served, Paxos is not available under the _strong definition of availability_. In practice, Paxos "maximizes the efficiency of availability" while being consistent.

PS - Perhaps calling Paxos as CP is also not correct since nodes of minor partition fall behind nodes of major partition.

__References__

*  Brewer's conjecture and the feasibility of consistent, available, partition-tolerant web services - Gilbert & Lynch
*  <a href="https://hackernoon.com/exploring-distributed-system-theory-availability-and-consistency-e8c59e0875cd" target="_blank">Exploring Distributed System Theory: Availability and Consistency</a>
