# CRUD (and REST)

#api #systems

See also: [[database]], [[sql]]

**CRUD** stands for CREATE - READ - UPDATE - DELETE. It is more of a general (and really old: 1983) framework for creating an interface between two entities. Apparently stems from ealy DB systems, but the main reincarnation of CRUD is REST, and it's an (also old) web API.

SQL is also originally based / inspired by CRUD.

**REST** (Representational State Transfer) is an HTTP-based API that usesGET, PUT and POST methods (also DELETE, but it is rarely used). The main feature here is **statelesness** which means that the server doesn't remember anything about the client - it is on the client to convince the server that some sort of data exchange is in progress. REST also supports a **Cache**: some responses may be marked as cachable, while some are not.

# Refs

https://www.bmc.com/blogs/rest-vs-crud-whats-the-difference/

https://en.wikipedia.org/wiki/Create,_read,_update_and_delete