# Scrum

#tools #lifehack #management

Highly formalized, open, collaborative product development framework framework for small or medium-sized teams. Iterative, incremental, team-centered.

# Roles

* **Product owner**: The voice of the customer. Owns the "Product backlog". Lots of negotiation and communication here.
* **Developers** - subj
* **Scrum master** - kinda a facilitator; apparently not a project manager, but someone to help to follow scrum methodology and resolve external conflicts? Educate on the methodology, and promote self-organization.

# Terms

* **Sprint** - basic unit of develpment; typically 1-2 weeks.
* **Story** - A single `to-do` item (basic unit of work) that can be done (or not)
    * With a full formal process, story names have weird names of a format "As a <stakeholder type> I want <software feature> So that <business value>". There are some concerns about this format tho [^1] (and personally I find it ugly and ineffective, especially on small screens)
* **Story points** - an integer number, roughly reflecting (non-linearly) the number of hours it would take to finish a task, but also novelty, associated risks, and a need for interaction. A sample scale could look like this:
    * 1: simple task, 1-2 hours
    * 2: clear, but some work, half a day (4 hours)
    * 3: involved but straightforward, 1 day
    * 5: also needs some follow-ups or some pre-work, 2 days
    * 8: complex, somewhat novel, major point: 3 days
    * 13: about a limit of what 1 developer can commit to for 2 weeks, as some research, iterations, and follow-ups may be needed; blocking is possible. About 5 straight full days of work.
    * 21: About 10 days of work, definitely needs to be split into small pieces before it is accepted into a Sprint.
    * All of this assumes an about 6-hours working day (which frankly, commenting from myself here, is not generous at all, considering that other scrum activities and development-related meetings probably consume at least 1 hour a day, on average, maybe closer to 2 hours a day!) The capacity of one person in story points, for 2 weeks, is assumed to be 26.
    * The default valuations of story points form a Fibonacci sequence, to give them this exponential-ish curve, reflecting the actual perception of projects. And then the time grows even faster than the number of story points.
* **Epic** - A subproject of the main project, or some type of an ongoing reason (linked to a target, kpi) to have the work done. Basically, a larger logical unit to house Stories.
* **Backlog** - Like a target to-do list of product requirements. Includes features, bug fixes, non-functional requirements (topics like safety, security, usability, scalability etc.). Often described as use cases and "user stories". Visible to everyone, but only modified by the product/process owner. Ranked by by value to the customer and effort to implement (?), classified on a Fibonacci scale (??? 🔥)
* **DoD** - "Definition of Done", the completion criteria (not just the code, but maybe also testing, certain actions on the repo, documentation, scrum actions)
* **Burndown chart** - estimated amount of remaining work, in hours. Usually goes down, but can go up a bit sometimes.


# The process

* **Sprint planning** - here everything starts.
    * A list of Epics is formulated.
    * Every member is assigned an epic to review.
    * Every user, for the epic(s) assigned to them, creates all stories that they foresee to happen here. At this stage, these stories go to the backlog (are . In some teams, they also estimate the effort (story points, required time); in some - it is left for the entire team at the next step (Grooming). It is generally better to create 
* **Sprint Grooming** - typically happens on Friday, before the new Sprint (that would start on Monday). The team gets together (it's a longer meeting), and goes through the following step:
    * All `To do` items are moved to the new sprint
    * All `In progress` items that weren't finished, but where there was some progress, are moved to the new sprint, but reduced by the amount of work done
    * For all new `To do` items, the team agrees on effort estimations.
    * Stale and dead-end items are pruned: either moved to backlog and deprioritized, or moved to `Reject` (if this status is supported - not all teams support this status), or maybe even moved to `Done` (if it is no longer needed, as the issue was resolved in some other way?)
    * Individual stories (action points) are assigned to team members. There is a target of a certain number of story points per person (slightly less than 12 per week?), unless there are special circumstances, and this is used for planning. It's ok to leave some buffer for unexpected work.
* **Sprint review** - discussing the results for the last sprint. Is often split into "Achivements review" (sprint review proper) and "Technical review".
* **Sprint retrospective** - not sure, but I think, discussing the overall scrum methodology, and whether the process needs to be improved?

On top of it, there's the main regular daily meeting:
* **Daily scrum** aka **Stand-up** - always starts on time, limited to 15 min, open to visitors. Each member shares their contribution from yesterday, their plan for today, and if there are any impediments (risks, blocks etc.) No detailed discussions - these are delegated to separate follow-up meetings.

# Footnotes

[^1]: About how "full-blown scrum" story names are weird:  https://blog.crisp.se/2014/09/25/david-evans/as-a-i-want-so-that-considered-harmful 

# Refs