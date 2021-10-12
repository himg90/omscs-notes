---
title: "L04 - Consistent Global States"
geekdocDescription : "Fundamental Concepts and Mechanisms"
---

# Consistent Global States of Distributed Systems Fundamental Concepts and Mechanisms

[Link to Paper](http://www.cs.cornell.edu/courses/cs5414/2012fa/publications/BM93.pdf)

## 1 Introduction

> What is global state of a distributed system?
> 
> 
> It is the union of the states of the individual processes.
> 
> **Why do we study construction of global state?**
> 
> *A large class of problems in distributed computing can be cast as executing some action when the state of the system satisfies a particular condition. Thus, the ability to construct a global state and evaluate a predicate over such a state constitutes the core of solutions to many problems in distributed computing*
> 
> **Why is it a hard problem?**
> 
> *uncertainties due to message delays and relative computation speeds prevent a process from drawing conclusions about the instantaneous global state of the system to which it belongs.*
> 
> .separate process [can] construct [consistent] global states but still reach conflicting conclusions regarding some global property. This "relativistic effect" is inherent to all distributed computations and limits the class of system properties that can be effectively detected.
> 
> **Contents of Paper**
> 

## 2 Asynchronous Model of Distributed System:

- Collection of sequential processes with unbounded relative speeds of computation
- Network
    - every process can communicate with rest of the processes
    - message delays are unbounded and unpredictable ⇒ out-of-order message

## 3 Distributed Computations

- The activity of each sequential process is modeled as executing a sequence of events
    - Event can be either (1) internal event, (2) send(m) event or (3) receive(m) event
- The **local history** of process Pi during the computation is a (possibly infinite) sequence of events hik = ei1 ei2...eik.
    - Note the local history has a ***total order***
- The **global history** of the computation is a set H = h1 ∪ … ∪ hn containing all of it's events.
    - Note the global history does **not** have ***total order***
- Formally, a distributed computation is a partially ordered set (poset) defined by the pair (H, →).
- **Space-time Diagram**
    - Arrows represent message passing
    - Horizontal axis is time
    - Note there are three kinds of events: internal, send(m), receive(m)
    - Some events are related by happened before (→)relationship, others are concurrent (||)

## 

## 4 Global States, Cuts and Runs

> 
> 
> 
> ![image4.png](/images/cs7210/readings/w03_consistent_global_states/image4.png)
> 
> ![Consistent%20Global%20States%20of%20Distributed%20Systems_%20F%2038e03beb575b41eab2d54316537f6a43/image2.png](/images/cs7210/readings/w03_consistent_global_states/image2.png)
> 

## 5 Monitoring Distributed Computation

- First strategy towards constructing global state
- **Active Monitor**: A single process (called monitor/observer) periodically polls all the process of distributed system for their local states. Once it receives response from all processes, it constructs a global state.
- Problem with this approach:
    - Suppose some process P1 sends a message to another process P2. The local states obtained by the *monitor process* may be such that send(msg) on P1 is not captured by the monitor but recv(msg) on P2 is captured. But a send event always happens before the recv event.
    - A global state constructed using this strategy may not be possible in reality under any circumstances.
    - Thus, any property/predicate calculated from this global state will be incorrect.
- Example showing effect of this problem: Detecting deadlock in a distributed system consisting of single threaded process communicating with each other.
    - We construct a dependency graph of processes waiting on each other.
    - If a cycle exists in graph, then we have a deadlock
    - Due to the problem discussed above, we may see a deadlock when it does not exist and vice versa

## 6 Consistency

- Causal Precedence:
    - A cut C is consistent if for any event e in C, C also contains any event e' which "happened before" e
      
        ![image3.png](/images/cs7210/readings/w03_consistent_global_states/image3.png)
        
    - In **Space-Time Diagram**: if all arrows that intersect the cut have their bases to the left and heads to the right of it, then the cut is consistent; otherwise it is inconsistent
    - Just as a scalar time value denotes a particular instant during a sequential computation, the frontier of a consistent cut establishes an “instant” during a distributed computation.
    - A run *R* is said to be consistent if for all events, e → e' implies that e appears before e' in *R*

## 7 Observing Distributed Computation

- In this section, we look at the second strategy to build distribute state: **The Passive Observer**
- All processes in the system send *updates* about every local event to the passive observer. The passive observer constructs a global state from these events.
- This global state may be consistent, inconsistent or outright invalid since updates can arrive at the passive observer in any order. eg. recv(msg) may arrive before send(msg), two internal events on the same process may also arrive out of order.
- To ensure that this strategy produces only consistent global state, we need to ensure two conditions

> Condition 1: order of the events is correct i.e. if e → e', then e should appear before e' in observer's history
> 
> 
> Condition 2: completeness of events i.e. if e → e' and e' is observed then e must be observed as well.
> 

## 8 Logical Clocks

- Continuing the discussion of previous section
- If every process maintains scalar logical clock and stamps *updates* to the observer with logical-event-timestamp, it is possible to meet condition 1 by sorting events based on this logical timestamp at the Observer.
- To meet condition 2, we need to impose a constraint on channel between pairs of processes. The channel needs to be FIFO. This ensures that *updates* from a process arrive in order and are not dropped. If we can say that latest event from each process has timestamp greater than T, then events up to T are complete on the Observer.

## 9 Causal Delivery

- This extends the concept of FIFO channels between pairs of processes
- Events sent from all the processes to the observer are delivered to the observer in an order which respects partial ordering of events i.e. events of a single process are totally ordered + send() and corresponding recv() events are correct ordered.
- Delivering events to the observer while respecting above property is called Causal Delivery

## 10 Constructing the Causal Precedence Relation

- Definition: Strong Clock Condition e → e' ⇔ T(e) < T(e')
- Vector Clocks satisfy Strong Clock Condition

## 11 Implementing Causal Delivery with Vector Clocks