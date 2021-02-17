# Cohort analysis

#stats #tools #marketing

Parents: [[01_Tools]] (actually it's more of a behavioral analysis / practical stats technique?)
See also: a/b testing, simpson's paradox, survival basis


One of the strongest approaches to actual business analysis and troubleshooting.

Main idea: instead of looking at all actions of a certain type happening with the product (on the site, in the app), try to reconstruct the paths of a **cohort of users that share common characteristics**. Like slicing, but in a way that doesn't slice across threads of human experience, but parallel to it, preserving the threads. For example, we may look at users that:
* just registered at the site today
* made an order last week
* initiated a first session after a gap within a certain time window

Then identify cohorts for comparison, either by slicing similarly (older customers, expert customers, customers acquired through a different process, etc.), or by sub-slicing this cohorts into smaller sub-cohorts, say:
* regionally, 
* by certain aspects of the order / behavior / interaction that they had, 
* computer /  mobile / tablet, OS, browser
* style of how they used the web (cookie settings, incognito mode) 

Finally, define measures of interest, and compare between cohorts. For example:
* order size
* order completion
* pageviews
* revenue
* session duration

With some care, can be a part of randomized A/B testing (say, users that have seen different ads, or interacted with different versions of tools and controls).

Intuitively, cohort analysis that is careful enough could be the only way to fight things like **Simpson's paradox**, **survival basis** etc., which may be important for targeting/mistargeting (identifying correct target audiences), prioritizing correct business measures, etc.

#### Interesting lingo used in this context:
* **churn** - the rate of loosing users. Like, 1-retention
* **sticky features** - those that seem reduce the churn (that make people stay). Although correlation / causation of course.
* **vanity metrics** - the opposite of useful metrics; stuff that seems good, but actually doesn't help, and may even hurt (pursuing metrics that don't bring value makes you lose on things that do bring value)

#### General advice
* Annotate all changes in your product, news about the company, external event, etc. Will be crucial for understanding historical trends, peaks, etc.
* Drill down into sudden unexpected changes of use.
* For intentional improvements / marketing activities, don't go all-in, but test instead

# Refs

Wiki: https://en.wikipedia.org/wiki/Cohort_analysis

An example from Google Analytics, that in this case may be considered a tool specifically designed for cohort analysis: https://improvado.io/blog/how-to-run-a-cohort-analysis-a-step-by-step-guide

https://medium.com/analytics-for-humans/a-beginners-guide-to-cohort-analysis-the-most-actionable-and-underrated-report-on-google-c0797d826bf4