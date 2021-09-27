---
title: "07 Fault Tolerance"
keywords: ["fault tolerance", "distributed system", "omscs"]
---

# Fault Tolerance

## Some Taxonomy

* Fault-Error-Failure
  * Fault is the problem (software bug, hardware failure)
  * When fault is activated (because buggy part of code is executed), it causes errors
  * Failure is the resulting behaviour
* Faults: Transient / Intermittent / Permanent
* Failures: Fail-stop / Timing / Omission / Byzantine
  * Timing: System becomes "slow"
  * Omission: some actions are missing like msg drops due to memory constraints
* Managing Failures: Avoidance / Detection / Recovery / Removal
  * Detected failues are either _removed_ by rollback or _recovered_ from. 

## Rollback-Recovery

* Rollback the system to a state before the failure
* Rolledback state may not be a real past state. It just needs to be consistent.

## Basic Mechanisms

* Checkpointing: Save the (full or incremental) state periodically
* Logging: Log individual write operations.
  * Undo Log: Log includes original values of variables. This makes it possible to undo an operation. System state can move backward.
  * Redo Log: Log includes new values. To obtain a state, start from beginning and keep applying operations. System state can move forward. 

## Checkpointing Approaches

**System Model**

* Network is non-partitionable. **Why ??**
* FIFO / communication channel reliability and num of failures will vary with protocol.

**Uncoordinated Approach**

* Processes take checkpoints independently.
* Problems
  * The failed node rollbacks to previous checkpoint. If the state of the whole system becomes inconsistent, more rollbacks may have to be performed at several nodes => Domino effect.
  * Leads to multiple useless checkpoints.

**Coordinated Checkpoint**

* Processes coordinate to take a consistent snapshot.
* Problems
  * How to coordinate?
  * No synchronous clock guarantee ie no global clock. (Everybody take snapshot at 5pm)
  * If msg delivery was reliable with bounded delay, some approach could be created.

**Communication induced Checkpoint**

* Use a consensus protocol: All nodes should reach a consensus that a snapshot will be taken and that they will not send any msg till it's complete. In a way, it's a blocking protocol.
* A non-blocking algo will be similar to Global Snapshot algo but it requires FIFO communication channel.

**Logging**

* When reconstructing state we must ensure that it's not a inconsistent state. A crashed process might not log the last event it sent. We might apply events in wrong order.
* Pessimistic: First log the event then send it.
* Optimistic: Assume log will be persisted but make it possible to remove it's effect if aborted
* Causalty-tracking: ensure causality related events are deterministically recorded

**Which Method to Use?**

* Workload characteristics: Frequency of updatss, size of updates, 
* Failures characteristics:
* System Characteristics: cost of communication/stoage, system scale, 





