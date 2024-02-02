# SAP

Parents: [[finance]]
See also: [[finance]], [[product_supply]]

#tools


The most famous ERP system, obviously, duh.

# Practical aspects for DS / Systems design

SAP modules are quite slow when talking to each other through integration-level APIs. If you need to achieve any level of performance, you need to work with the underlying databases directly. But at the same time, the specifics of working with SAP is that you often get updates for records far in the past, as by design it uses a more accounting-like representation. (Perhaps it's fair to say that it's equivalent to a REP-layer representation in a [[data_wh]], but without good access?) Which means that if you read from underlying databases, you end up emulating transactions (events) by detecting deltas, instead of getting access to real, original, underlying transactions. It makes re-populating data warehouses from SAP painful, if you have to do it near-realtime, and at scale.

# Notable 3d-party plugins

Simplement - an US company that provides a service for continuous real-time data replication from SAP to data WH. Like, the situation is so bad, there's a business niche just for inefficiently translating SAP infrastructure into the modern framework!

# Refs