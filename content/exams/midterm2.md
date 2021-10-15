---
title: "Midterm2"
geekdocDescription: ""
keywords: ["cs7210", "distributed computing", "omscs"]

date: 2021-10-12T16:55:27+05:30
draft: true
---

# Review Questions

### Part #1: Introduction

**What are the defining characteristics of a distributed system?**

* Collection of independent processes
* interact by exchanging messages via an interconnection network
* appear as a single coherent computing facility to external users, has common goals that all units must accomplish


2. What are some simple models to represent a DS? Why would you choose to use a system model? Using a simple model, can you describe how
>=2 nodes connected via uni-directional communication channels; send and receive messages for communication
>=2 nodes connected via uni-directional communication channels; send and receive messages for communication; each node contributes to the overall system state S
Because we have difficulty putting together nodes to fully investigate the behaviours of distributed systems using practical prototype evaluations.

3. What do we mean by asynchronous? What about asynchrony makes distributed computing hard?
unpredictable/might take infinite time to send/receive messages
Unpredictable/Unbound process execution speeds. 
Such properties of message latency will have implications on system design.

4. What about failures makes distributed computing hard?
There can be three types of failure: failstop/transient/byzantine and then figuring out which component failed (individual server/process or network)

5. Why is consistency a hard problem in distributed computing?
Need to have single up-to-date data/state. Different factors including concurrency, ordering, replication and caching introduce trade offs

6. Pick one of the Fallacies. Can you explain why it’s not true and how does it make distributed computing more challenging?
Transport cost is zero will mean it sending/receiving messages is free of cost but this is not possible as there are several costs (connection of communication infrastructure ,energy) associated to the network. We will have to guarantee there is enough capability of the network to support the bandwidth needed by us and worry about the energy cost of transmission,

7. Can you explain “The system gives correct answers always, regardless of failures”? What does it mean with respect to the required properties of the system?
Consistency, high availability, tolerant to partitions
A system should be fault-tolerant, highly available, recoverable, consistent, scalable, secure and provide predictable performance.


8. How does consideration of Latency relate to the observations made by CAP?
If partition occurs, then trade off between latency vs consistency. Slow response == no response and high latency == no availability. PACELC

02. RPC Primer
1. Can you identify and describe the main elements and steps involved in a distributed RPC system? 
Api, stub, RPC runtime, RPC compiler, service registry, interface definition language
1.implement procedure and write its specification in IDL
2.compiler specification using compiler
3. compiler generates code for the stubs and skeleton of server code
4.server process is created by adding implementation of the procedure to the skeleton and  is registered with the registry
5.client application is written according to the exposed API an compile the code together with the generated code
6.RPC runtime takes care of everything else

2. Contrast exactly once, at most once, at least once, invocation semantics – what would be required from the RPC runtime, under which assumptions would such invocation semantics be feasible (or not)… 
Practical Feasible: at most once, ideally feasible: exactly once 
Runtime requirements:
Exactly once: Timeout and retransmit, Eliminate duplicates, Blocks/fails on persistent failure of server/connection
At most once: Timeout and retransmit, Eliminate duplicates, it is known that call may not be executed
At least once: Timeout and retransmit, No guarantees duplicated will be eliminated

03. Time
1. Why is time important and hard in distributed systems?

quoted from fallacies:
1. The network is reliable.
2. Latency is zero.
3. Bandwidth is infinite.
4. The network is secure.
5. Topology doesn't change.
6. There is one administrator.
7. Transport cost is zero.
8. The network is homogeneous.




Why important:
To decide ordering and thus determine causality for the system's correctness. Need to do resource allocations such as scheduling and need to obtain objectives like fairness, priorities and other service level objectives. Can analyze the system and measure progress based on the ordering and to complete tasks like garbage collection.

Why hard:
No guarantee about global synchronous clock immediately available at each node,
Messages might take variable time to propagate in the network,
Delays may not be consistent,
There may be failures of nodes or network connection,
There may be malicious nodes

2. What is a logical clock? What are logical clocks used for? How is a logical clock related to physical clock?

