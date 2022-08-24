 # noSQL

Parents: [[database]], [[01_Tools]], [[system_design]]
Related: [[data_wh]]

#db #bib #systems

Subtopics: [[mongo]]


A general word for non-relational databases: not tabular-based, good for realtime big data. May work with documents, key-values, wide-columns, graphs. Have lower guarantees on consistency (stale reads: updates don't update immediately), which can lead to data loss. Yet an efficient (in terms of resources and cost) way of storing large amounts of data that are used either rarely, or at fixed granularity (fixed patterns of use, see below).

A reflection of the fact that storage is currently cheaper than computing power, so SQL (which optimizes storage, but is computationally expensive) is partially replaced by noSQL (bulky, but simpler). SQL joins are expensive.

**Data pressure**: when we cannot process existing data with existing processing capacity (because data is cheaper than processing). In 2020, we are at this point (cheap storage, expensive processing), so SQL optimizes a wrong part of the cost (ref: a talk by Rick Houlihan, below)

Instead of ad hoc queries (as in SQL), the best way of working with noSQL is to use **Instantiated views**. This allows OLTP (transactions, lots of reads) at scale. If access patterns are very stable, noSQL may be better than SQL. Predictable outputs can be cached really efficiently, which means that as the volume of requests increases, the response latency typically goes down, not up! But at the same time, nosql is inflexible, and thus really bad with random analytics.

The **development** of noSQL solutions has a different thought logic, compared to SQL. With SQL you start with the data (what the data really is, in its core), while reports may be left as an afterthought. With noSQL you have to really start with usage cases (aka **access patterns**, both read and write), and then build the representation backwards from there. Generally, **1 service = 1 table**; you want your requests be serviced by one table only, so that you could benefit from partitioning.

Each record in the table has only one required attribute: they key (aka **partition key**). Primary ids are called parition keys as they are actually used for hashing different records across different physical devices, which simplifies horizontal scaling. Partitions are often replicated several times; for example DynamoDB keeps 3 copies and writing is acknowledged when 2 out of 3 performed the write, while for reading they recommend to use **eventual consistency** (read from whatever node is faster) rather than **strong consistency** (always read from a primary node for this hash; see [[db_consistency]]).

There is also a **sort key** that allows sorting, ranges, and set operations. Typical pattern: return all entries for a given customer id within a certain date range. Sort key filtering is performed before the read, so we are not paying for reading info we don't need. As we only use one sort key however, it's better to fine-tune the sort key to the task by creating a **composite key**: say date+status, concatenated, or status+date. The sequence of concatenatin should reflect the hierarcy of "zooming in" that we want with the data, as we won't be able to flip it later. With lexicographical order, we always apply filters in the same sequence.

**Secondary indices** may be either **local**, in which case they don't change where the data is stored (don't affect partitioning), but may be used for sorting. Or they may be **global**, in which case they are used for partitioning… _Not sure how it works?_ Local are fast, and strongly consistent (happen within one processor), but the functionality is limited. Global are more flexible, but are only eventually consistent, as they need to be replicated / backed up.

What causes most performance problems in noSQL is when hashing of keys is not good, and so load is not distributed evenly, which throttles some requests, while simulatanelusly underutilizing idling hardware. To improve hashing, use more different unique keys, don't let super-keys emerge. (???) Also, because there are no joins, nothing prevents us from keeping all data in one table, even if data is heterogenious. For example, if you have clients and vendors, in principle, you don't need 2 tables for them; you can just use differently formatted keys for vendors and for clients, as you'll never be querying both of them in the same query. This relying on one table for different queries also helps to balance (and thus scale) the load.

This architecture is also conducive to dynamic scaling, so cloud services like AWS can use it for their services (aka **elasticity**).

noSQL is not really not good with things like sum(), count(), and average(), so typically you would run a separate service (ETL job?) to aggregate these values at some reasonable level and writes it back into a table as metadata (??). Then you can read it as much as you want. Precompute everything! Similarly, if you absolutely need to have joins, perform them in advance, as you write data in the db, not as you read it. And don't be afraid of some duplication; the mantra is that space is cheap, while compute is expensive.


# Refs

Blog post (2020) by Artem Bugara
https://codarium.substack.com/p/the-best-sql-vs-nosql-mindset-ive

A talk by Rick Houlihan, on Amazon DynamoDB
https://www.youtube.com/watch?time_continue=21&v=HaEPXoXVf2k