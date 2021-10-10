---
title: "Transaction Isolation"
geekdocDescription: ""
keywords: ["distributed computing", "omscs", "transaction", "ACID", "i"]

draft: true
---

# Transaction Isolation

__Links__

* Source: <a href="https://cs.uwaterloo.ca/~ddbook" target="_blank">FundamentalsPrinciples of Distributed Database Systems, Fourth Edition</a> > <a href="https://cs.uwaterloo.ca/~ddbook/downloads/appendix/Appendix-C" target="_blank">Appendix C: Transaction Processing </a>
* <a href="https://stackoverflow.com/questions/11043712/what-is-the-difference-between-non-repeatable-read-and-phantom-read" target="_blank">What is the difference between Non-Repeatable Read and Phantom Read? - StackOverflow</a>
* Ch 7: Transactions, Designing Data-Intensive Applications (DDIA) by Martic Kleppmann

## Transaction Basics: Excerpts from DDIA

> With the hype around this new crop of distributed databases(NoSQL databases), there emerged a popular belief that transactions were the antithesis of scalability, and that any large-scale system would have to abandon transactions in order to maintain good performance and high availability [5, 6]. On the other hand, transactional guarantees are some‐ times presented by database vendors as an essential requirement for “serious applica‐ tions” with “valuable data.” Both viewpoints are pure hyperbole.

> One database’s implementation of ACID does not equal another’s implementation. For example, ..., there is a lot of ambiguity around the meaning of isolation 

> Systems that do not meet the ACID criteria are sometimes called BASE, which stands for Basically Available, Soft state, and Eventual consistency. This is even more vague than the definition of ACID. It seems that the only sensible definition of BASE is “not ACID”; i.e., it can mean almost anything you want.



## Definition

Isolation is the property of transactions that requires each transaction to see a consistent database at all times. In other words, an executing transaction cannot reveal its results to other concurrent transactions before its commitment.

> *Isolation* in the sense of ACID means that concurrently executing transactions are isolated from each other: they cannot step on each other’s toes. The classic database textbooks formalize isolation as *serializability*, which means that each transaction can pretend that it is the only transaction running on the entire database. The database ensures that when the transactions have committed, the result is the same as if they had run *serially* (one after another), even though in reality they may have run con‐ currently.

> Atomicity and isolation also apply when a single object is being changed. For example, imagine you are writing a 20 KB JSON document to a database.

## Reasons for Transaction Isolation

1. Database Consistency (Constraints should hold in the face of concurrent writes)
2. Performance: Prevent _cascading aborts_. If a transaction permits others to see its incomplete results before committing and then decides to abort, any transaction that has read its incomplete values will have to abort as well. 

## SQL Phenomena

SQL defines a number of isolation levels based on _phenomena_ which are situations that can occur if proper isolation is not maintained


* __Dirty Reads:__ read UNCOMMITED data from another transaction

- __Non-repeatable Reads / Fuzzy Read / Read Skew:__ T1 executes a `Select` query twice but gets different values because T2 `committed` between two executions and `Updated` the queried row.
  
- __Phantom Reads:__ T1 executes a predicate query twice but they return different results. This happened because T2 `committed` between two executions and `Inserted/Deleted` rows satisfying the predicate.

__Note:__ The Non-Repeatable Read applies to a single row, the Phantom Read is about a range of records which satisfy a given query filtering criteria. Non-repeatable-reads read COMMITTED data from an UPDATE query from another transaction. Phantom-reads read COMMITTED data from an INSERT or DELETE query from another transaction

#### More Phenomena

* **Dirty Writes**
* **Lost Update**: 
  * Two transactions execute _read-modify-write_ cycles. One update gets lost.
  * Solution: 
    * Atomic update queries: `UPDATE inventory SET qty = qty-1 where item_id = 1  ` 
    * Locking : `txn_begin; Lock(x); read-modify-write(x); unlock(x); ...; txn_end`

* **Write Skew**
  * Generalization of list update
  * T1: read(x)-modify-write(y) ; T1: read(x)-modify-write(z)
  * eg. if (oncall_doctors() > 1), remove_from_oncall(me); 

## Isolation Levels

### Read uncommitted

*  For transactions operating at this level all three phenomena are possible.

### Read committed

* No dirty reads, No dirty writes

* But Fuzzy reads and phantoms are possible

  **Implementation**

  * Avoid Dirty Writes: using row-level locks
  * Avoid Dirty Reads: Lock-Read-Unlock  OR Maintain two versions of data (committed and uncommitted)

### Repeatable read:

* Only phantoms are possible.

  __Snapshot Isolation__
*  Non-serializable isolation level
* Commonly implemented in commercial products to provides repeatable reads

  **Implementation**

  * Avoid Dirty Writes: using row-level locks
  * Avoid Dirty Reads: Multi-Version Concurrency-Control (MVCC)


### Anomaly serializable: 

* None of the phenomena are possible.

SQL standard uses the term `serializable` rather than `anomaly serializable`. However, a serializable isolation level cannot be defined solely in terms of the three phenomena identified above; thus this isolation level is called “anomaly serializable” [Berenson et al., 1995]. The relationship between SQL isolation levels and the four levels of consistency defined in the previous section are also discussed in [Berenson et al., 1995].



