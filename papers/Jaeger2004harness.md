# Harnessing nonlinearity

Jaeger, H., & Haas, H. (2004). Harnessing nonlinearity: Predicting chaotic systems and saving energy in wireless communication. Science, 304(5667), 78-80.
https://pdfs.semanticscholar.org/8922/17bb82c11e6e2263178ed20ac23db6279c7a.pdf
plus online materials:
https://science.sciencemag.org/content/sci/suppl/2004/04/01/304.5667.78.DC1/Jaeger_SOM.pdf

#rnn #echo #chaos #complexity

See also: [[07_RNNs]] / [[echo_state]], [[complexity]], [[chaos]]


Original communication about reservoir computing (aka **Echo State Network**). 2000+ citations.

A randomly connected network (~1000 nodes, which was a lot back then, with most RNNs having 5-30 neurons) that propagates signals (aka **reservoir**). Sparse connectivity (~1% of all connections). One output node gets inputs from many (all?) nodes in the network, but also feeds back into the reservoir, to some randomly assigned "input nodes" within the network. This creates random "echoes" within the network, to be used in the future as a library to build from (filters).

During training, the output node is fed the teaching signal, and output layer is trained. As only one layer is taught, essentially we have a simple linear regression. Note that as it takes some time to "load" the network with activity, the training should start not right away, but after some time (in the order of N steps in, where N is the number of neurons in the network).

If after training, instead of feeding the training signal, we make the output neuron feed back into the reservoir, the network will continue generating a sequence on its own, based on the patterns it learned. It can look forward by ~N, or, in this paper, even ~2.4N steps.

As a complex process to predict, they used **the Mackey-Glass system**, a one variable ODE with chaotic behavior (see below). And then later, also some practical task about "equalization of a wireless communication channel" (I didn't quite understood what it means).

Not every randomly generated reservoir can support a process like that.

Mention that "liquid state networks" are very similar (Maas 2002; Maas 2003).

# Mackey-Glass

$\displaystyle \frac{dQ(t)}{dt} = \frac{βQ(t-τ)}{1+Q(t-τ)^n}-γQ(t)$ 

with parameter values like β=0.2, γ=0.1, n=10, and large τ (if you want it chaotic) of >20.

# Refs

https://en.wikipedia.org/wiki/Mackey-Glass_equations

Maass, W., Natschläger, T., & Markram, H. (2002). Real-time computing without stable states: A new framework for neural computation based on perturbations. Neural computation, 14(11), 2531-2560.

Maass, W., Natschläger, T., & Markram, H. (2004). Computational models for generic cortical microcircuits. Computational neuroscience: A comprehensive approach, 18, 575-605.
