---
title: "01 - Introduction"
geekdocDescription: ""
keywords: ["cs7210", "distributed computing", "omscs"]

date: 2021-10-10T16:16:01+05:30
draft: false
---

# Introduction to Distributed Systems



__What is a Distributed System?__

A collection of independent and autonomous processes(nodes) that interact by exchanging messages via interconnection network and appear to external users as a single coherent computing facility. 

The phrase "single coherent computing facility" means that there is a common goal which the processes are trying to achieve together.

__Simple Model__

* There nodes which communicate by exchaning messages.
* Communication channels are unidirectional. 
* Channels are unreliable.

__Complex Model__

* Nodes have state which may change in response to events received by the node

__Importance of Models in Distributed Systems__

* Analyzing distributed systems based on models is more predictable.
* Other option would be to prototype ideas and run exhaustive tests. Testing all possible scenarios in a distributed system is quite difficult.

__Components of a Model__

* System Elements i.e. nodes, network
* Rules eg. nodes communicate by sending msg
* Invariats/Assumptions: eg. network delivers msgs in FIFO

__How Good is a Model?__

* _Accurate_ representation of problem
* Tractable: easy to prototype and analyze

__Challenges in Distributed System__

* Asynchrony : Instant msg deliver vs bounded delay vs infinte delay
* Failures: Failstop vs transient vs byzantine failure
* Consistency: We want single up-to-date copy of data. This is complicated by concurrency, ordering, replication etc.

__Fallacies of Distributed Computing__
The fallacies of distributed computing are a set of assertions made by L Peter Deutsch and others at Sun Microsystems describing false assumptions that programmers new to distributed applications invariably make.

* The network is reliable.
* Latency is zero.
* Bandwidth is infinite.
* The network is secure.
* Topology doesn't change.
* There is one administrator.
* Transport cost is zero.
* The network is homogeneous.

__Properties of a Distributed System__

* Consistency: Provide correct answers
* High Availability
* Fault Tolerance

__Correctness of Distributed System__

* If input to distributed system is equivalent to the input to a single system performing the same function, the output of the two should be equivalent.
* To comment on whether the inputs are equivalent, we need to talk about the event ordering in distributed system. This is complicated by the fact that there is no global time available. Thus, different interpretations of event ordering can be made. We have to answer the question if all the nodes will even see the same order of events? 

![correctness](/images/cs7210/lectures/lecture01/correctness.png)



__CAP Theorem__

Under network partition, a distributed system can be either available or consistent.

