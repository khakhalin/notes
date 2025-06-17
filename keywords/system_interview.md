# Systems interviews

Parents: [[job_search]], [[system_design]]
See also: [[behav_interview]], [[ml_questions]]

#interview #lifehack


Overall structure:
1. Understand the problem, set the scope (5 min; ask many questions, know goals, features, numbers).
2. Propose high-level design, and get a buy-in on it (longerst part, half of all time. For a 45 min interview, at least 20 min)
    1. Start with defining a list of key features. Outline key use cases / scenarios we want to cover
    2. List key non-functional requirements to satisfy: availability, consistency, speed, security, latency
    3. Describe key actors (in the simplest case, backend & frontend?)
    4. Describe [[api]]s (aka endpoints) for each actor. Try to make them [[restful]] by default. First external, then internal. Review them. Don't allow feature creep at this stage. Maintain a list of talking points for later, don't deep dive or freeze tech assumptions too early
    5. Data model and schema. Estimate data access patterns and read/write ratio
    6. Propose techstack (only main idea)
3. Design deep dive (remaining time, open-ended, usually explores only 1-2 aspects of the story)
    1. Identify potential bottlenecks, dangers (edge cases), constraints. Peak usage, hot users etc.
    2. Addressing bottlenecks and scaling up. How would the techstack change
    3. Request (explicitly or implicitly) feedback, to check if the interviewer is expecting something in particular
    4. For any important problem, try to come up with at least two solutions
4. Summary (wrap-up: very short, almost a memorable punchline)

Key principles:
* There's no single good answer, but some answeres may be objectively bad.
* We are looking for an ability to take a poorly (vaguely) defined problem, make sense of it, describe and define it, and break it apart into a set of doable, actionable tasks (modules, items). Identify constraints, and satisfy them. Also explain it to stakeholders, to make them buy in the proposed design. We're looking for organization and clarity.
* Most information needed for the solution is probably not available originally, and should be requested, calculated, or guesstimated (which is fine, as IRL you'll look it up or go through a research phase).
* Numbers are important, as in some cases they define what is a constraint, and what isn't. Back-of-envelope calculations.
* Considering various aspects, bottlenecks, and targets is good (compute, memory, data, privacy, complexity, costs, support, scalability etc.)
* The problem is never "solved", in a sense that there is never enough time to cover all possible aspects of the problem. The weird rule of the game is that you need to keep approaching the problem from different aspects and challenging it, to demonstrate your ability to plan and lead product development.

Elements to play with: [[caching]], sharding, balancers, rate limiters, [[acid_base]], [[streaming]]

Key challenges and their solutions:
* Read-heavy - [[caching]] (e.g. [[redis]])
* Write-heavy - [[streaming]] (e.g. [[kafka]], [[rabbitmq]]). Also tree-based DBs (e.g. cassandra)
* Fail-proofing DB - redundancy, replication (sync, async)
* Balancers
* Edge computing 🔥 ???
* Indexing, Sharding

For data reading and writing, consider a range of possibilities:
* relational db (aka [[sql]]) - complex queries, strong (acid) consistency, hard to scale horizontally (aka "more servers")
* [[nosql]] - simple queries, high throughput, eventual consistency. Very fast, but no more than 1-2 optimized keys.
* in-memory storage (e.g. [[redis]]) - key-value store, realtime temporary data
* message queue (e.g. [[rabbitmq]], [[kafka]]) - event-driven asynchronous processing
* [[datalake]] - relatively fast write, slow read, unstructured data. Archival
* distributed cache - for caching of frequently accessed computationally expensive data
* file storage (e.g. blob) - large, simple

Some people say that you shouldn't really ask what are the defining features, and how much the system should scale, but sorta make hypotheses about that, state them very clearly, and use it as another dimension to the problem, to look around it (if this is important, then we can do this; if that other thing is important, then we can do that). Others say - ask questions, make sure you get from the interviewer what features are important and which ones aren't. The truth is probably in-between (and also your chance to demonstrate that you can build rapport with people).

Typically, with IRL white boards, you are always expected to draw block diagrams.

**Requirements**:
* Functional: key features to build
* Non-functional: scale, performance, accuraccy, freshness, consistency, security, safety, transparency, testabiliy, scalabiliy, usability, compliance (data protection, open source etc). In the context of an interview, scale & performance are usually the key

# For data specifically

As of 2023, "starting small" (building a start-up, or responding to a question about a hypothetical startup) probably includes [[postgres]], [[docker]] on [[kubernetes]], and [[airflow]] to run ETL jobs. Probably [[aws]] as the cloud provider.

As you grow (or rather, "grow" within the fantasy universe of an interview), you can mention [[dbt]].

Always remember that data / systems interviews also have a [[behav_interview]] aspect to them: show that you are thinking practically, from the busines pov, using tools as means to an end, not as a goal in itself.

# Questions to address

Identify:
* Key features, functionalities, tasks
* APIs (how different parts will interact)
* Abstract database structure: what tables, and how they are linked
* Bottlenecks (compute, storage, data, speed, availability) and targets. What to optimize for? What needs to be fast? What can be cached? What asynchronies are permitted?
* Scalability, security, privacy, complexity, consistency
* Trade-offs. Can something be pre-computed, cached? Can we trade memory for speed?
* Can we further optimize, by sending some requests through cached, and some through direct pipelines, depending on some of their aspects?
* What are the APIs for developers; is it flexible enough?

Example: for Twitter, it's easier to keep pre-calculated timelines and update all of them with every update, instead of writing a new tweet only once, and SQL selecting it after. It however creates a computational-graph-like problem, because you may see response-tweets before original tweets, if original tweets weren't yet precomputed.

Other considerations:
* Average compared to peak use
* Distributions of values; patterns
* Upper and lower bounds

# Refs

Lists of topics and keywords:
* https://www.palantir.com/2011/10/how-to-rock-a-systems-design-interview/

Systems design interview (a short video summary of a book)
* https://www.youtube.com/watch?v=i7twT3x5yv8