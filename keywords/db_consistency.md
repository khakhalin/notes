# Consistency

Parents: [[system_design]], [[database]]
See also: [[dictionary]]

#db


**Strong consistency** - when data is requsted, the system first checks whether any writes on it were initiated (or, alternatively, when replicas are used, whether different versions of this data all report the same data). If there's no discrepancy, and here are no locks from writes, the data is returned to the user. If the data is locked for write, or if data is not synchronized across instances, the system first waits until this issue is resolved, and only then returns the data. It increases the latency of course, but guarantees that the data is fully up to date. Follows ACID architecture [[acid_base]].

**Eventual consistency** - good for distributed systems, as it's fast. Is also called **optimistic replication**: the system eventually **converges** to a consistent state, but it may take more time. BASE (not ACID). For the user, it guarantees short latency, but the received data may be somewhat stale. Eventual consistency assumes that there's a constant exchanging of data between servers, to detect differences. In case of a difference, a **reconciliation** process kicks in, and there are several optinos here:
* **last writer wins** - the most common one (based on the timestamp)
* **first writer wins** – if guarantees are more important. 

Also some other terms:

**Strict consistency**: writes are scheduled at a certain time slot, so that they would happen synchronously on all processors. _I assume, in case of a failure, there's a roll-back?_

**Sequential consistency**: the sequence of actions should be the same for all processors, even if the exact times differe a bit.

# Refs

https://en.wikipedia.org/wiki/Consistency_model

https://en.wikipedia.org/wiki/Consistency_(database_systems)

https://hackernoon.com/eventual-vs-strong-consistency-in-distributed-databases-282fdad37cf7