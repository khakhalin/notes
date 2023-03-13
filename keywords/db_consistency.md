# Consistency

#db

Parents: [[database]], [[dictionary]]

**Strong consistency** - when data is requsted, the system first checks whether any write on it were initiated (or, alternatively, whether different versions of this data all report the same data). If there's no discrepancy, the data is returned to the user. If the data is locked for write, or if data is not synchronized across instances, the system waits until this issue is resolved, and only then returns the data. _Or at least that's how I understood it; some technical points may be missing here_

**Eventual consistency** - good for distributed systems, as it's fast. Aka **optimistic replication** - the system eventually **converges** to a consistent state, but it may take a few ms. BASE (not ACID). There's a constant exchanging of info between servers, to detect differences. In case of a difference, **reconciliation**: usually **last writer wins**, but for some solutions where guarantees are more important, it may be **first writer wins** (both - based on the timestamp). For the user, it guarantees short latency, but you can get stale data.

Also some other terms:

**Strict consistency**: writes are scheduled at a certain time slot, so that they would happen synchronously on all processors. _I assume, in case of a failure, there's a roll-back?_

**Sequential consistency**: the sequence of actions should be the same for all processors, even if the exact times differe a bit.

# Refs

https://en.wikipedia.org/wiki/Consistency_model

https://en.wikipedia.org/wiki/Consistency_(database_systems)

https://hackernoon.com/eventual-vs-strong-consistency-in-distributed-databases-282fdad37cf7