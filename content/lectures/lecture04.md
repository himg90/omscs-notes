---
title: "04 - State in D. Systems"
geekdocDescription: ""
keywords: ["distributed computing", "omscs", "distributed snapshot"]
---

# State in Distributed Systems



### Terminology

- The **Global State** of the system is the union of `Process States` and `Channel States` 
- __Channel State:__ The **State** of a channel between two processes is defined by the set of msgs sent from sender process but not delivered to receiver process.
- __Process State:__ Context stored on a client for eg variables, data structures etc.
- __Process History:__ Sequence of events that occured on a process in distributed system.
- **Cut** : Set of histories of all processes in distributed. The history of all processes may not be taken at the same exact time instant. Note, cut is a set of histories, it does not say anything about order of events on two different processes.
- **Run**: A totally ordered sequence of all the events belonging to a cut. Multiple runs can correspond to same cut.
- __Consistent Cut:__ A cut is consistent if - "For any event `e1` contained in that cut, all events that "happened before" `e1` are also present in the cut." Another way to say it, a consistent cut represents a _possible_ global state. It respects the partial order of events.
- __Pre-Recording Event:__ Events occured before the time of snapshot.
- __Post-Recording Event:__ Events occurred after the snapshot is recorded.



### Challenges in capturing Global State of a Distributed Systems

- Instantaneous recording not possible
    - **No Global Clock**: As a result, processes cannot capture individual states at exact same time.
    - **Unreliable Network**: As a result, a node cannot invoke every other node to record state instantaneously
- Deterministic vs Non-Deterministic Computation
    - Distributed computation is non-deterministic unlike a single threaded stand alone program.

### System Model

- Channels are FIFO. It's possible to build one. For eg. TCP protocol.

## Finding a Consistent Cut: Algorithm in Action

- Goal: Capture a **consistent** global state (process states + channel states)
- **State of channel**: Consider a scenario where we have a global clock. We are evaluating a system of two process : p and q. p sends msg to q at time t1. q receives msg from p at t3. We take the snapshot at time t2 such that t3 > t2 > t1. The state of channel from p to q will contain the inflight msg.

## Chandy and Lamport Snapshot Algorithm

- Snapshot (Global State) == State of Processes + State of Channels

- State of process is captured when it receives the marker token for the first time.

- Whenever a process captures it's state, it sends out marker tokens to all other processes. So that they can capture the state of channel from the process to them.

- __State of a channel__, Ch(i to j), is consists of all events received by Pj which were sent by Pi from the time Pj captured it's local state till a marker token is received from Pi.

    

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
- Characteristics of the algorithm
    - The snapshot algo doesn't interfere with the normal execution of the processes
    - Each process in the system records its local state and the state of its incoming channels

#### Features of *Chandy and Lamport Algorithm*:

- It gives us a __consistent snapshot__
- The recorded Global State **may not correspond to a state which actually existed**  
- The state, however, does **represent a possible global state** (a vertex in state lattice diagram from the paper)

## Properties of the Global State

![Lecture%204_%20State%20in%20Distributed%20Systems%2098b0ec24143549b0bb67a869a2ae1c7c/image1.png](/images/cs7210/lectures/lecture04/image1.png)

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