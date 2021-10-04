---
title: "04 - State in D. Systems"
geekdocDescription: ""
keywords: ["distributed computing", "omscs", "distributed snapshot"]
---

## Introduction

- Study challenges in maintaining state in a distributed system.
- Study capturing of snapshot of the global state of the Distributed System.
- This helps to understand what's happening in the system and to calculate its properties
- Chandy - Lamport Algorithm

## Global State, Snapshots and Other Terminology

![Lecture%204_%20State%20in%20Distributed%20Systems%2098b0ec24143549b0bb67a869a2ae1c7c/image2.png](/cs7210/lectures/lecture04/image2.png)

- Distributed system consists of a set of **processes** and communication **channels** between them.
- A **Process** has a series of events occurring. These events can be of three types:

> 1. send(msg)2. recv(msg)3. internal event
- **Process State** is defined by history of events on that process
- **Channel State** is defined by inflight message
- **Global State** ⇒ (all Process States + all Channel States)
- **Process History, h:** [-∞, .., e]
- **Cut** : h1 ⋃ h2 ⋃ ... ⋃ hn
- Actual and Observed Run can be different
- **Run**: Ordered sequence of events of a Cut
- Consistent Cut
- Snapshot
- Pre-Recording Event: Events occured before the time of snapshot.
- Post-Recording Event: Events occurred after the snapshot is recorded.

## Challenges About State in Distributed Systems

- Instantaneous recording not possible
    - **No Global Clock**: As a result, processes cannot capture individual states at exact same time.
    - **Unreliable Network**: As a result, a node cannot invoke every other node to record state instantaneously
- Deterministic vs Non-Deterministic Computation
    - Distributed computation is non-deterministic unlike a single threaded stand alone program.

## System Model

- Processes
- Channels:
    - Directed, process-to-process
    - FIFO
    - Error free (Messages are not corrupted)
- While normal channels are not FIFO and error free. It's possible to build one. For eg. TCP protocol.

## Finding a Consistent Cut: Algorithm in Action

- Goal: Capture a **consistent** global state (process states + channel states)
- **State of channel**: Consider a scenario where we have a global clock. We are evaluating a system of two process : p and q. p sends msg to q at time t1. q receives msg from p at t3. We take the snapshot at time t2 such that t3 > t2 > t1. The state of channel from p to q will contain the inflight msg.

## Snapshot Algorithm

- Initiator
    - Save it's local state
    - Send marker tokens on all outgoing edges
- All Other Processes: On receiving the first marker on any incoming edge
    - Save state, and propagate markers on all outgoing edges
    - Resume Execution but also save incoming messages until a marker arrives through the channel

```bash
handleMarker (channel c, Marker m) {
		if local_state not recorded:
				record local_state
				state(c) = EMPTY
				propage m
		else
				state(c) = msg received on c since local_state was recorded
}
```

Guarantees a consistent global state!

- Assumptions of the algorithm
    - Communication channel is FIFO
    - There are no failures and all messages arrive intact and only once
    - The snapshot algo doesn't interfere with the normal execution of the processes
    - Each process in the system records its local state and the state of its incoming channels

## Global State

Features of *Chandy and Lamport Algorithm*:

- Does not promise to give us exactly what is there but it gives us a consistent state (**Not Real-time snapshot**)
- The recorded Global State **may not correspond to a state that ever really happened** at some global time
- The state, however, does **represent a possible global state** (a vertex in state lattice diagram from the paper)
- The observed state respects all the partial order of events imposed by causality.

## Properties of the Global State

![Lecture%204_%20State%20in%20Distributed%20Systems%2098b0ec24143549b0bb67a869a2ae1c7c/image1.png](/cs7210/lectures/lecture04/image1.png)

## Benefits of Global State: Evaluate Stable Properties

- What good are "recorded states" if they never really occur?
- They can still be useful for measuring stable properties.
- A property is called a stable property iff once it becomes true in a state S, it remains true for all states S’ reachable from S. Note if stable property is false, we can't say anything about it in future states.
- For example:
    - Deadlock,
    - If computation is complete in recorded state, then it's also complete in actual terminal state

## Definite vs Possible State

- Certain unstable properties may also be important. For an unstable prop, there is no guarantee that once it becomes true it remains true forever. eg. buffer overflow, race condition.
- Is evaluating unstable properties on "recorded state" useful?
- Yes. If a undesirable unstable property was true in "recorded state", then we know that that property can be true under some conditions in the system. This is useful. We now need to either handle such cases or change system in a way that those conditions do not happen.