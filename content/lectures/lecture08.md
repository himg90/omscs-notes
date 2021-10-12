---
title: "08 - Consensus Protocols"
geekdocDescription: ""
keywords: ["cs7210", "distributed computing", "omscs"]

date: 2021-10-11T20:30:29+05:30
draft: true
---

# Consensus Protocols

## Paxos

Source: <a href="https://www.quora.com/In-distributed-systems-what-is-a-simple-explanation-of-the-Paxos-algorithm" target="_blank">Russell Cohen's Answer on Quora</a>

Paxos guarantees that if one peer believes some value has been agreed upon by a majority, the majority will never agree on a different value.
Any node that proposes after a decision has been reached must communicate with a node in the majority. The protocol guarantees that it will learn the previously agreed upon value from that majority.

Source: <a href="https://www.the-paper-trail.org/post/2009-02-03-consensus-protocols-paxos/" target="_blank">Paper Trail - Consensus Protocols: Paxos </a>
Paxos adds two important mechanisms to 2PC. The first is ordering the proposals so that it may be determined which of two proposals should be accepted. The second improvement is to consider a proposal accepted when a majority of acceptors have indicated that they have decided upon it. This is different from 2PC where proposals were accepted only if every acceptor agreed to do so. This lead to the blocking characteristics of 2PC, where a single failed node could lead to the protocol never terminating while the proposer waited for a reply that would never come. Instead, in Paxos, nearly half the nodes can fail to reply and the protocol will still continue correctly.
