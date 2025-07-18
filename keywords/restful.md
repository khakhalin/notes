# REST and Restful

Parent: [[system_design]], [[api]]
See also: [[crud]]


**REST = Representational State Transfer** is a predefined set of stateless operations (stateless here means that all session info is tracked by the client, not the server). HTTP is the archetypical RESTful system, and most restful APIs are HTTP-based; they use GET, PUT and POST methods (and also DELETE, which is rarely used).

The main feature of restful APIs is their **statelesness** which means that the server doesn't remember anything about the client: it is the client's responsibility to convince the server that some sort of data exchange is actually in progress. The REST framework also supports [[caching]]: some responses may be marked as cachable, while some wouldn't be.

As it's built on top of HTTP, it can enjoy a standard set of response codes:
* `10X` - informational
* `200` - success
* `20X` - other optimistic codes, like "created" or "accepted"
* `30X` - different status messages related to redirection
* `40X`- service denial messages, including bad request, forbidden, not found, unauthorized etc. The most rich space!
* `50X` - server-level errors