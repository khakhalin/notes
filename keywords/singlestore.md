# SingleStore

Parent: [[database]]
See also:

#db #tools


Distributed SQL database (although can be used for non-relational data as well), optimized for data-intensive high-throughput operations, such as data ingestion and transaction processing (OLTP). Compiles SQL into machine code.

Oddly, uses MySQL dialect of SQL, and is compatible with MySQL APIs (?), to let people seamlessly "upscale" from MySQL.

Proprietary. Originally branded MemSQL, as the whole idea was that the database sits entirely in memory, but now uses distributed (sharded) [[parquet]] files as a backend. Sharding and optimization need to be performed manually (?). Doesn't use primary keys (?)

# Refs

https://en.wikipedia.org/wiki/SingleStore

https://www.singlestore.com/faq/