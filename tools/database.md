# Notes on databases
#tools #todo

# Unprocessed:

* https://en.wikipedia.org/wiki/Database_transaction
* https://en.wikipedia.org/wiki/Consistency_(database_systems)
* https://en.wikipedia.org/wiki/Document-oriented_database
* https://en.wikipedia.org/wiki/Graph_database
* ACID transactions
* https://en.wikipedia.org/wiki/Polyglot_persistence
* https://en.wikipedia.org/wiki/Object-relational_impedance_mismatch
* https://en.wikipedia.org/wiki/Eventual_consistency

Should not some of it go in the [[dictionary]]?

# Principles and guarantees

**ACID**: a set of standard requirements for relational databases that maximizes their reliability: 
* **Atomic** - each transaction either fails as a whole, or succeeds as a whole; you cannot half-update a table. 
* **Consistent** - invariants (schema) are checked; database always remains valid.
* **Isolated** - if transactions are executed concurrently, the result is the same as if they were ran sequentially,
* **Durable** - completed transactions are recorded in non-volatile memory, so once a transaction is marked as "completed", it will survive a power outage.