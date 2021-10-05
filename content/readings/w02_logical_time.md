---
title: "W02 - Logical Time"
geekdocDescription : "A Way to Capture Causality in Distributed Systems"
---

## A Way to Capture Causality in Distributed Systems

[Link to Paper](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.35.6474&rep=rep1&type=pdf)

This paper is an easy read and not very long. Moreover, first few pages just repeat the same thing over and over. So, you can read through it in a couple of hours.

Big Ideas

- Global/physical clocks don’t work for distributed processes. So, we use a system of logical clocks where each process has its own clock.
- There is no way to determine a total order on the events, we work with partial ordering.

Abstract

- Logical time is better suited for tracking causality in distributed systems than physical time
- Paper reviews three ways to define logical time (scalar, vector, matrix) in distributed systems

1 Introduction

- Model: A group of distributed processes communicating by passing messages instead of sharing memory. Messages have finite but unpredictable delays
- Three types of events can occur at a process: internal events, msg send, and msg recv.
- “msg send” and “msg recv” events establish causality between the two processes ⇒ partial ordering
- Knowing causality helps to solve a number of problems which are listed in paper. I found the following one interesting
    - Concurrency measure: “...All events that are not causally related can be executed in parallel. Thus, an analysis of the causality in a computation gives an idea of the concurrency in the program”
- Physical( or wall) clocks don’t work for distributed systems because they are loosely synchronized. So, we turn to logical clocks.
- What is a logical clock?
- Paper presents
    - a general framework of a system of logical clocks
    - three popular systems of logical clocks - scalar, vector and matrix
    - efficient implementations of logical clocks

## A Model of Distributed Execution

### 2.1 General Context

A distributed program consists of n processes which communicate by passing messages. Communication delay is finite but unpredictable. There is no global clock. Communication is asynchronous i.e. processes do not wait for msg delivery.

### 2.2 Distributed Execution

- All events occurring on a single process have a total order.
- P1 sends msg M (event e1s) and P2 receives this msg (event e2r), then ⇒ All events the occurred on P1 upto e1s “happened before” any event the occurred on P2 beyond e2r
- Two events can either be related by “happened before” relationship else they are concurrent ie we cant decide their order
- Notation
    - A → B A happened before B
    - A || B A and B are concurrent

### 2.3 Distributed Executions at an Observation Level

Consider a set of events, E and a bunch of “happened before” relationships among them. An observation level is a subset E’ of E and a subset of all “happened before” relationships which relate events within E’.