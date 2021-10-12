---
title: "05 - Consensus"
geekdocDescription: ""
keywords: ["distributed computing", "omscs", "consensus"]
---
# Consensus

[Link to Lecture Slides](https://gatech.instructure.com/courses/220502/files/24077401/download?download_frd=1)

[Paper Trail: A Brief Tour of FLP Impossibility](https://www.the-paper-trail.org/post/2008-08-13-a-brief-tour-of-flp-impossibility/)

## Consensus

- What is Consensus?
    - Ability of processes in a distributed processes to agree on something like the value of a variable, taking an action etc.

> The problem of consensus is genuinely fundamental to distributed systems research. Getting distributed processors to agree on a value has many, many applications. For example, the problem of deciding whether to commit a transaction to a database could be decided by a consensus algorithm between a majority of replicas. You can think of consensus as ‘voting’ for a value.

[PaperTrail: A Brief Tour of FLP Impossibility](https://www.the-paper-trail.org/post/2008-08-13-a-brief-tour-of-flp-impossibility/)

- Key Properties
    - **Liveness/Termination:** All non-faulty processes eventually decide on a value
    - **Validity/Safety:** The decided value must be proposed by one of the processes
    - **Agreement/Safety:** All processes must decide on the same (single) value

## System Model

- Asynchronous Model
    - There is no upper bound on the amount of time processors may take to receive, process and respond to an incoming message.
    - Channel: Unpredictable delays and message reordering but message are not corrupted
- Failure Model
    - At most one faulty processor
    - Fail-stop failure model: Faulty process stops/dies. No malicious/random behaviour

## Definitions

- **Admissible run:** Run with 1 faulty processor and all messages eventually delivered (matches system model)
- **Deciding run:** Admissible run where ***some*** non-faulty processes reach a decision
- **Totally Correct Consensus Protocol:** If all admissible runs are also deciding runs
- **Univalent configuration:** Configuration of the system in which the system can reach a single value
- **Bivalent Configuration:** Configuration of the system in which it can reach more than one decisions

## FLP Theorem

- System Model: 
    - At most one fail-stop faulty process
    - 
- In a system with one faulty process, no consensus protocol can be totally correct.
- Intuition of the Proof:
    - Consider a simple system model with just 1 faulty process which fail-stops.
    - Is it possible to identify a starting configuration and legitimate admissible run such that system does not reach a deciding state

> OR
- Whether is it always possible to identify one admission schedule in the system with one faulty process where all messages are delivered and the system remains in bivalent configuration.

## Is Consensus Really Impossible?

* Algorithms like 2PC, 3PC, Paxos, Raft etc. change some of the __assumptions__ and **system properties**



## Excerpts

[PaperTrail: A Brief Tour of FLP Impossibility](https://www.the-paper-trail.org/post/2008-08-13-a-brief-tour-of-flp-impossibility/)

The ‘FLP result’ settled a dispute that had been ongoing in distributed systems for the previous five to ten years. The problem of consensus - that is, getting a distributed network of processors to agree on a common value - was known to be solvable in a synchronous setting, where processes could proceed in simultaneous steps. In particular, the synchronous solution was resilient to faults, where processors crash and take no further part in the computation. Informally, synchronous models allow failures to be detected by waiting one entire step length for a reply from a processor, and presuming that it has crashed if no reply is received.

This kind of failure detection is impossible in an asynchronous setting, where there are no bounds on the amount of time a processor might take to complete its work and then respond with a message. Therefore it’s not possible to say whether a processor has crashed or is simply taking a long time to respond. The FLP result shows that in an asynchronous setting, where only one processor might crash, there is no distributed algorithm that solves the consensus problem.