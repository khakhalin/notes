# Outbreak detection

Parent [[graphs]]
Related: [[graph_cascades]], [[sir]], [[influence_maximization]] 

**Problem**: a network is given, and every now and then there's a spreading process on this network (outbreak, contamination). Where should you place your sensors to detect contamination as quickly as possible? Or a social network example: posts are spreading through the network (as retweets). Which accounts should one follow to detect news and trends (cascades) as soon as possible? Similarly, can be applied to network security.

Sorta the opposite of (an inverse problem for) [[influence_maximization]]. "Something" is trying to start cascades, and you need to fight them. What matters is that some cascades are larger than others, and we are interested in detecting (1) large cascades (2) early. So that's where it is similar to influence maximization: we probably want to be close to a place that tends to be in the beginning of big outbreaks.

And once we can place more than one sensor, we have this coverage problem with **submodularity**, again exactly as for _influence maximization_: each sensor essentially "quenches" outbreaks (detects them and somehow prevents them), which means that some part of the network is now "covered", and further sensors should be placed in parts of a network that are not yet "covered". Submodular **utility function** f(S).

But unlike before, now there's also a notion of **cost**: following a high-volume poster, or sampling a high-volume tube is more costly.

* What kind of **dataset** becomes our input here? Time when each outbreak (cascade) i hits node u: T(u,i), on a graph G(V,E).
* What are we trying to optimize (**our goal**)? Find a subset of node S (those to place sensor on) that maximize some combo of utility and cost: $\displaystyle \max_{S} f(S) = \sum_i P(i)f_i(S)$, given $cost(S)<B$. Here $P(i)$ is a probability of a certain ("type" of a?) outbreak, and $f_i (S)$ is the impact of our placement on this particular outbreak. For cost, to keep it easier, let's not plug it into the objective function, but just consider a hard constraint for the total budget, B.
* How to **define reward** f(S)? Several options here:
    1. Minimize time to detection
    2. Maximize number of detected cascades
    3. Minimize total number of affected nodes across cascades - the most realistic one in most cases
* **Costs** are task-dependent (may be flat, or may be proportional to the total flow through the node)

Let's now formalize it, by defining a **penalty** π for detection outbreak i at time t.
1. If interested in time to detection: π(t) = t
2. If interested in the total number of detected cascades: π(t)=0, but π(∞)=1 (where ∞ probably has some practical limit-value)
3. If minimizing affected nodes: π(t) = number of nodes affected by time t.

Notes:
* The total effect of outbreaks is additive, which is nice
* Detecting earlier never hurts (monotonous in respect to it)
* Depending on what objective we choose, we may get slightly different optimal solution. For example, in a real flow network, if you want to minimize impact, your sensor will cluster where most nodes are, and neglect remote parts of the network, while if you care about the number of detected cascades, you'll place sensors much more uniformly.

Our **utility function** f(S) will be now defined as **penalty reduction**: f(S) = π(∞)-π(T(S,i))

> I wonder how the math would change if detection is probabilistic. Is it a trivial generalization, or will it introduce some non-obvious complications?

**Proof of why f() is submodular**: if A⊆B, then adding a new sensor to A gives a bigger gain than adding a new sensor to B (because more is covered by B). That is: $f(A \cup \{u\})-f(A) ≥ f(B \cup \{u\}) -f(B)$. We can do it for one outbreak only (because $f()$ is a linear combination of  $f_i()$ for individual outbreaks with outbreak probability coefficients $P_i$). And because f_i() are defined in terms of π_i, and π_i is an ever-growing propagation, we get submodularity.

Here's why: consider 3 cases:
1. u detects outbreaks later than either A and B - in this case it doesn't matter, nothing changes
2. u detects outbreaks earlier than both - so no difference in this case, the changes are the same
3. And finally, if u is in-between B and A (after B, but before A), then it benefits when added to A, but doesn't bring benefit if added to B. So that's when ">" happens.

For submodular functions with flat costs, we know that a **greedy algorithm** (aka **Hill-climbing**) is guaranteed to give a reasonable solution that is at worst (1-1/e) worse than the true optimal solution (see [[influence_maximization]] - although to be honest we never gave a proof there). But in a more general case, costs may not be flat (each node comes with a cost). And this breaks the greedy algorithm. And also, the expected cost to put K sensors for a full greedy climb is $O(N_V \cdot K)$, which is slow.

# CELF algorithm

Consider naive **benefit-greedy** approach: add sensors ignoring the costs $c(u)$, and just stop once the budget is exhausted. Intuitively, one can create counter-examples where this will go arbitrarily bad. We can make a set of nodes have a marginally negligible improvement in utility, but a ridiculously overwhelmingly higher cost that will immediately exhaust the budget, even tho an ε-negilibly-worse solution was right there, in plain sight.

