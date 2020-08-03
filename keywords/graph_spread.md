# Cascading processes on graphs (diffusion, propagation, pandemics etc.)

#graphs #complexity

Parents: [[09_Graphs]]
Related: percolation

Examples: epidemics, diffusion of innovations, cascading failures in databases, spread of information, rumors, memes and viral marketing, fake news. Behaviors that cascade from node to node.

9:30 - until this point, all examples and memes

Desease propagation on a graph (along edges, from node to node) creates a **propagation tree**, or a **cascade**. 

**Decision-based models** (good for social effects): nodes make **local decisions** (e.g. about product adoption) based on the behavior of their neighbors. For example, you join if k of your friends join. Not probabilistic for now. 

The idea of following your friends comes from game theory: if each player can have two behaviors, A and B, and behaviors are synergistic, it's always beneficial to adopt whatever behavior that is popular with your friends. A good IRL example: VHS vs Betamax; as people wanted to exchange cassettes, it was beneficial for people to use whatever everyone else were using, so one standard won without any clear reasons for why it had to win (it wasn't better in any way).

**Payoff matrix**: AA→a (a>0), BB→b (b>0), AB or BA → 0. So as the node decides, it if has d friends, and p of them choose A, while 1-p chose B, the payoff is dpa if it goes A, and d(1-p)b if it goes B. What is the threshold? $dpa > d(1-p)b$ ⇒ if $p>b/(a+b)$ we go option A, else - option B.

Consider a scenario when everyone are using B. Now A showed up, and A is strictly better, but just a little bit (negligibly better, but better), so every node switches if 50% or more of its neighbors switches to A. Also, a small group of nodes uses A no matter what. What will happen? Depends on the topology, but it may propagate ghrough the network. Articulation nodes however may present a problem.

One interesting consequence: **the process is not reversible** (unless something changes): nodes go in one direction only, so you will never have fewer converted neighbors than you have currently, so the drive to switch may only increase or remain the same; never decrease.

27:44 - ONE POINT

**Example**: model protest recruitment on social networks; analysis of tags, actual study. González-Bailón, S., Borge-Holthoefer, J., Rivero, A., & Moreno, Y. (2011). The dynamics of protest recruitment through an online network. Scientific reports, 1, 197. - 500 citations. 

Austerity protests in Spain; recruitment happened on Twitter, so we deal with a graph of ~90k nodes (this part of a network) that exchanged ~0.5M messages. Back then collecting this kind of data was easier, as Twitter wasn't yet hiding (and selling) it. Directed graph, with an undirected core (strong network of mutuals).

Let's apply our decision-based model. For each node we can calculate:
* Activation time: moment when this particular user started tweeting protest messages
* k_in: the total number of friends they had at this moment
* k_a: the number of activated users among their friends at this omment
* De-facto activation threshold for this user will be $k_a / k_{in}$ (note that it's very case specific; no fake psychology can predict a universal value here). Threshold ~0 means that this particular node was an early adopter; ~1 means a conservative, slow node.

Actual distribution of activation thresholds in this study was almost uniform; a bit hill-shaped, with a drop-off for values close to 1 (very few people are that slow). Two spikes in the distribution: at near-0 (those who were already ready to start tweeting), and a strange narrow spike near 0.5. _Honestly, it does look like some numerical artifact; like one of those effects of small numbers. The spikes look a bit like a comb, so I think what happened is that people with a very small number of following were more abundant, and thus overrepresented in theis sample_.

Interesting effect: people got involved more readily if their neighbors experienced a burst of recruitment (if $Δk_a / k_a$ was large). _Here they show a strange plot of $Δk_a / k_a$ as a function of $k_a / k_{in}$ that somehow looks like a monotonous function, and I don't understand it. Really not explained in the lecture at all._

39:27 - smaller break

**How to identify cascades in the data?**

…

54:17 - Big break - end of Twitter story analysis and the decision model. Now: extending the model

# Refs

Lecture conspects:
https://snap-stanford.github.io/cs224w-notes/network-methods/network-effects-and-cascading-behavior

Lecture video:
https://www.youtube.com/watch?v=_iGi9EC5ZXE&list=PL1OaWjIc3zJ4xhom40qFY5jkZfyO5EDOZ&index=9