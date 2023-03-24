# Decision-based models

Parents: [[09_Graphs]]
Related: percolation, [[sir]]

#graph #complexity


**Cascading processes**: those where something propagates on a graph, creating cascades of node-level events. Examples: epidemics, diffusion of innovations, cascading failures in databases, spread of information, rumors, memes and viral marketing, fake news. In any of these scenarios, the propagation creates (or may be described as) a **propagation tree**, or a **cascade**. 

**Decision-based models** (good for social effects): nodes make **local decisions** (e.g. about product adoption) based on the behavior of their neighbors. For example, you join if k of your friends join. Not probabilistic for now. 

The idea of following your friends comes from game theory: if each player can have two behaviors, A and B, and behaviors are synergistic, it's always beneficial to adopt whatever behavior that is popular with your friends. A good IRL example: VHS vs Betamax; as people wanted to exchange cassettes, it was beneficial for people to use whatever everyone else were using, so one standard won without any clear reasons for why it had to win (it wasn't better in any way). Another good example: which communication system to use (WhatApp, Skype, GoogleHangouts, Telegram etc.)

**Payoff matrix**: AA→a (a>0), BB→b (b>0), AB or BA → 0. So as the node decides, it if has d friends, and p of them choose A, while 1-p chose B, the payoff is dpa if it goes A, and d(1-p)b if it goes B. What is the threshold? $dpa > d(1-p)b$ ⇒ if $p>b/(a+b)$ we go option A, else - option B.

Consider a scenario when everyone are using B. Now A showed up, and A is strictly better, but just a little bit (negligibly better, but better), so every node switches if 50% or more of its neighbors switches to A. Also, a small group of nodes uses A no matter what. What will happen? Depends on the topology, but it may propagate through the network. Articulation nodes however may present a problem, as the innovation won't spread through them.

> And at this point the game theory part becomes interesting: it may be beneficial to switch to A, but due to network inertia, A may not be accepted, or won't be accepted by the entire network, or will only occupy part of the network, but not the other part. Even tho a strictly better state is available (due to the locality of decisions, aka a network version of the tragedy of commons :)

One interesting consequence: **the process is not reversible** (unless something changes): nodes go in one direction only, so you will never have fewer converted neighbors than you have currently, so the drive to switch may only increase or remain the same; never decrease.

# Propagation of protest activity on social networks

**Example**: model protest recruitment on social networks; analysis of tags, actual study.
González-Bailón, S., Borge-Holthoefer, J., Rivero, A., & Moreno, Y. (2011). The dynamics of protest recruitment through an online network. Scientific reports, 1, 197. - 500 citations. 
https://www.scienceopen.com/document_file/8382cf11-fbd2-4c4f-8e7d-76bc8d104b1d/PubMedCentral/8382cf11-fbd2-4c4f-8e7d-76bc8d104b1d.pdf
Supplementary information:
https://static-content.springer.com/esm/art%3A10.1038%2Fsrep00197/MediaObjects/41598_2011_BFsrep00197_MOESM1_ESM.pdf

Austerity protests in Spain; recruitment happened on Twitter, so we deal with a graph of ~90k nodes (this part of a network) that exchanged ~0.5M messages. Back then collecting this kind of data was easier, as Twitter wasn't yet hiding (and selling) it. Directed graph, with an undirected core (strong network of mutuals).

Let's apply our decision-based model. For each node we can calculate:
* Activation time: moment when this particular user started tweeting protest messages
* k_in: the total number of friends they had at this moment
* k_a: the number of activated users among their friends at this omment
* De-facto activation threshold for this user will be $k_a / k_{in}$ (note that it's very case specific; no fake psychology can predict a universal value here). Threshold ~0 means that this particular node was an early adopter; ~1 means a conservative, slow node.

Actual distribution of activation thresholds in this study was almost uniform; a bit hill-shaped, with a drop-off for values close to 1 (very few people are that slow). Two spikes in the distribution: at near-0 (those who were already ready to start tweeting), and a strange narrow spike near 0.5. _Honestly, it does look like some numerical artifact; like one of those effects of small numbers. The spikes look a bit like a comb, so I think what happened is that people with a very small number of following were more abundant, and thus overrepresented in this sample_.

> In the paper itself: interesting analysis of N_a/N (the share of activated nodes) over time: a small exponential fragment every day with a slow start, rapid acceleration during the day, and almost instantaneous stop by the end of the day.

Interesting effect, allegedly: people got involved more readily if their neighbors experienced a burst of recruitment (if $Δk_a / k_a$ was large). A somewhat hard-to-relate plot of "cumulative derivative over time" (_whaaat?_), shown on a log scale, $\log(Δk_a / k_a)$ as a function of $k_a / k_{in}$ that looks like a bendy monotonous function (Fig 3A). And (judging from the paper) sensitivity to bursts is supposed to be reflected by the slope of this curve (the steeper - the more sensitive). _Like, whaaat? Neither the paper nor the lecture about it gives a clear formula for how this "cumulative log derivative" (in what order do these operations even go) is calculated._

> This formula is messed up. Judging from the fact that the curve isn't jittery, y either has to be a smooth function of k_a/k_in (as otherwise nodes with different k_in wouldn't have fallen on the same curve), or something like a cumulative probability distribution (an integral over x). As k_in isn't mentioned in this formula however, it has to be an integral. But it's not a clear cumulative probability distribution function either, as y has units. Which means that it is an integral of something. So what happened (probably) is that they found all points $(Δk_a / k_a \, , \, k_a/k_{in})$ as a fuzzy cloud, then sorted these points by $k_a/k_{in}$, and integrated by rank (calculated a cumulative sum over this array). And then they want us to look at the slope of this curve (differentiate back), which means that this whole operation is just an obscene way to smoothen the joint distribution of $Δk/k$ and $k_a/k_{in}$, and show which pairs of these values are more common. This is one of the worst presentation of simple data I've ever encountered. But the take-away is supposedly that medium-sensitive ($k_a/k_{in}$≈0.5) and low sensitive (≈1) people didn't depend on bursts that much, while medium-high threshold people (≈0.75) were sensitive to bursts. (And even this conclusion is contingent on their interpretation of a peak for threshold of 0.5, which in my opinion is incorrect, or rather, uncompensated for).

