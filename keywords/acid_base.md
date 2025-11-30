# ACID vs BASE in Database design

Parents: [[database]], [[db_consistency]]
See also: [[crud]]

#db


These are two funny-named alternatives to how database transactions can be approached:
* **ACID**: Atomic, Consistent, Isolated, Durable (actual acronym)
* **BASE** : Basically Available, Soft-state, Eventually-Consistent (silly backronym)

When people talk about "transactions" in a DB context, typically they assume that it's ACID-style consistent.

More details:
* **ACID** - strong [[db_consistency]]
    * **Atomic** - each transaction either fully fails, or fully succeeds, even it consists of several changes. A transaction can't be half-successful. If something happens, and we are not sure that the transaction succeeded, all changes that seemed successful are nevertheless rolled back, and the transaction is reported as failed. It also means that for a 3d observer the transaction is never "in progress", it's either not there, or is already over.
    * **Consistent** - the reason we wanted this "atomic principle" above: we want the database to follow a certain logic (to be consistent according to this logic) both before and after the transaction. Allowing "progress" would mean that while "in progress" the database would be in an inconsistent, disheveled state, and we can't afford that.
    * **Isolated** - that transactions don't see each other, and even when they are run concurrently, the result should be as if they were run nicely and sequentially. Which in practice is achieved by clever identification of conflicts and required locks. (Transactions that don't conflict should be allowed to be fully concurrent, but if there's a conflict - they can't be). Keyword: **concurrency control**
    * **Durable** - If a system as a whole experiences a dramatic failure (eg. something like of a power-off), a transaction that was reported as "done" should be there after system recovery

The opposite of is **BASE** - eventual [[db_consistency]]

It's kinda silly to backronym BASE, so let's just concentrate on what it means. It means that ACID is violated in favor of increasing speed and availability. A bit of a do now, think later mentality: in this approach (**eventual consistency**) it's OK to log partial transactions, and have the system in contradictory states _at times_, as long as we have a process to "sort the things out" later that typically (?) leads to a resolution. First get the system out of consistency, change what needs to be changed, then deal with it later in a separate process.

Cases when BASE is ok:
* Social media - we want it fast, but we don't mind losing consistency for a few minutes, as long as everything remains responsive
* E-platforms, when fast read is critical, but fast updates after writes (after products are changed) is not
* Ingestion of data from devices - when fast writing is critical, but reading it immediately is not
* Realtime visualization - we want the visuals to be fast and kinda true, but they don't have to be identical across nodes. You need it fast, reliable, scalable, and actionable. But you can afford to be a bit imprecise, or to have a slight delay for aggregation.