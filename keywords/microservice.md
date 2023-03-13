# Microservice

Parents: [[system_design]]
See also: [[docker]]

#systems

Subtopics:
* [[kubernetes]] - key technical solution
* [[ci]] - Continuous integration, a critical approach when your service never sleeps


**Microservice**: instead of one big program, you have several small applications, each only supporting one function (users), and talking through each other via some API (like http). Each either talks separately to a database, or even has its own database.

Because of the abstract API, they can be written in different languages. Can be updated separately, supported by different teams, more resilient to crashes. But most importantly, can be dynamically scaled at runtime (you can always containerize them and just run more if you need them). See [[docker]].

But as a downside, complexity. Harded to support (lots of info to learn or remember); require fancier infrastructure (not just one program running on one server).

# Refs

Intro by James Quigley: https://www.youtube.com/watch?v=1xo-0gCVhTU