What if we use the **benefit/cost ratio**? $s_i = Δf(u)/c(u)$ . We can make it fail as well, by creating near-useless sensor that are extremely cheap, and the benefit-cost overshoots as you divide by near-zero. You'll start buying cheap but useless sensors, exhausting your budget, instead of investing into something reasonable (but a bit more expensive).

But the interesting thing is that these "counterexamples" are in some way polar opposites of each other: in one "bad" nodes have slightly better performance and very high cost, in the other they had slightly worse performance and very low cost. Each counter-example 1 would work just fine with approach 2, and vice versa.

Enter **CELF: Cost-Effective Lazy Forward-Selection**:
1. Run benefit-greedy: get suggestion S1
2. Run unit-cost (cost-ratio) greedy: get suggestion S2
3. Of S1 and S2, pick whichever is better. And we don't do it at every step, we really calculate just two full solutions, and pick the better one.

Quite curiously, CELF is **near-optimal**, and is never worse than true optimum than the (1-1/e)/2 coeff. Which is wild, if you think of it, as both algorithms independently seem so naive, and potentially crappy, and yet the combo of them is guaranteed to work.

# Speeding it up with lazy evaluations

**Lazy**: only compute stuff when it's needed. Even in CELF we need to compare the utility of sensors, and we need to redo it after each sensor is added, as marginal utilities (improvements) change. $O(N_V \cdot K)$ still. But we know that they don't just "change", but monotonously go down, due to submodularity, and it can help. Let's keep a [[priority_queue]] of all nodes, from best (highest utility) to worst. After a new node is added, start looking at top nodes in the queue. A later node can never jump up (maginal utilities never improve), the only change that can happen is that former leaders will "sink down". So once we have one updated value on top of the queue, it is guaranteed to be the winner, as all others will be 
* either already for-real lower than it (if they were updated and checked), 
* or "guaranteed to be lower", if they weren't updated, but were lower than the current (updated) winner even in their previous iteration. This means that they are probably even lower now, but there's no point in calculating the exact value. Until the true winner (the one we hope to find) falls under "un-updated values", there's no reason in recalculating these values.

In the worst case, we can of course make it as slow as greedy, by creating a graph where the new winner is always the previous looser. But in most real cases it will be a substantial optimization (~2 orders of magnitude, for realistic networks?).

> I think one possible "worst case" would be a linear chain graph where at first one end wins, and improvements fall down linearly. Then at each step the "next best sensor" will be the one furthest from the previously placed sensor, and we'll be eating this chain from both ends, taking turns, so the "last worst" will always be "the new best". But it is a weird scenario, that only works with these unrealistic linear influence function that don't decay spatially. With every spatial decay we immediately get spatial fragmentation, and thus an improvement.

# Data dependent bound on data quality

On **easy data** greeding algorithms may do better than (1-1/e). It takes **adversarial data** to worsen the solution to its "worst guarantee". Can we recognize easy data?

Let f(S) be our "heuristic solution", f(opt) be a true optimum, nodes {u} be the nodes that separate us from imperfect to perfect solution, and δ(u)=f(S ∪ u)-f(S) be marginal improvements given by each node u, upon our imperfect solution. Then $f(opt)-f(S) ≤ Σ_uδ(u)$. This is kinda obvious from submodularity, as regardless of the sequence in which we'll be adding u, real benefits will shrink, due to submodularity.

And it gives us an additional guarantee. For whatever imperfect solution we have S, we can find several other "finalists", and guarantee that a true f(opt) is not further from the imperfect solution than f(S)+Σ of these marginal gains δ(u). _Not sure I understand that, as we don't know the overlap between S and f(opt), so we don't know how many more nodes to add, but whatever. I give up here._

Interestingly (Ostfeld water resource planning) CELF works better than any network-based proxies (random in space, degree-based etc.)

# Refs

CELF:
Krause, A., Leskovec, J., Guestrin, C., VanBriesen, J., & Faloutsos, C. (2008). Efficient sensor placement optimization for securing large water distribution networks. Journal of Water Resources Planning and Management, 134(6), 516-526.

That CELF works better than network-based heuristics:
Ostfeld, A., Uber, J. G., Salomons, E., Berry, J. W., Hart, W. E., Phillips, C. A., ... & di Pierro, F. (2008). The battle of the water sensor networks (BWSN): A design challenge for engineers and algorithms. Journal of Water Resources Planning and Management, 134(6), 556-568.
(500 citations)

Lecture:
https://www.youtube.com/watch?v=V-qd8Thwysk

Slides:
http://web.stanford.edu/class/cs224w/slides/15-outbreak.pdf