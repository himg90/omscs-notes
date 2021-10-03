# Consistency Tradeoffs in Modern Distributed Database System Design

#### By Daniel J. Abadi

The main point that paper tries to put across is this:

> Modern DDBS do not provide strong consistency guarantees under normal operations (absense of any faults) not because of CAP theorem but because of consistency-vs-Latency tradeoff. Consistency-vs-Latency tradeoff is more fundamental than CAP theorem because faults like network partitions are usually rare.



* CAP only puts restrictions on DDBS under certain types of failures. During normal functioning, DDBS can work without constraints. In other words, until a partition occurs, the database can be both consistent and available. 
* If a DDBS provides lower consistency under normal operation, it does so due to the tradeoff between Latency and Consistency instead of CAP theorem
* In CAP's proof, authors use "Linearizability" form of consistency



## Data Replication

It can be done by three means

1. Leaderless Replication
2. Single Leader Replication
3. Multi-Leader Replication or Master-Master Replication

In all forms of replication, there is tension between consistency and latency.

* If synchronous replication is used between nodes, consistency improves but cost is paid in latency
* Asynchronous replication is faster but replicas can diverge if different nodes apply changes in different orders
* In asynchronous replication, if nodes try to decide on a global order of events, latency increases.



Example: Amazon's Dynamo

* A single key is stored at multiple nodes. One of these nodes becomes master for an operation and drives it. => Multi-Leader
* The leader propagates the operation to other owners of the key. First `W` writers are updated synchronously while the rest are updated asynchronously.



## PACELC

```
if (Partition)
	Availability vs Consistency
Else
	Latency vs Consistency
```

Example: MongoDB : PA/EC (Under `Partition`, it's `Available` `ELSE` it's `Consistent`)

* Under normal operation, single master and linearisable system
* Under partition, all nodes accept writes and thus system stays available but in consistent.

