---
title: "03 - Time in D. Systems"
geekdocDescription: ""
keywords: ["distributed computing", "omscs", "logical time", "vector clock"]
---
# Time in Distributed Systems

## Introduction

- Distributed Systems cannot rely on Physical Clocks so they rely on Logical Clocks
- Reason: Physical clocks of individual processes need to be synchronized but perfect synchronization is not possible.

## Why Do We Need Time

- There are many scenarios in which knowing the time can be helpful. For eg. checking the correctness of the program. Same operations executed in different
    - Determining the order of operations executing in the system
    - Checking correctness of the final state with the expected final state

## Why Is Measuring Time Hard in DS?

- Option 1: We can use the local clock on each node to record the time
    - Good: Since each message is timestamped, there is a global order of all the events
    - Bad: It's impossible to precisely synchronise all the local clocks
- Option 2: Let there be a receiver/observer node. Every node in the system sends it a message when an event occurs on that node.
    - Due to unpredictable delays, message can arrive at the observer out of order
    - Different Observers can report different order of events

## Logical Time

- Logical clocks, unlike physical clocks, progress only when an event occurs and provides a distinct timestamp to each event.
- Types of clocks (details left out in lecture)
    - Scalar Clock / Lamport Clocks
    - Vector Clocks
    - Matrix Clocks

## Ordering Relationship Among Events

- There are three type of events occur on a node: send(msg), recv(msg) and internal events.
- Note that when a msg is send from node n1 to node n2, two events occur
  1. n1 registers send(msg) event
  2. n2 registers recv(msg) event.
- e1 → e2 implies, e1 "happened before" e2
- All events that take place on a single node are totally ordered
- If a node receives a msg (e1) and sends it (e2) , then e1 → e2
- If a node sends a msg (e1) and it is recvd by another node(e2), then e1 → e2

## Concurrent Events

- If e1 and e2 are not related by "happened before" relationship, we call them concurrent. It means that we cannot conclusively state the order in which order those events occurred. 
- Notation: e1 || e2

## Logical Clocks

* Logical clocks are used to assign timestamps to events.
* A logical clock defines a set of rules on how to advance it and how to compare two timestamps.

- __Monotonicity property:__ A logical clock must always increase
- __Clock Consistency Condition__
    - If e1 → e2 then, C(e1) < C(e2)
    - Note that converse is __not__ true i.e. C(e1) < C(e2) does __not__ imply e1 → e2
- For concurrent events, there are no guarantees. The logical timestamps can be in any order or they simply may not be comparable.
- __Strong Clock Consistency__ (not mandatory for all logical clocks)
    - e1 → e2 ⇔ C(e1) < C(e2)

## Lamport's Scalar Clock

- Each process had it's own local logical clock
- Rules

![Lamport Scalar Clock](/images/cs7210/lectures/lecture03/image6.png)

## Vector Clocks (vt)

- Lamport Clock does not satisfy the Strong Clock Consistency
- Length of vector clock is equal to number of processes in the system
- If vt is the vector clock at a process, then vt[i] represents that process's view of time of ith process.
- Rules
    ![Vector Clocks 1](/images/cs7210/lectures/lecture03/image5.png)
    
- Comparison Rules

![Vector Clocks 1](/images/cs7210/lectures/lecture03/image4.png)

## Matrix Clocks

- Each process maintain N x N matrix
- Consider process Pi,
    - i-th row of matrix corresponds to Pi's own vector clock
    - j-th row (other than i) corresponds to Pi's view of Pj 's vector clock
- Benefits
    - A process can know if the vector clock of every other process has progressed past a certain time, t.
    - This can be used to delete any data which is cached for processed which are falling behind.