Clock is used for solving the problem how to decide the ordering in the system. In big distributed system., it’s too hard to implement a universal consistent physical clock, so logic clock is introduced.
Logical clocks introduce the notion of virtual time as physical clocks are hard to work with. Used to generate timestamps, advance time in some manner, to maintain the order of the events in the distributed system. A logical clock maps events to a partially ordered time domain T.

3. What is required to implement a logical clock? What properties must be followed for this function to behave like a clock? Why/What’s the meaning of the Clock Conditions?

Let C be the logical clock function.
Monotonicity Property: if e1 happens before e2, C(e1) < C(e2)
This means that if an event happens before another event, the clock value for the second event must be higher than the first. The clock must be monotonically increasing.
To implement a logical clock, we need to determine the data structure to note a timestamp, and the rules needed for the clock function to satisfy at least the monotonicity property.

Data structure in local to record clock and protocol for define the condition. It must be able to capture the chronological and causal relationship between events. (Means capture happen-before relationship) 
For logical clock, we need rules to advance time (clock function) and a data structure to represent timestamps. 
Clock Consistency Conditions:
1) monotonicity property: if e1 -> e2 => C(e1) -> C(e2) ie, if event e1 happens before e2 then the timestamp of e1 < timestamp of e2. (It does not hold conversely, since it will makes every concurrent events have same clock, too hard to implement)
2) If e1 || e2 ?? C(e1) ?? C(e2) ie, if event e1 is concurrent to e2, then we don't know anything about their timestamps

4. Contrast Scalar, Vector, Matrix clocks? What is the cost or overhead associated with each? What is the benefit from each of those/what is the main challenge addressed by introducing each of these/what are the properties provided by each clock? 
Lamport clock is scalar, consistent, not strongly consistent. Benefit: sufficient for correctness, overhead O(1)
Vector clock, overhead O(n) where n is the number of nodes in the system. Benefit: strongly consistent, thus better efficiency
Matrix clock, overhead O(n2), complicated, each process maintains its view about every other process’ view of the global time     Benefit: can be used to implement garbage collection

5. Can you look at the example execution shown in the papers/video and explain how these clocks are getting updated? Can you point out what you are able to tell about some specific events based on the clock values alone?





04. State
1. Understand the meaning and purpose of the concepts: distributed system state, consistent cut/snapshot, actual/observed run, … 

Distributed system state means the states of processes and channels; process state is defined by most recent event while channel state is defined by inflight messages

Run means a sequence of events that took place in the system. If the sequence correspond to a schedule of events that indeed happened, it is Actual run; If the sequence correspond to some legitimately possible ordering of events (not necessarily the real one), it is Observed run,

A cut of a distributed computation is a subset of its global history and contains an initial prefix of each of the local histories. Consistent cut corresponds to a snapshot of the system, which corresponds to some possible point in the execution which has a consistent ordering. Consistent cut must preserve happened before the relationship of the events. If any received event is recorded in the snapshot the corresponding send event must also be recorded in the snapshot.


Snapshot is a collection of state(global state) of the system

2. Understand the snapshot algorithm, what are the assumptions under which it’s valid, why are those assumptions necessary/how are they reflected in the algorithm?
Assumptions
There are no failures and all messages arrive intact and only once: TRUE
The communication channels are unidirectional and FIFO ordered: TRUE
There is a communication path between any two processes in the system：？？？
Any process may initiate the snapshot algorithm
The snapshot algorithm does not interfere with the normal execution of the processes : TRUE
Each process in the system records its local state and the state of its incoming channels: TRUE


3. Can you trace through an execution the consistent state that could be captured by the algorithm?
Yes, CL doesn’t promise to give the exact what happened, but it does give us a consistent state of the system （consistent with the ordering imposed by message sends and receives), 

4. By knowing the state of a property in a given state in the system, what can we tell about that same property at the start/at the end of the execution? Can you provide examples when this would be useful?
If it’s a stable property, 
if it’s true at given state we see, it will remain true after (at the end of execution)
If it’s false at a certain state we see, it was false before (at the start of execution).