**How to identify cascades in the data?** We have to find them somehow; some of them will be successful, some will die out. And in fact most cascades are small (dying quickly).

An interesting (and more practical, from the marketing POV) question: if we only look at successful cascades, what points do they tend to originate from? Are they more central in some **centrality** sense? To answer, **k-core decomposition**. Def: **k-core** is the biggest fully connected subgraph in which each node has a degree of at least k. To find, incrementally remove nodes with degrees < k, and not once, but several times, peeling. For example, if you keep removing nodes of degree 1, until the graph stops changing, you'll actually remove not only fuzz of nodes of degree 1, but also all "thin tendrils" made of chains of nodes of degree 2. And so on. Which also means that this method is different than simple **degree centrality**: it's a combination of having a relatively high degree, and being connected to nodes of higher degree.

> This is similar to image processing algorithm of incremental erosion that is used for de-noising. When lonely pixels are incrementally removed.

They found that successful cascades are more likely to be started by nodes with higher k-core values. What they mean by that is that if you look at the total size of a cascade started by any given node, then nodes in higher-cores would have larger cascades.

# When A and B are not mutually exlusive

Now let's get to the decision-based model, and consider that in many cases users (nodes) can accept **both** behaviors A and B (say, buy two devices), but at an additional cost. To model that we can modify the payout matrix:
* A+A→a
* B+B→b
* AB+AB→max(a,b)
* But sticking to 2 strategies also comes with a cost -c (applied as a fixed node-based cost at each simulation step, but not per edge)

Example: a=3, b=2, c=1, and the original configuration is AABBB. What happens? Right now middle B node is getting 0 from the left, and 2 from the right. But if it switches to AB, it starts to get 3 from the left (from A), but has to pay a cost of 1, so the net improvement is 2. So it switches. But It brings it to a tie with just B (a-c=b), which means that A stops spreading.

If we however make a=5, b=3, c=1, and the same starting point of AABBB then accepting A on top of B becomes beneficial for all further B people. But they won't drop B, as the benefit (c=1 = disappearance of -1) doesn't outweigh the costs (disappearance of b=3).

In a general case, still for a 1D chain , it's a 2-parameter model (a,c), as everything is relative, so we can fix b=1. First, what will happen if an "empty" node X is flanked by two standards: AAXBB? a>1=b is one decision (of switching to A vs B). a+b-c=a+1-c>a is another decision, of switching to AB from pure A. And finally a+1-c=1 is a decision of switching to AB from pure B. If we draw all that, we get 3 sections on the (a,c) plane (two wedges and an infinite square), where high-A low-C options lead to AB, low-A, high-C options lead to B, and high-A high-C values lead to A.

Then, will the AB border be moving on a chain? Consider AB-X-B. A similar solution, but shifted, as X would prefer AB to B (switch B→AB) if the cost of switching (+a-c) would match losing b (-1), So not not a=c as for B→A switch before, but a=c+1.

If we join both pictures, we see that for a chain (not even a graph yet!), there are 4 different solutions: only A, only B, some AB on the border, an AB winning over B, but not killing B (B switch to AB, but don't drop B).

Interestingly, one marketing consequence is that low cost of adoption is good for penetrating the market, but high cost of adoption may be good for forcing a choice. _Really?_

Now, this was for a 

# Refs

Lecture summary:
https://snap-stanford.github.io/cs224w-notes/network-methods/network-effects-and-cascading-behavior

Lecture video:
https://www.youtube.com/watch?v=_iGi9EC5ZXE&list=PL1OaWjIc3zJ4xhom40qFY5jkZfyO5EDOZ&index=9