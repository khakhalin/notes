# System Design
#tools

# Resources

* https://github.com/donnemartin/system-design-primer
* https://github.com/checkcheckzz/system-design-interview
* https://www.byte-by-byte.com/3-ways-to-ace-your-system-design-interview/

Every solution depends on parameters of scale: amount of data, n users, requests per seconds, expected respont time, read/write ratio.

# Terminology and tools

**Load balancer**: a process of distributing tasks over resources. May involve scheduling tasks of different size; scheduling of tasks that depend on each other (there are heuristic methods to optimize computation of a directed graph that arises from dependencies); and effective segregation (splitting into "independent" tasks that can be parallelized).

Some possible approaches for a client-server architecture:
* Server-side balancing
* Client-side random load balancicng
* Round-robin (for IP service): resolve into a list of IP sorted by priority, and the list is adjusted (permuted) for every response. The client tries IP in the order, and goes to a diff IP after a time-out. An IP that ended up working is used again, but with a certain expiration time. ([ref](https://en.wikipedia.org/wiki/Round-robin_DNS))

Words to look up:
* HTTP methods GET, POST, PUT
* How does Internet work? DNS?
* Redis cluster / database

# Solving problems

* Identify key tasks, functionalities, features
* Identify an abstract database structure: what tables, and how they are linked
* What to optimize for? What needs to be fast? What can be cashed? What asynchronies are permitted?
* What can be pre-computed before? Trade-offs. Say, can we trade memory for speed?
* Can we optimize, by sending some flows through cashed, and some through vanilla pipelines, depending on their activity?

Example: for Twitter, it's easier to keep pre-calculated timelines and update all of them with every update, instead of writing a new tweet only once, and SQL selecting it after. It however creates a computational-graph-like problem, because you may see response-tweets before original tweets, if original tweets weren't yet precomputed.