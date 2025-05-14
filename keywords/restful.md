# REST and Restful

Parent: [[system_design]]
See also: [[crud]]


**REST = Representational State Transfer** is a predefined set of stateless operations (stateless here means that all session info is tracked by the client, not the server). HTTP is the archetypical RESTful system, and most restful APIs are HTTP-based; they use GET, PUT and POST methods (and also DELETE, which is rarely used).

The main feature of restful APIs is their **statelesness** which means that the server doesn't remember anything about the client: it is the client's responsibility to convince the server that some sort of data exchange is actually in progress. The REST framework also supports [[caching]]: some responses may be marked as cachable, while some wouldn't be.