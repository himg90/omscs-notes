---
title: "High Availability vs. Fault Tolerance"
geekdocDescription: ""
keywords: ["cs7210", "distributed computing", "omscs"]

date: 2021-10-10T16:32:34+05:30
draft: false
---

# High Availability vs. Fault Tolerance

[source](https://www.linkedin.com/pulse/high-availability-vs-fault-tolerance-jon-bonso/)

> High Availability aims for your application to run 99.999% of the time. Its design ensures that the entire system can quickly recover if one of its components crashed. It has an ample number of redundant resources to allow failover to another resource if the other one fails. This concept accepts that a failure will occur but provides a way for the system to recover fast.
>
> Fault Tolerance, on the other hand, has the goal of keeping your application running with zero downtime. It has a more complex design, and higher redundancy to sustain any fault in one of its components. Think of it as an upgraded version of High Availability. As its name implies, it can tolerate any component fault to avoid any performance impact, data loss, or system crashes by having redundant resources beyond what is typically needed. The caveat for implementing a fault-tolerant system is its cost as companies have to shoulder the capital and operating expenses for running its required numerous resources. 
>
> A system can be highly available but not fault-tolerant, and it can be both. If an application is said to be fault-tolerant then it is also considered highly available. However, there are situations in which a highly available application is not considered fault-tolerant.
