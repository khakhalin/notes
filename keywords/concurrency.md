# Concurrency

Parents: [[parallelism]]
See also: [[gridlock]], [[acid_base]]

#realtime #concurrency


# Optimistic concurrency

Aka “compare-and-swap”. When in most cases processes are independent from each other and calculations don't conflict, but sometimes they do. Then first perform a calculation, then try to "commit" the result. Check for conflicts. If no conflicts - happily commit. If a conflict, either try to resolve, or maybe even just naively recalculate.

In distributed systems, it is called eventual consistency (as in BASE of the [[acid_base]] fame). Possible ways to resolve and optimize:
* CRDT 🔥
* vector clocks 🔥

# Locking, aka Pessimistic Concurrency Control

A most logical alternative to optimistic concurrency: a strict system where you forbid all changes (ideally, locally rather than globally) in an area where you predict changes, before calculating and committing these changes. Can lead to [[gridlock]]s.

# Fancier alternatives

**Seriazability**: in some cases changes may be designed to be additive, in a way: so instead of the end-state A we're calculating the `delta_A`, such that it could be applied on any underlying state, and produce a correct state. Essentially, calculate _operations_ rather than end-states (operations that can be serialized, sequenced, and eventually applied).

**Timestamp Ordering**, aka **Multiversion Concurrency Control**. Similar to optimistic concurrency, but sorta server-side rather than client-side. Every process gets a snapshot of the system, which is never locked, which means that different processes may get different snapshots. Once they did something to it, they commit them. These transactions are registered as a multiverse of versions: parallel realities, as they stem from different snapshots. Because of that, it's expensive in space, as one has to support increasingly branching versions of the same db. It's a good approach when you need read a lot, interact with the data, but commit it rarely. 🔥 _For now I'm not sure what happens next: Wiki says something about just abandoning older versions, which sounds weird? I'm assuming that in most cases you do the same conflict resolution as everywhere else, just within a multiverse?_