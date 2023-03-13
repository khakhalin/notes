# Network structure effects in reservoir computers

Carroll, T. L., & Pecora, L. M. (2019). Network structure effects in reservoir computers. Chaos: An Interdisciplinary Journal of Nonlinear Science, 29(8), 083130.
https://arxiv.org/pdf/1903.12487.pdf

#echo

Parents: [[09_Graphs]], [[07_RNNs]] / [[echo]]
Related: [[chaos]], [[Carroll2020chaos]] (next paper)

Consider a reservoir of 100 neurons with weights in {0, 1}, then start flipping some of +1 edges to −1, thus altering the network. Networks were random, except that they made sure they were strongly connected (there's a path between any 2 nodes). No loops (diagonal edges).

For inputs, they seem to be feeding to all nodes, with weight +1 for odd nodes, and −1 for even. This part wasn't ever changed.

Node activation functions ([[activation_functions]]) are either polynomial (ODE that defines dr/dt through a cubic polynomial of r + inputs), or some custom sigmoid-like (leaky tanh). They say that trying two different functions helps to generalize, but also it appears to resemble some mysterious analog system they are actually thinking about (both authors are from a military naval research institute, huh?).

Leaky tanh:
$r ← αr + (1-α)\text{tanh}(Ar + \text{Input} +1)$
which is apparently according to the patent from Jaeger.

> Interesting precedent for activation functions. Should I, like use softplus - just because?

Lorentz system for a signal ([[lorenz]]). For training, discarded first 2000 points (so ×20 N, which is a lot, huh?). No gradient descent, but explicitly (Moore-Penrose pseudo-inverse) of the out matrix. L2 regularization ([[ridge_regression]]).

Everything done in Matlab.

## Main analysis (connectivity)

Looked into **symmetries** of the network, as apparently there's a fast algorithm for that (Stein 2013, but also [[graph_symmetry]]). It's hard to figure out what they did exactly though, as they don't provide enough details to reproduce the calculations. (Do they even test ordered or unordered graphs? It sounds from the text that it should be on real, ordered graphs, but it's not quite clear...)

Analytically show that symmetries are bad, as they make the network poorer (symmetrical parts may just copy each other). **Cluster synchronization**. Even if symmetric subnetworks are not exactly identical, it would lead to rows of data in the echo library (they call it Ω) that are similar to each other, which is bad. They then just looked at the rank of covariate matrix ΩᵀΩ, which is equal to the number of uncorrelated vectors in Ω, except that they used approximate rank (the number of eigenvalues above a certain threshold).

Set 9800/10000 = 98% of edges to 1 (the rest are 0). Then flip anything between 0 and 50% of 1-edges to −1. The testing error actually decreases from almost 0.1 to about 0.01. Inhibition is important, huh! In terms of inputs, an equal mix of 1 and −1 seems preferable (_as it indirectly enables inhibition, right?_) Finaly, they show that if you only make 20% of edges non-zero, and use a uniform distribution for weights (a "classic" echo state machine), testing error stays flat. All in all, while in the intro they claim that they took a baseline network, and then went further and further from it, in practice they started with a dysfunctional network (no inhibition), and by "going further from it", actually just moved towards a more reasonable solution. That's all, no?

> The choice of scenarios is strange, especially this last one with uniform edges. I guess it might have been  requested by the reviewer, as it totally doesn't fit the story, and is poorly explained. But that's also how people usually make echo state machines, so maybe they really just wanted to provide a baseline, idk.

When they look at the number of symmetries, the more symmetric the network (the higher the number of symmetries), the higher is the testing error for the best signal it generates (a straight line in log-log coordinates). This is true for both types of nodes, and several types of signals, even though actual shapes and values are slightly different.

Flipping edges reduced symmetries (even if a graph is symmetric, a +edge and a −edge are no longer interchangeable), and after lots of flips (~100) symmetries disappear. Similarly, flips increased the rank of the echo matrix Ω. Which may be another way to explain why rando-flipped networks are better in approximating signals. The training error steadily went down; the testing error went down, then increased once again, then again went down. They are saying that maybe there's a range of flips (~30-40% of edges) where "the reservoir is not stable" (more sensitive to small changes in the input signal).

> They don't explore it here beyond that, and don't explain why it would be the case, but see next paper [[Carroll2020chaos]] show that actually working at the edge of chaos decreases computational capacity, as the system becomes unstable, and chaotically diverges.

> They also show that linear nodes don't work, but that's obvious, isn't it? I'm not sure why they spent a whole page on it.

Memory capacity also increases with the share of negative weights. They give a formula for memory capacity (citing Jaeger 2002), and it's not obvious; also they have to use a weird absolutely random signal, and they throw some shade on this calculation, saying that they don't like it, but it's required in the field.

> I'm not convinced that it's a good variable at all. Something like degradation of the prediction with prediction horizon for a particular signal would be more valuable, I think.

Finally, they show that having about 50% of weights negative (they call it "flipped") is much more important than having the connectivity matrix sparse. Because it's a better way of achieving asymmetric matrices. Going from 80% dense network with 20% of negative edges to 30% of negative edges has the same effect as going from 80% density to 40% density, while retaining 20% edges. Still sparse AND diverse in terms of weight signs (50%) is best.

> Which suggests that maybe inhibition, the way it works in the brain, having distinct role and architecture, is not always a feature, but often a bug. Maybe it would have been much better to have all neurons capable of making both inhibitory and excitatory synapses; it's just that it is biologically hard. So maybe nature went for a less efficient, simplified solution, where excitation and inhibition are separate, but artificial networks don't have to follow their example?

> And they totally didn't explore how to distribute weight signs in a sparse network. They just show that 50% is optimal, on average, but are some distributions better than others? Probably, right?

## Random notes

Starts with a brief review of applications, and weird analog systems that can be used as a physical substrate for reservoir computing (photonics and what not).

# Refs

Pseudo-inverse:
https://en.wikipedia.org/wiki/Moore%E2%80%93Penrose_inverse

The project they cite as a fast algorithm for detecting all symmetries of a graph.
Technically the citation is:
W. Stein, SAGE: Software for Algebra and Geometry Experimentation (http://www.sagemath.org/sage/,
http://sage.scipy.org/, 2013).
But this paper doesn't exist. The software is updated regularly, so can be cited with any year. There's also a paper with a similar name, but it's more of a release note, and it's from 2005.
Stein, W., & Joyner, D. (2005). Sage: System for algebra and geometry experimentation. Acm Sigsam Bulletin, 39(2), 61-64.
https://www.sagemath.org/files/sage_stein2005.pdf
http://www.sagemath.org/sage/
http://sage.scipy.org/
See [[graph_symmetry]] for some more practical details of how to actually find graph symmetries.

For most original definitions, they cite this:
H. Jaeger, Technical report GMD-Forschungszentrum Informationstechnik (2002).