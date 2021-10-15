---
title: "W07 - Spanner"
geekdocDescription : "Google’s Globally-Distributed Database"
---

#### Links

* [Spanner and Open Source Implementations](https://www.binwang.me/2018-07-29-A-Review-on-Spanner-and-Open-Source-Implementations.html)
* http://muratbuffalo.blogspot.com/2013/07/spanner-googles-globally-distributed_4.html
* [OSDI12 - Spanner: Google’s Globally-Distributed Database - Youtube](https://www.youtube.com/watch?v=C75kpQszAjs)

## Spanner FAQ

Source: <a href="https://pdos.csail.mit.edu/6.824/schedule.html" target="_blank">MIT 6.824 - Distributed Systems</a> > <a href="https://pdos.csail.mit.edu/6.824/papers/spanner-faq.txt" target="_blank">Spanner FAQs</a>

**Q: How does external consistency relate to linearizability and serializability?**

A: External consistency seems to be equivalent to linearizability, but applied to entire transactions rather than individual reads and writes. External consistency also seems equivalent to strict serializability, which is serializability with the added constraint that the equivalent serial order must obey real time order. The critical property is that if transaction T1 completes, and then (afterwards in real time) transaction T2 starts, T2 must see T1's writes.



