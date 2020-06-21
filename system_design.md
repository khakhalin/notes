# System Design

#tools #systems #bib #interview

See also:
* [[job_search]]
* [[database]]
* [[nosql]]

# Web services

**Scaling**: **Vertical** - better CPU, better RAM etc. **Horizontal** - parallelize, potentially with cheaper hardware.

**Load balancer**: a process of distributing tasks over resources. May involve scheduling tasks of different size; scheduling of tasks that depend on each other (there are heuristic methods to optimize computation of a directed graph that arises from dependencies); and effective segregation (splitting into "independent" tasks that can be parallelized).

Possible balancing approaches for a client-server architecture:
* **Round-robin** (common for DNS/IP services): resolve into a list of IP sorted by priority, and the list is adjusted (permuted) for every response. The client tries IP in the order, and goes to a diff IP after a time-out. An IP that ended up working is used again (cashing), but with a certain expiration time. ([ref](https://en.wikipedia.org/wiki/Round-robin_DNS)) But it means that within-user load is not balanced, which is a potential issue.
* Client-side random load balancing (randomize where the requests are sent)
* Server-side balancing (necessary for horizontal scaling). May combine round-robing (randomizing) with more sophisticated load balancing.

In a **Data Center** lots of servers have private IP addresses, but share one public IP address, with messages managed by a load balancer. TCP/IP within the data center.

# OS and hardware

Quad-core processors have parallelism (4 cores, so can do 4 things at a time).

Working with databases requires faster drives, as typically involves lots of writing. So one of the bottlenecks for a server.