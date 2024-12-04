# Systems interviews

Parents: [[job_search]], [[system_design]]
See also: [[behav_interview]], [[ml_questions]]

#interview #lifehack


Key principles:
* There's no single good answer, but some answeres are objectively bad.
* Ability to take a poorly (vaguely) defined problem, make sense of it, define and describe it, and then break it apart into a set of doable, actionable tasks (modules, items). Identify constraints, and satisfy them. And also explain it. Try to be organized and clear.
* Most information is not available originally, and should be requested, remembered, calculated, or guesstimated (which is fine, as IRL you'll look it up).
* Numbers are important in this context, as in some cases they define what is a constraint, and what isn't. Back-of-envelope calculations.
* Consider various aspects, bottlenecks, and targets is good (compute, memory, data, privacy, complexity, costs, support, scalability etc.)
* The problem is never "solved", in a sense that there is never enough time to cover all possible things that can be said about it. The weird rule of the game is that you need to keep approaching the problem from different aspects and challenging it, to demonstrate ability to plan and lead product development.

Some people say that you shouldn't really ask what are the defining features, and how much the system should scale, but sorta make hypotheses about that, state them very clearly, and overall use it as another dimension to the problem, to look around it (if this is important, then we can do this; if that other thing is important, then we can do that). Others say - ask questions, make sure you get from the interviewer what features are important and which ones aren't.

Typically, with real white boards, they always expect you to draw block diagrams.

# For data specifically

As of 2023, "starting small" (building a start-up, or responding to a question about a hypothetical startup) probably includes [[postgres]], [[docker]] on [[kubernetes]], and [[airflow]] to run ETL jobs. Probably [[aws]] as the cloud provider.

As you grow (or "grow" within the fantasy universe of an interview), you can mention [[dbt]]

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