05. Consensus
1. What is consensus? Explain in your own words/understand all elements of the definition in the papers/video
It is an important mechanism in the Distributed system to make forward progress.
It is an agreement between various distributed processes.
 Agreement can be on:  value, action or timestamp of a particular action.
                                            	Outcome of transaction.
Example of a bank transaction between accounts on 2 different servers.
Reaching a consensus makes it possible for the system to be correct.
The ability of any distributed system to eventually reach to consensus, depends on 3 key properties:
Termination/ liveness Property
Agreement/ Safety
Validity/ Safety

2. What is the goal of the FLP work, what did they want to learn/prove? Provide intuition about the approach they took to achieve this goal.
Goal of the FLP work was to prove that it is impossible to build a distributed system for which we can guarantee that it will behave correctly. 

3. State the FLP theorem and provide intuition of the proof/do you understand it?
In a system with one fault, no consensus protocol can be totally correct.
The intuition is to study a simple system model which uses asynchronous communication, has one faulty process and uses a fail-stop model and see whether we can always find at least one admissible schedule which is not a deciding schedule. 
4. What’s the intuition about the significance of FLP, in light of much other work of consensus algorithms, replication protocols, … 
Goal of the FLP work was to prove that it is impossible to build a distributed system for which we can guarantee that it will behave correctly.
But there are other consensus protocols as well, like 2PC, 3PC, Paxos, Raft.
These protocols do not contradict FLP Theorem but change some of the initial assumptions and system properties of the system model.
These protocols let us achieve consensus and also provide us the conditions under which the protocol will terminate, or under which condition it will provide consensus. 

06. Replication
1. Contrast active vs. stand-by replication
·         Stand-by Replication also called Primary backup replication.
·         For active replication, all replicas are active at a given point of time, whereas for backup replication, only one replica is active at a given point of time.
·         In Active replication, each replica can handle read requests, whereas in stand by replication, only one replica can handle read requests at a given point of time.
·         In case of a failure, for stand-by replicas, one of the replicas needs to intervene to take over as primary. No such need in active replication.
·         In case of updates, in active replication, whenever any replica receives request for update, it is the responsibility of that replica to update the state of all other replica so that they all have a consistent data. For stand-by replication, it is the responsibility of the primary to pass the data to other replicas after each update, so that fast failover can be achieved.
·         For stand-by replication, usually primary is the leader/ coordinator, whereas for active replication, each replica can be a leader at different point of time to ensure correctness for replication, consensus and consistency management.

2. Contrast state vs. log/RSM replication 
·         State Replication: Command for update first executed at one of the replicas, and then copy the state changes to other replicas.
·         Replicated State Machines: Copy each operation (log of execution) to each replica and execute to produce the same state update.
·         Replication State machine technique can be used if the executions are deterministic.
·         No need to perform the same operation on every replica, if we are using State replication, whereas for replication state machine re-execution is required at each replica and we have to ensure deterministic execution.
·         Since we only send the statein state replication technique, sometimes it becomes difficult because the state may be large and hard to identify where all updates are, whereas in Replication state machines, since we only send the operation logs, they are simpler and may be smaller.

3. What are the problems addressed by chain replication? How are they addressed? 
Main problem addressed by the Chain replication is as the number of replica increases, there is a slowdown in the process of replication, because there are more messages required to be sent and received. At least 2 rounds of messages between the leader and other participants. This impacts scalability. 
In chain replication, the first replica is the head, which usually receives write requests and a tail which is the last replica that will only receive read requests generally. When the head receives the write request, then it propagates the message to one of its subsequent replicas only instead of taking the overhead to send the message to all the replicas.
Once, the tail receives the message for any write, at that time, the tail sends the acknowledgement once the update is executed. If the tail does not receive the write message, then that write is discarded in the system.
The reason for the read request being sent to tail is that it always has the latest update.
Chain replication facilitates more replication nodes without overload of messages for the leader, hence a better fault tolerant system. It also facilitates high write throughput, as the write is a pipeline approach, where one replica keeps pushing the message to other node. The head can accept new write messages once it pushes its message to its next node. It provides strong consistency because of the fact that the write is committed only after the message is received by the tail, and read requests are only sent to the tail.
This method should not be used in case of read intensive application, because we cannot utilize intermediate nodes for read. It has low efficiency because intermediate nodes may be underutilized.

