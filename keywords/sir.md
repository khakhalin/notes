# SIR, SIS, and other probabilistic processes on graphs

#graph #chaos

Parents: [[09_Graphs]]
See also: [[graph_cascades]] graph_spread

**Epidemic-like spreading processes**: no explicit decision-making (unlike in [[graph_cascades]]); processes are probabilistic. But there is still a tree (a branching process). Formalism: each node "meets" d new people, and with probability q infects each of them.

For which (d,p) will the pandemic run forever? 

$\displaystyle \lim_{h→∞} P(\text{at least 1 infected node}) >0$, where h = tree depth. If the process dies out, P converges to 0.

Recursive problem, so we can write it down as such: $p_h = 1-(1-qp_{h-1})^d$

Let's iterate this: $f(x) = 1-(1-qx)^d$, with $x_0 = 1$. We can draw this function and solve it geometrically (in the sense of identifying fixed points). In our case the function $f(x)$ is monotone, which is easy to see if we calculate $f'(x) = qd(1-qx)^{d-1}>0$. Also, $f'$ is non-decreasing, and $f'(0)=pq$, which means that for pq>1, the curve is above the line, and we have a single fixed point of x=1 (single coz f' is not just monotone, but also non-increasing). Otherwise (if pq<1), we have a single fixed point x=0. So a simple result: depending of how pq compares to 1, either everyone get sick, or it dies out.

$dq=R_0$ - a fundamental property for every pandemic, as the final outcome depends on it. Either way, behavior is exponential.

In the wild (without any measures?), for diseases, measles has R_0 of about 15, ebola of about 2 etc. But measures (practices, quarantines etc.) change R_0. In social networks, likes also spread as pandemics, and if like timestamps are accessible, the cascades can be reconstructed.

We can estimate R_0 as $\displaystyle R_0 = qd \frac{\text{mean}(d^2_i)}{(\text{mean} (d_i))^2}$, to correct for the non-iniform degree distribution in the network, where d is the average degree.… _Wait, why doesn't it get canceled out with the denominator then? Check the formulas, as this doesn't make sense without additional info. _ #todo

Or in some cases we can measure R_0 empirically by explicitly tracing for each affected node how many nodes it affects, but to do so we may need access to more detailed (and well-observed) information.

In practice, even the behavior of the very first tweet (the number of direct retweets) is a good proxy for whether it will go viral.

> From this POV, a good design of a social network should allow some viral transmission, to keep it fun, but we can regulate how R_0 changes with time to control a size of a typical cascade (by making R_0 drop in time).

**A generalization**: let's assume that each node can have different **states**, and transitions between these states with different probabilities. A whole family of models that are named depending on which of these states are considered in the framework. The possible states include:
* S = susceptible (naive)
* E = exposed
* I = infected
* R = recovered
* Z = immune

The nodes are connected in the S→E→I→R sequence, with Z added to it as some sort of an offshoot, and all nodes linked back to S node, except that all transition arrows are assigned a probability, and some of these probabilities may be quite low, essentially closing a flow.

## SIR

Simplest model: **SIR** - just a direct straightforward process, where R doesn't link back to S, either because you die, or because you are immune forever. Two probabilities: β for getting infected, and δ for recovery.

One way to solve it analytically is to assume **perfect mixing** (essentially - a fully connected graph), in which case we can go from a cellular-automaton-like models to a system of differential equations:

$\displaystyle \begin{cases} I' = bSI - δI \\ S' = -βSI \\ R'=δI\end{cases}$

> I'm writing the equations out of order, as the equation for I' is easy to relate to, and the other two are a direct consequences of the first one. But the correct order would be SIR of course.

Nice smooth solutions with S flowing through I to R with some dynamics (one swipe through the entire population).

## SIS

Another model: **SIS** - where you get susceptible again.

$\displaystyle \begin{cases} I' = bSI - δI \\ S' = -βSI + δI \end{cases}$

The stable solution is a pair of values with a fixed value of I (a fixed presence of infection in the population), and a dynamic solution is a smooth slide from original (S,I) = (1, 0) to this (S,I) = (1-share, share).

For **actual graphs**, a threshold for an epidemic can be defined in terms of β/δ (above a certain value, epidemic happens, below - it dies out). The exact value of threshold $τ = 1/λ$, where λ is the largest eigenvalue of the adjacency matrix A of the graph.

> Do they mean it in a probabilistic meaning of P(pandemic) = 0.5? What is a pandemic in this case, all nodes? The majority of nodes? Most probably, here they work with a probabilistic case (where each node is simultaneously somewhat sick and somewhat healthy, equivalent to working with probabilities of being sick), and a process that is continuous in time (ODE rather than iterative equations). This way we can ignore probabilistic edge cases (of very few poorly connected nodes where everything is down to chance), as model always reaches a stable state (it just may take longer).

#todo: when was it described? What are some good references?

> Would be fun to see the math.

Does the original rate of infection (I_0) matter? Essentially, no (because of the exponential dynamics); it's either or, we transition from (1,0) being either stable, or unstable node. _"Exactly on the threshold" is probably a non-converging unstable solution, where P t→∞ just doesn't converge, right?_

# Jin 2013

Jin, F., Dougherty, E., Saraf, P., Cao, Y., & Ramakrishnan, N. (2013, August). Epidemiological modeling of news and rumors on twitter. In Proceedings of the 7th workshop on social network mining and analysis (pp. 1-9).
http://people.cs.vt.edu/~ramakris/papers/news-rumor-epi-snakdd13.pdf
(~200 citations, so not bad, but not a seminal paper)

**SEIZ** model (with immunity) is also good for modeling fake news, in which case E is exposed, and Z (immune) can be interpreted as "skeptics". Except for fake news it may be better described as kinda "competing transitions" to two final states, I and Z. Like two infections that compete with each other (somewhat reminiscent of decision-based models: XXX). Then S may transition to either I or Z, but also to E (unstable state) that can fall to I later.

> My intuition would be to make the transition tree symmetric on the I/Z axis, and assume that E eventually falls to either I (conspiracy theorist) or Z (rational). But for them empirically a forever-uncertain E is probably not different from Z, so they only consider conversion from E to I, but not from E to Z. Their diagram of transitions is almost symmetric, but not quite: with this exception.

One can then solve ODE equations every time, and fit the dynamics to actual data, thus estimating the parameters. One can also compare different models in terms of their fit. In this case, for example, a SEIZ curve fits real data much better than a SIS, curve; probably just because the SEIZ system is more expressive.

# Backstrom 2006

Backstrom, L., Huttenlocher, D., Kleinberg, J., & Lan, X. (2006, August). Group formation in large social networks: membership, growth, and evolution. In Proceedings of the 12th ACM SIGKDD international conference on Knowledge discovery and data mining (pp. 44-54).
https://www.cs.cornell.edu/home/kleinber/kdd06-comm.pdf
An influencial study (2k citations) about joining groups (on LiveJournal):

A more flexible approach to modeling: try to combine decision-based and probabilistic models, and derive a dependency between the probability of adoption, and the number of friends that adopted an innovation. p(k) is probably monotonously increasing, but the shape may be weird (S-shape, for a bistable, decision-like situation), or maybe even non-monotonous (if you are getting annoyed when all your friends convert, so you don't convert just out of a principle lol). The paper doesn't have all these pretty pictures though; it's mostly a descriptive analysis of what was happening with LJ.

# Romero 2011

Romero, D. M., Meeder, B., & Kleinberg, J. (2011, March). Differences in the mechanics of information diffusion across topics: idioms, political hashtags, and complex contagion on twitter. In Proceedings of the 20th international conference on World wide web (pp. 695-704).
http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.298.4938&rep=rep1&type=pdf
(~1600 citations)

Data from 2009-2011, 3B tweets, 60M users. Calculated the average "exposure curve" for top 500 hashtags (looking at hashtag virality). The curve grows really fast, then peaks and kinda platoes, with very slow decay.

Introduced a simple, almost visual measure of **curve persistence** = the area under the curve, divided by time ∙ peak (like a rectangle drawn around the curve). High persistence = low decay. Show that curves for different thematic hashtags have different average shapes: idioms and music decay fast (low persistence), while politics and sports linger. Music has a higher peak, compared to, say, tech and movies.

So it's mostly a paper about how different topics have different cascade dynamics, not about the math of it.

# Refs

Slides:
http://web.stanford.edu/class/cs224w/slides/13-contagion.pdf
Video:
https://www.youtube.com/watch?v=dKkJJX_sf6c