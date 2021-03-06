# Notes on databases

#tools #bib #db

Parents: [[01_Tools]], [[system_design]]

See also:
* [[data_wh]] - Data Warehouse
* [[nosql]] - an alternative to tables
* [[dictionary]] - many names are still sitting there

# Principles and guarantees

**ACID**: a set of standard requirements for relational databases that maximizes their reliability: 
* **Atomic** - each transaction either fails as a whole, or succeeds as a whole; you cannot half-update a table. 
* **Consistent** - invariants (schema) are checked; database always remains valid.
* **Isolated** - if transactions are executed concurrently, the result is the same as if they were ran sequentially,
* **Durable** - completed transactions are recorded in non-volatile memory, so once a transaction is marked as "completed", it will survive a power outage.