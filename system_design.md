# System Design

See also: [[design_docs]], [[swe]]

#swe #systems #bib


Subtopics:
* [[api]]: [[restful]], websocket etc.
* [[parallelism]] - general concepts of concurrency
* [[streaming]] - Message queues and message brokers
* [[datalake]] - First level of precipitation of data, as it goes from streams/queues to databases
* [[database]] - Everything databases
* [[microservice]] - Everything microservices

See also:
* [[system_interview]] - collections of tips regarding system interviews
* [[hardware]] - Everything hardware
* [[fairness]] - an ethical aspect of system development


# Scaling

**Scaling**: **Vertical** - better CPU, better RAM etc. **Horizontal** - parallelize, potentially with cheaper hardware.

# Balancing

**Load balancer**: a process of distributing tasks over resources. May involve scheduling tasks of different size; scheduling of tasks that depend on each other (there are heuristic methods to optimize computation of a directed graph that arises from dependencies); and effective segregation (splitting into "independent" tasks that can be parallelized).

Possible balancing approaches for a client-server architecture:
* **Round-robin** (common for DNS/IP services): resolve into a list of IP sorted by priority, and the list is adjusted (permuted) for every response. The client tries IP in the order, and goes to a diff IP after a time-out. An IP that ended up working is used again (caching), but with a certain expiration time. ([ref](https://en.wikipedia.org/wiki/Round-robin_DNS)) But it means that within-user load is not balanced, which is a potential issue.
* Client-side random load balancing (randomize where the requests are sent)
* Server-side balancing (necessary for horizontal scaling). May combine round-robing (randomizing) with more sophisticated load balancing.

In a **Data Center** lots of servers have private IP addresses, but share one public IP address, with messages managed by a load balancer. TCP/IP within the data center.

# Hardware

Quad-core processors have parallelism (4 cores, so can do 4 things at a time).

Working with databases requires faster drives, as typically involves lots of writing. So one of the bottlenecks for a server.

# ML in production

Two architectural decisions to make:
* Training:
    * **Offline training** - easy, testable (both for crazy effects, and for the overall quality of the model), computationally cheaper (run less frequently, optimized, can be scheduled), but can go stale, and needs to be monitored at inference
    * **Online training** - harder to implement, more computationally demanding, harder to ensure quality, but auto-adjusting
* Inference:
    * **Offline inference** - saving on compute, easy to catch crazy outputs early on, but potentially losing on storage; have to predict all possible inputs in advance.
    * **Online inference** (aka online serving, or predicting on demand) - by definition, covers all inputs, but much more compute-demanding (as there's a  limit on max affordable latency); needs to be monitored very closely, and still there's a risk of exposing users to bad results, with all sorts of bad consequences happening.

### Data dependencies

Input features that used for training and eventual functioning of a model are somewhat analogous to code modules in classical programming, as they come with maintenance, need to be monitored, and tested. But creating "unit tests" for features may be hard ([[unit_test]]). Things to consider:
* **Reliability** (always present, calculatable in the same way). If not - need a backup plan.
* **Versioning** - will the process computing this feature change over time? If yes (say, different formats), we need to enable versioning: store the version of the code that produced the feature, on top of the "value" of the feature, so that we could choose between several ways of processing it, without abandoning older, legacy data.
* **Necessity** - some features just create technical debt and direct costs without meaningfully improving the model. Something reminiscent of [[regularization]].
* **Correlations vs causality** - here some testing and studies are useful
* **Feedback loops** - if we are going to update our model, we need to think about how the performance of our model (outputs) will affect consecutive iterations of training data (inputs). (The crazy textbook listed on Amazon for millions of dollars is a good example)