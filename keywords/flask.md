# Flask

Path: [[tools]] / [[python]]
See also: [[microservice]], [[restful]]

#python


A thing to create simple html APIs.

As of 2025, is mostly replaced by `fastAPI`, which works better.

Works well in conjunction with [[asyncio]]

When responses become too fast and the chances of a greedlock increase, consider using message queues ([[streaming]]) to smoothen things out (acting as rate limiter with memory?).