4. What are the problems created by chain replication? How are they addressed in CRAQ (high level)? Are chain replication and CRAQ equivalent from the consistency guarantees they provide (i.e., guarantee when writes become visible)? Can you explain the result from the experimental comparison of CR and CRAQ?
Chain replication is inefficient in case of read intensive applications, because read operation can be done on tail replica only.
This problem is addressed by Chain replication Apportioned queries (CRAQ).
Apportioned means, divided among the chain replicas. The read operations are divided among several replicas.
Query just means read operation.
Write operations still are taken care of by the head. 

CRAQ maintains old and new versions of data at each replica and replica checks with the tail when both values present.

No. CR corresponds to a strong consistency model while CRAQ always returns the latest committed value and corresponds to a sequential consistency model.

The experiment shows CRAQ can scale to higher read throughput compared with CR even as the write load increases. And CRAQ throughput scales with the increase of the number of replicas in the chain.







07. Fault Tolerance
1. What’s the main idea of rollback-recovery as a FT technique?
RR assumes that the system is a collection of applications communicating through the network and all processes have a stable storage that can survive all tolerated failures. FT can be achieved by using all these devices to save recovery information periodically. Which works very well in big distributed system. (Not lock, no central control)
If failure detected, rollback to previous state, rollback any effects of exchanged messages. Once rolled back, continue execution with fault removed.

2. What are the differences and tradeoffs of checkpointing vs. logging as a FT technique? What are all the different metrics you’d need to think about when comparing the two?

Checkpoint record one state of application in durable storage. It’s a consistent state of point of system. It might need to freeze the system (coordinated checkpointing), it might lose the events if only be used. 

On the other hand, logging is trying to log event states of change (information about operations performed). The problem for logging is log could become huge, and it will take too long to recover when it’s only to be used.  
Recovery time/Amount of IO/Stop Time/Garbage Collection Time/Storage Size are the metrics that can be used to compare two.

3. Describe and explain the pros/cons/tradeoffs of coordinated, uncoordinated, communication-induced checkpointing. 
Uncoordinated  checkingpointing is easy to implement since all the nodes can be done by themselves but it has the problem of domino effects which can cause loss of work, it also can cause useless checkpoints and multiple checkpoints per process and the need for garbage collection. On the other hand, coordinated checkpointing lets processes coordinate their checkpoints to get a consistent state. Thus, recovery no longer requires a dependency graph to calculate a recovery line and has no domino effect. Also it only takes a single checkpoint per process and needs no garbage collection. However, it is hard to coordinate when there is no synchronous clock guarantee and the message delivery is not reliable or in bounded time. Sometimes unnecessary checkpoints are taken when the process doesn’t really make any change but is required to take checkpoints. Thirdly, communication-induced checkpoints piggyback the marker message in the message and encourage periodic independent checkpoints. Thus, there is no need for FIFO assumption and checkpoints will be done even if there is no communication.


4. We mention a number of factors which influence the choice of a FT technique. Can you reason through some examples, say considering change in storage cost, or system scale, or read/write ratio of the workload, and whether or how those changes would impact the winner among any two of the techniques we discussed? 
Pessimistic logging is cheap and reliable if there is hardware available that makes it cheap to write to persistent storage. 

Coordinated checkpointing is also illustrated as favorable, but it relies on some properties of the network (like FIFO-ness) to guarantee consistency, and also requires global communication with the processes involved in the system. If the system scales to be very large, then the coordination and communication involved will not scale well and become a bottleneck. 

For workloads that are heavy in writes, then a checkpointing-based mechanism might be better since a pessimistic logging based mechanism will require writing to persistent storage on every write update. However this can be slightly mitigated with specialized hardware or with optimistic logging (which writes updates to persistent storage asynchronously but comes with its own sets of problems such as potentially having to clean up orphaned processes). Workloads heavy in reads can probably be well served by both - unless a logging-based mechanism requires the traversal of the log to read any value. If this is the case, then a read-heavy workload would likely be better served by a checkpointing based scheme.

