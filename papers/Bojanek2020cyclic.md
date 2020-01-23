# Cyclic transitions between higher order motifs underlie sustained activity in asynchronous sparse recurrent networks
Bojanek, K., Zhu, Y., & MacLean, J. (2019). Cyclic transitions between higher order motifs underlie sustained activity in asynchronous sparse recurrent networks. bioRxiv, 777219.
https://www.biorxiv.org/content/10.1101/777219v1.article-info

Updated version tbc

#networks #neuro #spiking #motifs #criticality

Is it possible to predict whether a spiking network with a given topology would spontaneously halt (truncate), or become persistently active, after being activated with a kick to a few newurons? Can we guess it, either from simple network properties, or early on, from the dynamics of network activation?

> Something like a halting problem for directed graphs :)

While simple properties, such as network density, define the region at which persistent activity is possible, there is lots of variability in activation stability between different instantiations of a random network. Moreover, even for networks that can support persistent activity, a reasonable change in the starting conditions may lead to a truncation. 

Even more curiously, truncation is not always easy to predict, as the network may seem to be quite active, before suddenly coming to an unexpected halt. 

To some extent, truncation can be predicted from the statistics of network activity, but perhaps in somewhat unexpected ways: say, **synchronous networks are more prone to truncation**, compared to asynchronous networks. That's not what many people would have probably guessed.

# Motif analysis

A critical part of this paper is that they generated some connectivity networks, but then ran activity on them, and looked at **activation graphs**, or something like that. So not what motifs were present in the underlying skeleton, but what motifs were actually activated by the network. That's kinda unusual, isn't it?

To quantify that, they looked at the comparison of three **motif-bound clustering coefficients**, aka **motif propensity** or **triplet clustering coefficients**. For example, when they write that in-fan motifs were more pravalent than out-fan motifs, it is (_I think?_) equivalent to a statement that nodes with high in-degree have higher clustering coefficient. 

Or, more precisely, but much less neatly, in this example, if two edges were often activated in such a way that they converged on the same node (aka in-fan), their backs were more likely to be connected and activated consecutively (triangle motif, or clustering coefficient), compared to a different pair of edges that started at one node, and diverged activation to two different nodes (fan-out).

It seems that stable networks tend to have a balance of 3 types of non-cyclical triangle motifs (fan-in, fan-out, and middle-man), and that these **types of motifs are often activated in succession** (perhaps as an epiphenomenon of network modularity, with activity fanning from smaller clusters to larger clusters, before returning back to smaller ones). If this quasi-cyclicity is absent, the networks are unstable. Moreover, if you go into a 3D space of activations for these 3 types of motifs, and look at trajectories, they mostly fall in a 2D plane lying at an angle in this 3D space. If you loook at the trajectores in this 2D plane, **persistent networks were nicely quasi-cyclical** (chaotic, but constrained), while networks that eventually halted, first did all sorts of extreme stuff, with wild variations of activity, and then just got ejected away from the attractor, towards nothingness.

In activation graphs, fan-in seem to be particularly overrepresented. 3-cyclical graphs are an order of magnitude more rare.

The statement about motif cycling was quantitatively replicated on random Erdos-Renyi graphs.

> An interesting statement then: can you fix a halting (truncating) network? Like, what is the minimal number of edges to add (or remove?) from a halting network to make it persistent? And how to find them? Is there a good heuristic?

> It seems that in this particular paper, they rewired networks randomly (not sure if from scratch every time, or continuously with continuous monitoring), but there was no genetic alrogirhm, and ther was no target rewiring.

# Model

Leaky integrate and fire (AdEx), 4000 excitatory + 1000 inhibitory neurons, conductance-based synapses (mean 1.13 nS for excitatory, ~ 10 times stronger for inhibitory), synaptic strength comes from a heavy-tailed log-normal distribution. Resting ≈ -70 mV, spike threshold ≈-50 mV, Cm ≈ 280 pF.

> So excitatory reversal was 0, so excitatory drive of 70e-3 V, giving the current of ~1e-9 * 7e-2 ≈ 1e-10 A. Prob acting for a few ms, (say, 10 ms, as their excitatory time const is 10 ms). It gives 1e-10 * 10e-6 = 1e-15 Q. With Cm of 3e-12 and ∆V=20mV=2e-5, we need CV=6e-17 to spike. Right? So does it mean that 1 incoming spike was enough to make another neuron spike? Looks like that.

Connectivity probabilities that they lifted from previous literature: Pee = 0.2, Pei = 0.35, Pie = 0.25, Pii = 0.3 (here "ee" stands for excitation-excitation, etc.). But then they set Pee = 0.2, Pii = 0.3, and used computational experiments to find a region where network was **running the longest, but with lowest spiking rate** (kinda "threshold area", in a way), which happened at Pei = 0.22 and Pie = 0.31. So close to previous studies, but slightly different. Probably if one were to try another model, this would have to be recalibrated, as it obviously depends on time constants for excitation and inhibition, but at the very least, it's a good starting point.

**Model creation:** first set 50 random clusters, then assigned each excitatory neuron to 2 different pre-defined clusters simultaneously. Neurons were 2 times more likely to connect within their clusters, then outside of them. Inhibitory connections were fully random.	

Then selected "good" networks: they used a proxy for synchrony: var_t( mean_n(V) )/mean_n( var_t(V) ), where V are voltages for each neuron, variance is taken over time, and averaging happens across all neurons. In other words, do neurons variate together (which would make variation of toal signal ~ variation of individual neurons), or do they sub for each other (that would bring var(mean(V)) to 0). A value of 0.5 as an arbitrary threshold for "asynchrony".

And then, it seems they rewired networks randomly, keeping the density of connections as constant as possible (but it seems, still a narrow range, so not exactly constant), until the networks would become low-spiking and low-synchrony (aka near-critical).

# References

* When talking about quantification of network branching (a measure of critiquality): Beggs JM, Plenz D. Neuronal avalanches in neocortical circuits. J Neurosci. 2003;23(35): 11167-11177. doi: 10.1523/JNEUROSCI.23-35-11167.2003.