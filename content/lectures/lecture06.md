---
title: "06 - Replication"
geekdocDescription: ""
keywords: ["replication", "distributed system", "consistency", "omscs"]
---

# Replication

### Goals of Replication
* State available at more than one node => Fault-tolerance, Availability
* Service can be provided from more than one node => Scalability

### Replication Modes
* Active Replication (all replicas read, write and update each other)
* Stand-by (Primary-backup) Replication (Only one replica reads and writes, rest just follow updates from primary)

### Replication Techniques
| State Replication                                      | Log Replication or Replicated State Machine                 |
| ------------------------------------------------------ | ----------------------------------------------------------- |
| Change in state is sent to other replicas              | The operation (event) is executed(applied) to every replica |
| + No need to execute operation again                   | + No need to transmit large state delta                     |
| - Determining change in state can be complex and large | - Operation needs to be executed and must be deterministic  |

### Replication and Consensus

* When data is replicated over multiple nodes various design choices have to be made
  * Synchronous vs Asynchronous replication
  * Leader based , Multi-leader or Leader-less
  * Strong vs Weak consistency
* __Question:__ Does synchronous replication imply strong consistency and asynchronous replication imply weak consistency?
* Consensus based replication algorithms are used to build ` CP ` systems i.e. it provides `strong` consistency under network partition.

### Chain Replication

* Lag in replication increases linearly with number of replicas
* Another option (van Renesse, Schneider, OSDI'14) is to do a synchronous replication.