Alternate answer:
To compare FT techniques in regards to storage costs, we can consider Uncoordinated checkpointing and Causal logging. It would be unfavourable to choose Uncoordinated logging because it is possible to have a domino effect upon the failure of a node and so it would require us to keep multiple checkpoints per node in persistent storage. With Causal logging, the storage costs are more reasonable because each node would have a single checkpoint corresponding to the most recent consistent checkpoint with an additional storage cost of maintaining logs of those events which are causally related to output commits performed by the node.

For system scale, compare Pessimistic logging and Causal logging. With pessimistic logging, each node maintains its own logs and a checkpoint from which it replays upon failure. It does not need to maintain logs or state for other nodes. With Causal logging, a node would store logs for causally related events for nodes other than itself. In a large scale system where there is much communication between nodes, the cost of maintaining these logs can increase dramatically.

08. Paxos & Friends
1. Main idea and limitations of 2PC and 3PC.

Both belongs to All-Or-Nothing atomic commit protocol. They all assume a network with bounded delay and nodes with bounded response times. It might not be the case for many practical systems. 

2PC and 3PC (from lecture):  In the 2-phase commit protocol, there is always one coordinator and it is assumed that this coordinator will not fail. The coordinator proposes a value that all participants need to agree on the participants vote, the coordinator tallies the votes and then communicates this decision (commits the decision at that point). The protocol is simple, but it blocks if there are failures, so it does not guarantee liveness for sure.

The 3-phase commit protocol addresses this blocking problem. There is an initial pre-prepare phase followed by the actual prepare phase when the votes are solicited, and then, the decision phase when the decision is communicated to or eventually learned by all, if a node fails, this protocol won't block all nodes so it guarantees liveness. However, it only works with a fail stop mode, it assumes that if a node fails, it will not restart or that if a message is delayed beyond the timeout, it will not be ultimately delivered at some later time. And if that's not the case, the protocol will raise issues with safety.


2. Some history behind PAXOS

Original paper about Paxos was written in 1990 by Leslie Lamport
The paper wasn't published until eight years later in 1998
reviewers didn't appreciate the humorous description of the algorithm
The algorithm represents a set of rules. This is the protocol that needs to be followed by the participating parliamentarians in order for them to agree on a single decree and to also run the government by passing multiple decrees consecutively.
Despite the fact that Paxos was demonstrated to be practical, many reviewers felt that it is unapproachable
led Leslie Lamport to write a new paper on Paxos in 2001

3. Description of PAXOS? What’s expected from the system (the system model) for the protocol to hold?

System Model - 
Systems with asynchronous communication (operate at arbitrary speed)
Non Byzantine process failures ( fail stop and restart)
Persistent memory to retain state after starting
Description - 
State Machine Replication - 
each node is a replica of the system state
follows the same set of rules that determine how the state is updated.
Majority quorum -  
requires a majority of the participants to be part of the quorum in order to reach an agreement. it is guaranteed that two quorums will always intersect. 
This makes it safe to disseminate consensus decisions even when some notes have failed 
Works even when messages arrive out of order
Relies on timestamps for ordering messages

4. What’s hard about determining whom to follow?

 PAXOS doesn't guarantee that progress can always be made. It doesn't guarantee liveness
two proposers keep issuing a sequence of proposals with increasing numbers. If before the first value is accepted by the majority of the acceptors, or even learned by the majority of learners, the next value in the system comes in and is accepted as a proposal, then no node will receive a majority quorum for the previous value and this previous value will not be learned.
If this continues, then there is no guarantee that the system will reach a decision. There's no guarantee that the system will be making forward progress.


5. Main ideas that PAXOS assumes: state-machine replication, majority quorum, …

State Machine Replication - 
each node is a replica of the system state
follows the same set of rules that determine how the state is updated.
Majority quorum -  
requires a majority of the participants to be part of the quorum in order to reach an agreement. it is guaranteed that two quorums will always intersect. 
This makes it safe to disseminate consensus decisions even when some notes have failed 
Works even when messages arrive out of order
Relies on timestamps for ordering messages


