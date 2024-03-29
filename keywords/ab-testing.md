# A/B testing

Parent: [[stats]]
See also: [[effect-size]]

#stats #abtesting


He estimated (both modeled and derived, I think) the cost of running A/B testing for a certain amount of time. What power should you aim at? (Hint: not necessarily 80%; it's just another magic number).

Essentially, as time goes on, sample size increases, and the chances of making a mistake and picking a wrong option decrease, what it seems, exponentially. (Probably not at all exponential, but it kinda looks like one). The benefits grow real fast, as you are more and more likely to pick a correct answer as your sample size increases.

At the same time, running 2 options at the same time is costly, so instead of saturation at fixed benefit we the have a slow linear decline. Which means that there is actually an optimal time of running an A/B testing.

> I'm guessing, in practice one can either google it, or model it. At least for a given effect size it is fairly straightforward to model. With an unknown effect size, I'm less sure, but it would be a fun model to play with.

Practical consequences, and somewhat unintuitive advice for when underpowered experiments are best, and high power is not actually desired:
1. If the costs of testing is high (he calles it "high discount rate"). That's clear.
2. When the user base is small, the duration curve will be about the same, but at optimal duration, the power will still be fairly small. Just because, again, fixed costs of running a continuous test may easily overshadow the benefit of making a correct decision.

But that said, running an experiment for too long by mistake is almost always better than cutting it too short, as the decrease is slow and gradual, while the first kinda-exponential improvement is pretty steep.

# Refs

A nice blog post by Chris Said, Jan 2020: https://chris-said.io/2020/01/10/optimizing-sample-sizes-in-ab-testing-part-I/
