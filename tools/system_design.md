# System Design

#tools #system

# Resources

* Go through 2-3 youtube lectures in the queue that describe some of the existing solutions
* https://www.byte-by-byte.com/3-ways-to-ace-your-system-design-interview/
* Go through the list of terms and write down a definition for each of them
* https://github.com/donnemartin/system-design-primer
    * https://github.com/donnemartin/system-design-primer#index-of-system-design-topics
* https://github.com/checkcheckzz/system-design-interview

Every solution depends on parameters of scale: amount of data, n users, requests per seconds, expected respont time, read/write ratio.

# Topics to look up

Concepts
* [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem) (Consistency, Availability, Partition tolerance) - impossible to provide all three, always a trade-off.	 P is a given, so relational DBs favor C, while noSQL favors A
* Data sharding (how to split data that doens't fit).
* Consistent hashing - way to solve data sharding
* Optimistic and pessimistic locking
* Strong vs eventual consistency
* Map reduce
* Multithreading (consistency, locks, CAS)
* Concurrency, threads, deadlocks, starvation

Practicality - web
* Bloom filters and count-min sketch
* Design patterns (OOP)
* Cashing in DBs
* TCP/IP
* IPv4 vs IPv6
* DNS lookup
* HTTP vs HTTP2 vs websockets
* TCP vs UDDP
* Data centers and how they work
* Public key infrastructure, symmetric and asymmetric encryption
* CDN - content delivery network - important for streaming

List from Google
* Processes, threads, concurrency
* locks, mutexes, semaphores, monitors
* deadlocks, livelocks, and how to avoid them
* resource allocation
* context switching
* scheduling
* multicore concurrency

Practicality - local
* Parts of a modern OS? Levels of cashing in a modern OS?
* Performance (bandwidth) of CPU / memory / SDD / HD / network
* How to use sequential (as opposed to random) reads and write on HD to speed up your processes
* Paxos - what is it?
* Virtual machines and containers

Solutions and tools
* MongeDB
* Couchbase
* Memcashed
* Redis cluster
* Zookeeper
* Kafka
* NGINX, HAProxy - balancers
* Solr, ElasticSearch
* Blobstore

# System interviews mindset

* No single good answer, but some answeres are objectively bad.
* Ability to take a poorly (vaguely) defined problem, make sense of it, define and describe it, and then break it apart into a set of doable, actionable tasks (modules, items). Identify constraints, and satisfy them. And also explain it. Try to be organized and clear.
* Most information is not available originally, and should be requested, remembered, calculated, or guesstimated (which is fine, as IRL you'll look it up).
* Numbers are important in this context, as in some cases they define what is a constraint, and what isn't. Back-of-envelope calculations.
* Consider various aspects, bottlenecks, and targets is good (compute, memory, data, privacy, complexity, costs, support, scalability etc.)
* The problem is never "solved", in a sense that there is never enough time to cover all possible things that can be said about it. The weird rule of the game is that you need to keep approaching the problem from different aspects and challenging it, to demonstrate ability to plan and lead product development.

Some people say that you shouldn't really ask what are the defining features, and how much the system should scale, but sorta make hypotheses about that, state them very clearly, and overall use it as another dimension to the problem, to look around it (if this is important, then we can do this; if that other thing is important, then we can do that). Others say - ask questions, make sure you get from the interviewer what features are important and which ones aren't.

Typicall, with real white boards, they always expect you to draw block diagrams.

# Questions to ask (oneself)

Identify:
* Key features, functionalities, tasks
* APIs (how different parts will interact)
* Abstract database structure: what tables, and how they are linked
* Bottlenecks (compute, storage, data, speed, availability) and targets. What to optimize for? What needs to be fast? What can be cashed? What asynchronies are permitted?
* Scalability, security, privacy, complexity, consistency
* Trade-offs. Can something be pre-computed, cashed? Can we trade memory for speed?
* Can we further optimize, by sending some requests through cashed, and some through direct pipelines, depending on some of their aspects?
* What are the APIs for developers; is it flexible enough?

Example: for Twitter, it's easier to keep pre-calculated timelines and update all of them with every update, instead of writing a new tweet only once, and SQL selecting it after. It however creates a computational-graph-like problem, because you may see response-tweets before original tweets, if original tweets weren't yet precomputed.

Other considerations:
* Average compared to peak use
* Distributions of values; patterns
* Upper and lower bounds

# Web services

**Scaling**: **Vertical** - better CPU, better RAM etc. **Horizontal** - parallelize, potentially with cheaper hardware.

**Load balancer**: a process of distributing tasks over resources. May involve scheduling tasks of different size; scheduling of tasks that depend on each other (there are heuristic methods to optimize computation of a directed graph that arises from dependencies); and effective segregation (splitting into "independent" tasks that can be parallelized).

Possible balancing approaches for a client-server architecture:
* Server-side balancing (necessary for horizontal scaling)
* Client-side random load balancing (randomize where the requests are sent)
* Round-robin (common for DNS/IP services): resolve into a list of IP sorted by priority, and the list is adjusted (permuted) for every response. The client tries IP in the order, and goes to a diff IP after a time-out. An IP that ended up working is used again, but with a certain expiration time. ([ref](https://en.wikipedia.org/wiki/Round-robin_DNS))

In a **Data Center** lots of servers have private IP addresses, but share one public IP address, with messages managed by a load balancer. TCP/IP within the data center.

# OS and hardware

Quad-core processors have parallelism (4 cores, so can do 4 things at a time).

Working with databases requires faster drives, as typically involves lots of writing. So one of the bottlenecks for a server.

# Refs

Lists of topics and keywords:
* https://www.palantir.com/2011/10/how-to-rock-a-systems-design-interview/