6. Goal and description of the phases in Paxos

Prepare phase 
a Initiator/node, proposes to the rest of the participants to reach a consensus and agree upon a value (sends a propose message). The propose message is time stamped by the order number of the proposal, so proposal number one, proposal number two, and so forth.
Accept phase
The initiator, or the node gathers responses from the participants. Response messages will include information about the value that they're willing to agree to and the timestamp of the agreement proposal they're agreeing to.
Learn Phase 
Once the leading node gets sufficient nodes to agree, it communicates this information with all participants by sending them a message about the round of the agreement and the value they're agreeing to. 



7. What’s the functionality Paxos provides? Why do we need Multi-Paxos?

Paxos allows the system to agree on a single value
In practice program executions require entire sequences of updates to the application state to the database and so forth. And this is achieved by Multi Paxos. In Multi Paxos each simple single decree Paxos would be used for agreeing on individual values.



8. Motivation for Raft, key idea and differences than Paxos

Raft
Paxos
Leader based, only leader can start proposal 
Paxos is leaderless, any node can be proposer except followers
Term based, one term at most has one leader
Paxos doesn’t have concept of term, basic Paxos only works for one variable, multiple Paxos is an idea, no algorithm
Log Based, log entries record the index, command, term and additional information
Both Basic and multiple Paxos doesn’t explain how a historical proposal should be kept 
1 phase commit, leader start log copy RPC, committed after received majority ack, then reply to client。
2 phases, 1st phase start proposal, 2nd phase to commit, longer latency




The main motivation for Raft is that Paxos is quite complex for practical implementations to be accomplished based on specification. It is not very simple to understand. Raft on the other hand is easy to understand compared to paxos.
Raft has a distinct leader election phase. Like in paxos anyone can be a candidate for a leader but only a node which receives the most votes is elected to be one and the rest are followers. After a leader is elected, the normal operation phase starts, which is that of log replication.
During this phase the leader makes proposals for updates and replicates this information among the followers. 
By explicitly separating the leader election from the log replication thesis, raft makes it easier to reason about the behavior of the system and to keep track of which proposal should be winning proposals versus not.



9. Log management in Raft – how is info used to update, discard entries, trim/garbage collection?

Each node maintains a log of entries. A log entry contains the information about the operation that was performed and the term during which the update occurred. Each entry is indexed using an index number
Leader node accepts request for updates from clients, updates the request to its log and then pushes the logs (previous entry + new log entry) to followers
If a node failed and is behind with its updates. When it comes up again it will receive its updates via heartbeat mechanism. 
When the leader receive ack from majority of the followers that their logs are applied, then the leader will commit the new log entry and notify the client.
If a leader fails before committing some entries, the new leader elected won’t know about the uncommitted entries and those log entries will be discarded. Clients will know that some of the updates were not successful (not received ack) and will have to resend.
If a newly elected leader has some uncommitted log entries from a previous term, but he was a leader. Once it becomes the leader again, it has a chance to commit these uncommitted logs in the new term. In that sense if a client waits long enough, its request may in fact get committed and it may receive an acknowledgement.
When the log in the system become quite long and some node fallen behind - trim the log by taking snapshot and send it to the node that is falling behind during the heartbeat. Delete/discard the logs prior to the snapshot that is no longer needed.


09. Distributed Transactions
1. What is the concept of TrueTime? How is that related to the Logical Clocks discussed earlier in the semester? How can it be implemented?
TrueTime introduces an uncertainty interval on realtime, and it is implemented by periodically probing master clock servers in the datacenter (this is why true time can have maximum 2*Ɛ offset to the real time as the probing needs to travel back and forth in the network).  

2. How does the use of TrueTime change what is required from the system (Spanner) in order to establish ordering among transactions?

When we use True Time to pick a value (TT.now().latest) for a transaction (it is guaranteed to be greater than the current reading of the clock at any of the nodes in the system). This guarantee that any transactions elsewhere in the systems that completed before this true time would have completed before the current transactions and hence establishes order among the transaction

