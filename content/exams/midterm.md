---
title: "Midterm"
geekdocDescription: "Review Questions"
keywords: ["distributed computing", "omscs"]

date: 2021-10-10T10:47:36+05:30
draft: true
---

# Midterm Exam Review Questions

## Lecture 01. Intro
__1. What are defining characteristics of a distributed system?__

* A collection of independent autonomous

__2. What are some simple models to represent a DS? Why would you choose to use a system model? __
__3. What do we mean by asynchronous? What about asynchrony makes distributed computing hard?__
__4. What about failures makes distributed computing hard?__
__5. Why is consistency a hard problem in distributed computing?__
__6. Pick one of the Fallacies. Can you explain why it’s not true and how does it make distributed computing more challenging?__
__7. Can you explain “The system gives correct answers always, regardless of failures”? What does it mean with respect to the required properties of the system?__
__8. How does consideration of Latency relate to the observations made by CAP?__


## Lecture 02. RPC Primer
__1. Can you identify and describe the main elements and steps involved in a distributed RPC system?__
__2. Contrast exactly once, at most once, at least once, invocation semantics – what would be required from the RPC runtime, under which assumptions would such invocation semantics be feasible (or not)…__


## Lecture 03. Time
__1. Why is time important and hard in distributed systems?__
__2. What is a logical clock? What are logical clocks used for? How is a logical clock related to physical clock?__
__3. What is required to implement a logical clock? What properties must be followed for this function to behave like a clock? Why/What’s the meaning of the Clock Conditions?__
__4. Contrast Scalar, Vector, Matrix clocks? What is the cost or overhead associated with each? What is the benefit from each of those/what is the main challenge addressed by introducing each of these/what are the properties provided by each clock?__
__5. Can you look at the example execution shown in the papers/video and explain how these clocks are getting updated? Can you point out what you are able to tell about some specific events based on the clock values alone?__


## Lecture 04. State
__1. Understand the meaning and purpose of the concepts distributed system state, consistent cut/snapshot, actual/observed run, …__
__2. Understand the snapshot algorithm, what are the assumptions under which it’s valid, why are those assumptions necessary/how are they reflected in the algorithm?__
__3. Can you trace through an execution the consistent state that could be captured by the algorithm?__
__4. By knowing the state of a property in a given state in the system, what can we tell about that same property at the start/at the end of the execution? Can you provide examples when this would be useful?__

## Lecture 05. Consensus
__1. What is consensus? Explain in your own words/understand all elements of the definition in the papers/video__
__2. What is the goal of the FLP work, what did they want to learn/prove? Provide intuition about the approach they took to achieve this goal.__
__3. State the FLP theorem and provide intuition of the proof/do you understand it?__
__4. What’s the intuition about the significance of FLP, in light of much other work of consensus algorithms, replication protocols, …__

## Lecture 06. Replication
__1. Contrast active vs. stand-by replication__
__2. Contrast state vs. log/RSM replication__
__3. What are the problems addressed by chain replication? How are they addressed?__
__4. What are the problems created by chain replication? How are they addressed in CRAQ (high level)? Are chain replication and CRAQ equivalent from the consistency guarantees they provide (i.e., guarantee when writes become visible)? Can you explain the result from the experimental comparison of CR and CRAQ?__

## Lecture 07. Fault Tolerance
__1. What’s the main idea of rollback-recovery as a FT technique?__
__2. what are the differences and tradeoffs of checkpointing vs. logging as a FT technique? What are all the different metrics you’d need to think about when comparing the two?__
__3. Describe and explain the pros/cons/tradeoffs of coordinated, uncoordinated, communication-induced checkpointing.__
__4. We mention a number of factors which influence the choice of a FT technique. Can you reason through some examples, say considering change in storage cost, or system scale, or read/write ratio of the workload, and whether or how those changes would impact the winner among any two of the techniques we discussed?__


## Lecture 08. Paxos & Friends
__1. Main idea and limitations of 2PC and 3PC.__
__2. Some history behind PAXOS__
__3. Description of PAXOS? What’s expected from the system (the system model) for the protocol to hold?__
__4. What’s hard about determining whom to follow?__
__5. Main ideas that PAXOS assumes: state-machine replication, majority quorum, …__
__6. Goal and description of the phases in Paxos__
__7. What’s the functionality Paxos provides? Why we need Multi-Paxos?__
__8. Motivation for Raft, key idea and differences than Paxos__
__9. Log management in Raft – how is info used to update, discard entries, trim/garbage collection?__


## Lecture 09. Distributed Transactions
__1. What is the concept of TrueTime? How is that related to the Logical Clocks discussed earlier in the semester? How can it be implemented?__
__2. How does the use of TrueTime change what is required from the system (Spanner) in order to establish ordering among transactions?__
__3. Using TrueTime how do you establish ordering of write operations? How do you implement a read operation? What about reading a snapshot “in the past”?__
__4. Describe at a high-level Spanner, how does it organize and replicate data, who is involved in serving read or write operations…__
__5. Describe at a high-level Aurora, how does it organize and replicate data, who is involved in serving read or write operations…__
__6. Can you explain/justify the differences in the two systems. You can consider a number of possible dimensions such as different design goals, underlying assumptions, performance, …__
__7. What are some other design points that can be considered in systems which don’t have TT?__


## Other
__1. What are the names of some of the authors of the papers we talked about? Do you know where they’re now, what they’re famous for, what else they’ve done?__
__2. Think of some distributed service you use – daily (cloud mail, search, social networks, some service at your work, …). Make an assumption on how they implement some aspect of the distributed system (from Time to Distributed Transactions) and think through the pros/cons of that design decision based on what you assume the system and workload look like__