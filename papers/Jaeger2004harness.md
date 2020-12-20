# Harnessing nonlinearity

Jaeger, H., & Haas, H. (2004). Harnessing nonlinearity: Predicting chaotic systems and saving energy in wireless communication. Science, 304(5667), 78-80.
https://pdfs.semanticscholar.org/8922/17bb82c11e6e2263178ed20ac23db6279c7a.pdf
plus online materials:
https://science.sciencemag.org/content/sci/suppl/2004/04/01/304.5667.78.DC1/Jaeger_SOM.pdf

#rnn #echo #chaos #complexity

Parents: [[07_RNNs]] / [[echo_state]]
See also: [[complexity]], [[chaos]], [[mackey-glass]]

# Basics

Original communication about reservoir computing (aka **Echo State Network**). 2000+ citations.

A randomly connected network (~1000 nodes, which was a lot back then, with most RNNs having 5-30 neurons) that propagates signals (aka **reservoir**). Sparse connectivity (~1% of all connections), random weights (uniform from −1 to 1). tanh activation function.

One output node gets inputs from many (in this case, all) nodes in the network, but also feeds back into the reservoir, to some randomly assigned "input nodes" within the network. This creates random "echoes" within the network, and these will be used in the future as a "library of signals", or "filters", to build a complex prediction from them.

During training, the output node is fed the teaching signal, and output layer is trained. As only one layer is taught, essentially we have a simple linear regression. Note that as it takes some time to "load" the network with activity, the training should start not right away, but after some time (in the order of N steps in, where N is the number of neurons in the network).

If after training, instead of feeding the training signal, we make the output neuron feed back into the reservoir, the network will continue generating a sequence on its own, based on the patterns it learned. It can look forward by ~N, or, in this paper, even ~2.4N steps.

As a complex process to predict, they used **the Mackey-Glass system**, a one variable ODE with chaotic behavior ([[mackey-glass]]), with standard parameters and τ=17. And then later, also some practical task about "equalization of a wireless communication channel" (I didn't quite understood what it means).

Mention that "liquid state networks" are very similar (Maas 2002; Maas 2003).

# Details (from Supporting Materials)

Not every randomly generated reservoir can support a process like that. They call it "damping properties": the spectral radius should be smaller than 1. In practice, they generated a random network, then rescaled it to spectral radius of 0.8.

To make sure the network is really chaotic, they let it run in generation mode for 317 steps after the "standard" generation sequence; then perturbed the very last state of the standard run (one time point) with normal noise. Looked at the exponential divergence rate [[lyapunov-exp]]. Averaged over 100 trials, got λ~0.0058, which is very close to λ for the original signal.

In some runs they also tried to add noise to the network, and it helped during learning for the "standard training", but not for the "advanced training method" (below).

Ensemble method also helped (training 20 different models and averaging their outputs). And because you feedback back the averaged output, in each of the networks, not its own output, it also presumably improves the prediction horizon (not just the accuracy).

Also applied same method to Lorenz attractor (see [[lorenz]]). Similar results. Quickly mention that use of Lorentz attractor as a benchmark, as everyone do it slightly differently, making comparisons hard.

## Advanced training method

The idea is to refine network performance by switching to partial self-generation at some point, as this signal is easier to learn. So at first you force-feed it with ground-truth output. But at some point, once it had learned it, generate a full set of one-step predictions from the ground-truth, and then feed this signal in instead of ground truth. As this signal is more "compatible" with the inner workings of this particular network, it will make self-generation more stable.

> Isn't it essentially like generating 2 steps ahead, instead of 1 step ahead? Because if yes, it can be generalized further, by gradually learning to predict k steps ahead…

They say however that trying to predict further in didn't consistently improve prediction. Sometimes did, sometimes didn't. 

> The way it's explained in the paper, it's curiously similar to **noisy student** type of self-supervised learning, just for this network in particular. See: [[08_Self_Supervised]].

Back then they did it all in Matlab :)

# Refs

https://en.wikipedia.org/wiki/Mackey-Glass_equations

Maass, W., Natschläger, T., & Markram, H. (2002). Real-time computing without stable states: A new framework for neural computation based on perturbations. Neural computation, 14(11), 2531-2560.

Maass, W., Natschläger, T., & Markram, H. (2004). Computational models for generic cortical microcircuits. Computational neuroscience: A comprehensive approach, 18, 575-605.