3. Using TrueTime how do you establish ordering of write operations? How do you implement a read operation? What about reading a snapshot “in the past”?

Ordering in write operation: 
We choose the timestamps at the start of the transaction such that it is greater than any transactions that occurred in the system, i.e. it is greater than the bounds of the true time interval. When the transaction is complete, we need to wait to release the lock. We have to ensure that the clock on all the other nodes has advanced sufficiently greater than the timestamp s. THis ensures that any transaction that start after the release of lock is going to have a timestamp greater than s.

Ordering in read operation:
Reads in a read-only transaction execute at a system-chosen timestamp without locking, so that incoming writes are not blocked. A read-only transaction executes in 2 phases: assign a timestamp sread, and then execute the transaction's reads as snapshot reads at sread. The snapshot reads can execute at any replicas that are sufficiently up-to-date. (http://muratbuffalo.blogspot.com/2013/07/spanner-googles-globally-distributed_4.html) 

There is another type of read operation, called read now. Here the leader will determine a safe timestamp. Determining safe timestamp may delay the read now operation as there may be prepared transactions that are not committed and leader will wait for those transactions to complete. 



4. Describe at a high-level Spanner, how does it organize and replicate data, who is involved in serving read or write operations…

Spanner consists of a stack of multiple components.
Bottom of the stack is the persistent storage like a distributed file system. IT is replicated for providing fault tolerance and greater availability
Data model layer abstraction for the application
Key value store, BigTable (later modified by layering a new system over it called Mega Store,  to better support operations which access different types of related data. It exports a view of distributed sore that matches relational database
Each of the table is replicated and replicas are kept consistent with Paxos


For write transactions that span multiple replicas, a transaction coordinator ensure two things
All the participant nodes in the transaction perform their share of the update using the 2 phase commit
Each participant has the make sure that the updates it needs to perform are consistently replicated to its replica set (by acquiring the locks and computing the timestamps)
For read transactions the leader and replicas may be involved in serving the reads

5. Describe at a high-level Aurora, how does it organize and replicate data, who is involved in serving read or write operations…

Uses primary replica architecture. Primary serves read and write operations and other nodes only serve read requests
AWS servers organized in many zones
Each zone is independently managed at different locations
Aurora first replicates data at each zone and in each zone replicates the data to two different locations 












6. Can you explain/justify the differences in the two systems? You can consider a number of possible dimensions such as different design goals, underlying assumptions, performance, … 













                   Spanner			                 Aurora
Architecture
Distributed transactions architecture
Primary replica architecture
Design Goals

A global, scalable database 
• General-purpose transactions (ACID) 
• SQL query language 
• Schematized tables 
• Semi-relational data model 

 Global: data is replicated in multiple data centers 

Scalable: increase transac>on read/write throughput by adding more machines

Guarantee high availability under large number of failures

High performance
Performance 
The coordination overhead used to ensure consistency comes at the cost of being unsuitable where very low latency operations are needed (~1 ms).
Query latency is very low due to all nodes serving read requests















7. What are some other design points that can be considered in systems which don’t have TT? 

snapshot isolation Or  multi-version concurrency control. To guarantee correctness, it is necessary to make sure that the order in which the snapshots are used. And the order in which they depend upon one another forms a serializable sequence. So if we draw directed lines among the snapshots that a transaction depends on or produces, there shouldn't be a cycle in that graph.



Other:
1. What are the names of some of the authors of the papers we talked about? Do you know where they’re now, what they’re famous for, what else they’ve done?
Leslie Lamport currently works at Microsoft. At least that is what his website says.(Yes. Just confirmed on Teams)
Michael. J. Fischer is at Yale
Nancy Lynch is at MIT
Michael Stewart Paterson is a British game designer and mathematician who has been the director of the Centre for Discrete Mathematics and its Applications in the Department of Computer Science at the University of Warwick

2. Think of some distributed service you use – daily (cloud mail, search, social networks, some service at your work, …). Make an assumption on how they implement some aspect of the distributed system (from Time to Distributed Transactions) and think through the pros/cons of that design decision based on what you assume the system and workload look like.
