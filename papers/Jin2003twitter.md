# Jin 2013

Jin, F., Dougherty, E., Saraf, P., Cao, Y., & Ramakrishnan, N. (2013, August). Epidemiological modeling of news and rumors on twitter. In Proceedings of the 7th workshop on social network mining and analysis (pp. 1-9).
http://people.cs.vt.edu/~ramakris/papers/news-rumor-epi-snakdd13.pdf
(~200 citations, so not bad, but not a seminal paper)

#networks

Parent: [[sir]]

**SEIZ** model (with immunity) is also good for modeling fake news, in which case E is exposed, and Z (immune) can be interpreted as "skeptics". Except for fake news it may be better described as kinda "competing transitions" to two final states, I and Z. Like two infections that compete with each other (somewhat reminiscent of decision-based models: XXX). Then S may transition to either I or Z, but also to E (unstable state) that can fall to I later.

> My intuition would be to make the transition tree symmetric on the I/Z axis, and assume that E eventually falls to either I (conspiracy theorist) or Z (rational). But for them empirically a forever-uncertain E is probably not different from Z, so they only consider conversion from E to I, but not from E to Z. Their diagram of transitions is almost symmetric, but not quite: with this exception.

One can then solve ODE equations every time, and fit the dynamics to actual data, thus estimating the parameters. One can also compare different models in terms of their fit. In this case, for example, a SEIZ curve fits real data much better than a SIS, curve; probably just because the SEIZ system is more expressive.