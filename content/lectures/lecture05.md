---
title: "05 - Consensus in Distributed Systems"
geekdocDescription: ""
keywords: ["distributed computing", "omscs", "consensus"]
---
[Link to Lecture Slides](https://gatech.instructure.com/courses/220502/files/24077401/download?download_frd=1)

[Paper Trail: A Brief Tour of FLP Impossibility](https://www.the-paper-trail.org/post/2008-08-13-a-brief-tour-of-flp-impossibility/)

## Consensus

- What is Consensus?
    - Ability of processes in a distributed processes to agree on something like
        - the value of a variable
        - current timestamp or point of computation
        - taking an action etc.
- Why is it important?
    - Important to make forward progress in distributed system.
    - Reaching a consensus makes it possible for the system to be correct.
- Why is it hard?
    - lack of global clock, network delays, faulty or malicious nodes etc.
- Key Properties
    - **Liveness/Termination:** All non-faulty processes **eventually** decide on a value
    - **Validity/Safety:** The decided value must be **proposed by** one of the processes
    - **Agreement/Safety:** All processes must decide on the **same (single) value**

## System Model

- Asynchronous Model
    - Channel: Unpredictable delays and message reordering but message are not corrupted
- Failure Model
    - At most one faulty processor
    - Fail-stop failure model: Faulty process stops/dies. No malicious/random behaviour

# Definitions

- **Admissible run:** Run with 1 faulty processor and all messages eventually delivered (matches system model)
- **Deciding run:** Admissible run where ***some*** non-faulty processes reach a decision
- **Totally Correct Consensus Protocol:** If all admissible runs are also deciding runs
- **Univalent configuration:** Configuration of the system in which the system can reach a single value
- **Bivalent Configuration:** Configuration of the system in which it can reach more than one decisions

## FLP Theorem

- Paper: Impossibility of Distributed Consensus with One Faulty Process
- In a system with one faulty process, no consensus protocol can be totally correct.
- System Model: Asynchronous Model, One fail-stop faulty process
- Intuition of the Proof:
    - Consider a simple system model with just 1 faulty process which fail-stops.
    - Is it possible to identify a starting configuration and legitimate admissible run such that system does not reach a deciding state

> OR
- Whether is it always possible to identify one admission schedule in the system with one faulty process where all messages are delivered and the system remains in bivalent configuration.

## Proof in a Nutshell