# Databases

Parents: [[system_design]]
See also: [[streaming]], [[datalake]]

#tools #bib #db


Subtopics
* [[data_wh]] - Data Warehouse
* [[nosql]] - a conceptual alternative to tables
* [[sql]] - about the language in general.    

 Solutions and SQL dialects:
* [[mongo]] - everyone's favorite [[nosql]] nosql
* [[postgres]]
* [[databricks]] 
* [[snowflake]]
* [[singlestore]]

Transformations:
* Schedulers: [[airflow]] (see also: [[cron]])
* Transformations: [[dbt]]

# Comparison of solutions

| Name                | Licensing            | Query          | Good                                                                                                  | Bad                           |
|---------------------|----------------------|----------------|-------------------------------------------------------------------------------------------------------|-------------------------------|
| Redis               | o.s.                 | keys to values | very fast, fully in-memory caching                                                                    | large data                    |
| MongoDB             | o.s.                 | range-based    | document retrieval                                                                                    | can't have >2 keys            |
| Postgres            | o.s.                 | sql            | simple relational tables, good community                                                              | can't do any nosql            |
| MySQL               | o.s.                 | sql            | simple, lightweight, good community                                                                   | scale, speed                  |
| Singlestore         | license, cloud only  | sql            | transactions                                                                                          | analytics                     |
| Microsoft SQLServer | license              | sql            | enterprise back-end. Mostly transactions-oriented                                                     | analytics                     |
| Snowflake           | by usage, cloud only | sql            | analytics, data warehousing. automatic scaling, index optimization, separation of storage and compute | fast transactional operations |

There's also Oracle, but it seems that while it is popular, it's mostly because people are locked-in with it, as it's both more expensive, and less effective than the alternatives.
 
# Principles / guarantees

**ACID**: a set of standard requirements for relational databases that maximizes their reliability: 
* **Atomic** - each transaction either fails as a whole, or succeeds as a whole; you cannot half-update a table. 
* **Consistent** - invariants (schema) are checked; database always remains valid.
* **Isolated** - if transactions are executed concurrently, the result is the same as if they were ran sequentially,
* **Durable** - completed transactions are recorded in non-volatile memory, so once a transaction is marked as "completed", it will survive a power outage.