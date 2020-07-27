# Cyclic transitions between higher order motifs underlie sustained activity in asynchronous sparse recurrent networks

Bojanek, K., Zhu, Y., & MacLean, J. (2019). Cyclic transitions between higher order motifs underlie sustained activity in asynchronous sparse recurrent networks. bioRxiv, 777219.
https://www.biorxiv.org/content/10.1101/777219v1.article-info

Updated version tbc (plos?)

#networks #neuro #spiking #motifs #criticality #synchrony #graph

Related: [[graph_properties]]

Is it possible to predict whether a spiking network with a given topology would spontaneously halt (truncate), or become persistently active, after being activated with a kick to a few newurons? Can we guess it, either from simple network properties, or early on, from the dynamics of network activation?

> Something like a halting problem for directed graphs :)

While simple properties, such as network density, define the region at which persistent activity is possible, there is lots of variability in activation stability between different instantiations of a random network. Moreover, even for networks that can support persistent activity, a reasonable change in the starting conditions may lead to a truncation. 

Even more curiously, truncation is not always easy to predict from raw observations of activity, as the network may seem to be quite active, before suddenly coming to an unexpected halt. On the other hand, truncation can be predicted from slightly more advanced statistics of network activity, and perhaps in somewhat unexpected ways: **synchronous networks are more prone to truncation**, compared to asynchronous networks. (_It was not quite obvious to me, but apparently there's some literature about it, like Gutkin 2001._)

# Motif analysis

A critical part of this paper is that they generated some connectivity networks, but then ran activity on them, and looked at **activation graphs**, or something like that. So not what motifs were present in the underlying skeleton, but what motifs were actually activated by the network. That's kinda unusual, isn't it?

To quantify that, they looked at the comparison of three **motif-bound clustering coefficients**, aka **motif propensity** or **triplet clustering coefficients**. For example, when they write that in-fan motifs were more pravalent than out-fan motifs, it is (_I think?_) equivalent to a statement that nodes with high in-degree have higher clustering coefficient. 

Or, more precisely, but much less neatly, in this example, if two edges were often activated in such a way that they converged on the same node (aka in-fan), their backs were more likely to be connected and activated consecutively (triangle motif, or clustering coefficient), compared to a different pair of edges that started at one node, and diverged activation to two different nodes (fan-out).

It seems that stable networks tend to have a balance of 3 types of non-cyclical triangle motifs (fan-in, fan-out, and middle-man), and that these **types of motifs are often activated in succession** (perhaps as an epiphenomenon of network modularity, with activity fanning from smaller clusters to larger clusters, before returning back to smaller ones). If this quasi-cyclicity is absent, the networks are unstable. Moreover, if you go into a 3D space of activations for these 3 types of motifs, and look at trajectories, they mostly fall in a 2D plane lying at an angle in this 3D space. If you loook at the trajectores in this 2D plane, **persistent networks were nicely quasi-cyclical** (chaotic, but constrained), while networks that eventually halted, first did all sorts of extreme stuff, with wild variations of activity, and then just got ejected away from the attractor, towards nothingness.

In activation graphs, fan-in seem to be particularly overrepresented. 3-cyclical graphs are an order of magnitude more rare.

The statement about motif cycling was quantitatively replicated on random Erdos-Renyi graphs.

> An interesting question then: can you fix a halting (truncating) network? Like, what is the minimal number of edges to add (or remove?) from a halting network to make it persistent? And how to find them? Is there a good heuristic?

> It seems that in this particular paper, they rewired networks randomly (not sure if from scratch every time, or continuously with continuous monitoring), but there was no genetic alrogirhm, and there was no target rewiring. Which may indicate an opportunity.

# Model

Leaky integrate and fire (AdEx), 4000 excitatory + 1000 inhibitory neurons, conductance-based synapses (mean 1.13 nS for excitatory, ~ 10 times stronger for inhibitory), synaptic strength comes from a heavy-tailed log-normal distribution. Resting ≈ -70 mV, spike threshold ≈-50 mV, Cm ≈ 280 pF.

> So excitatory reversal was 0, so excitatory drive of 70e-3 V, giving the current of ~1e-9 * 7e-2 ≈ 1e-10 A. Prob acting for a few ms, (say, 10 ms, as their excitatory time const is 10 ms). It gives 1e-10 * 10e-6 = 1e-15 Q. With Cm of 3e-12 and ∆V=20mV=2e-5, we need CV=6e-17 to spike. Right? So does it mean that 1 incoming spike was enough to make another neuron spike? Looks like that.

Connectivity probabilities that they lifted from previous literature: Pee = 0.2, Pei = 0.35, Pie = 0.25, Pii = 0.3 (here "ee" stands for excitation-excitation, etc.). But then they set Pee = 0.2, Pii = 0.3, and used computational experiments to find a region where network was **running the longest, but with lowest spiking rate** (kinda "threshold area", in a way), which happened at Pei = 0.22 and Pie = 0.31. So close to previous studies, but slightly different. Probably if one were to try another model, this would have to be recalibrated, as it obviously depends on time constants for excitation and inhibition, but at the very least, it's a good starting point.

**Model creation:** first set 50 random clusters, then assigned each excitatory neuron to 2 different pre-defined clusters simultaneously. Neurons were 2 times more likely to connect within their clusters, then outside of them. Inhibitory connections were fully random.	

Then selected "good" networks: they used a proxy for synchrony: var_t( mean_n(V) )/mean_n( var_t(V) ), where V are voltages for each neuron, variance is taken over time, and averaging happens across all neurons. In other words, do neurons variate together (which would make variation of toal signal ~ variation of individual neurons), or do they sub for each other (that would bring var(mean(V)) to 0). A value of 0.5 as an arbitrary threshold for "asynchrony".

And then, it seems they rewired networks randomly, keeping the density of connections as constant as possible (but it seems, still a narrow range, so not exactly constant), until the networks would become low-spiking and low-synchrony (aka near-critical).

# Top references

Zylberberg J, Pouget A, Latham PE, Shea-Brown E. Robust information
propagation through noisy neural circuits. PLoS Comput Biol. 2017;13(4):
e1005497. doi: 10.1371/journal.pcbi.1005497.

Barzel B, Barabasi AL. Universality in network dynamics. Nature Physics.
2013;9: 673–681. doi: 10.1038/nphys2741.

van Vreeswijk C, Sompolinsky H. Chaos in neuronal networks with balanced
excitatory and inhibitory activity. Science. 1996;274: 1724 –1726. doi:
10.1126/science.274.5293.1724.

Vogels TP, Abbott LF. Signal propagation and logic gating in networks of
integrate-and-fire neurons. J Neurosci. 2005;25(46): 10786-10795. doi:
10.1523/jneurosci.3508-05.2005.

Ganmor E, Segev R, Schneidman E. A thesaurus for a neural population code.
eLife. 2015;4: e06134. doi: 10.7554/eLife.06134.

Jovanovic S, Rotter S. Interplay between graph topology and correlations of third
order in spiking neuronal networks. PLoS Comput Biol. 2016;12(6): e1004963.
doi: 10.1371/journal.pcbi.1004963.

# References

When talking about quantification of network branching (a measure of critiquality): Beggs JM, Plenz D. Neuronal avalanches in neocortical circuits. J Neurosci. 2003;23(35): 11167-11177. doi: 10.1523/JNEUROSCI.23-35-11167.2003.

Previous studies that showed the interplay between asynchrony and persistent activity (?): Gutkin, B. S., Laing, C. R., Colby, C. L., Chow, C. C., & Ermentrout, G. B. (2001). Turning on and off with excitation: the role of spike-timing asynchrony and synchrony in sustained neural activity. Journal of computational neuroscience, 11(2), 121-134.

Perin R, Berger TK, Markram H. A synaptic organizing principle for cortical
neuronal groups. PNAS. 2011;108(13): 5419-5424. doi: 10.1073/pnas.1016051108.

Song S, Sjostrom PJ, Reigl M, Nelson S, Chklovskii DB. Highly nonrandom
features of synaptic connectivity in local cortical circuits. PLoS Biol. 2005;3(3):
e68. doi: 10.1371/journal.pbio.0030068.

Seeman SC, Campagnola L, Davoudian PA, Hoggarth A, Hage TA, Bosma-Moody
A, et al. Sparse recurrent excitatory connectivity in the microcircuit of the adult
mouse and human cortex. eLife. 2018;7: e37349. doi: 10.7554/eLife.37349.

Teramae J-N, Tsubo Y, Fukai T. Optimal spike-based communication in
excitable networks with strong-sparse and weak-dense links. Scientific Reports.
2012;2: 485. doi: 10.1038/srep00485.

Litwin-Kumar A, Doiron B. Slow dynamics and high variability in balanced
cortical networks with clustered connections. Nat Neurosci. 2012;15: 1498–1505.
doi: 10.1038/nn.3220.

Brunel N. Dynamics of sparsely connected networks of excitatory and inhibitory
spiking neurons. J Comp Neurosci. 2000;8: 183–208. doi:
10.1023/A:1008925309027.

Beggs JM, Plenz D. Neuronal avalanches in neocortical circuits. J Neurosci.
2003;23(35): 11167-11177. doi: 10.1523/JNEUROSCI.23-35-11167.2003.

Karimipanah Y, Ma Z, Wessel R. New hallmarks of criticality in recurrent neural
networks. arXiv:1610.01217v2 [q-bio.NC] [Preprint]. 2016. [cited 2019 Sept 12].
Available from: https://arxiv.org/pdf/1610.01217.pdf.

Haldeman C, Beggs JM. Critical branching captures activity in living neural
networks and maximizes the number of metastable states. Phys Rev Lett.
2005;95: 058101. doi: 10.1103/PhysRevLett.94.058101.

Bertschinger N, Natschl¨ager T. Real-time computation at the edge of chaos in
recurrent neural networks. Neural Comput. 2004;16(7): 1413-1436. doi:
10.1162/089976604323057443.

Kriener B, Enger H, Tetzlaff T, Plesser HE, Gewaltig MO, Einevoll GT.
Dynamics of self-sustained asynchronous-irregular activity in random networks of
spiking neurons with strong synapses. Front Comput Neurosci. 2014;8: 136. doi:
https://doi.org/10.3389/fncom.2014.00136.

Pajevic S, Plenz D. Efficient network reconstruction from dynamical cascades
identifies small-world topology of neuronal avalanches. PLoS Comput Biol.
2009;5(1): e1000271. doi: 10.1371/journal.pcbi.1000271.

Orlandi JG, Soriano J, Alvarez-Lacalle E, Teller S, Casademunt J. Noise focusing
and the emergence of coherent activity in neuronal cultures. Nature Physics.
2013;9: 582-590. doi: 10.1038/nphys2686.

Shimono M, Beggs JM. Functional clusters, hubs, and communities in the
cortical microconnectome. Cereb Cortex. 2015;25(10): 3743–3757. doi:
10.1093/cercor/bhu252.

Nigam S, Shimono M, Ito S, Yeh F-C, Timme N, Myroshnychenko M et al.
Rich-club organization in effective connectivity among cortical neurons. J
Neurosci. 2016;36(3): 670-684. doi: 10.1523/jneurosci.2177-15.2016.

Dechery JB, Maclean JN. Functional triplet motifs underlie accurate predictions
of single-trial responses in populations of tuned and untuned V1 neurons. PLoS
Comput Biol. 2018;14(5): e1006153. doi: 10.1371/journal.pcbi.1006153.

Chambers B, MacLean JN. Higher-order synaptic interactions coordinate
dynamics in recurrent networks. PLoS Comput Biol. 2016;12(8): e1005078. doi:
10.1371/journal.pcbi.1005078.

Chambers B, Levy M, Dechery JB, MacLean JN. Ensemble stacking mitigates
biases in inference of synaptic connectivity. Net Neurosci. 2018;2(1): 60-85. doi:
10.1162/netn a 00032.

Koulakov AA, Hromadka T, Zador AM. Correlated connectivity and the
distribution of firing rates in the neocortex. J Neurosci. 2009;29(12): 3685-3694.
doi: 10.1523/JNEUROSCI.4500-08.2009.

Ecker AS, Berens P, Keliris GA, Bethge M, Logothetis NK, Tolias AS.
Decorrelated neuronal firing in cortical microcircuits. Science. 2010;327: 584-587.
doi: 10.1126/science.1179867.

Renart A, de la Rocha J, Bartho P, Hollender L, Parga N, Reyes A, Harris KD.
The asynchronous state in cortical circuits. Science. 2010;327: 587-590. doi:
10.1126/science.1179850.

van Rossum MCW. A novel spike distance. Neural Comput. 2001;13(4): 751-763.
doi: 10.1162/089976601300014321.

Kumar A, Rotter S, Aertsen A. Conditions for propagating synchronous spiking
and asynchronous firing rates in a cortical network model. J Neurosci. 2008;20:
5268-5280. doi: 10.1523/JNEUROSCI.2542-07.2008.

Hlinka J, Hartman D, Paluˇs M. Small-world topology of functional connectivity
in randomly connected dynamical systems. Chaos Interdiscip J Nonlinear Sci.
2012;22: 33107. doi: 10.1063/1.4732541.

Muldoon SF, Bridgeford EW, Bassett DS. Small-world propensity and weighted
brain networks. Sci Rep. 2016;6: 22057. doi: 10.1038/srep22057.

Milgram S. The small-world program. Psychology Today. 1967;2: 60–67.

Hu Y, Brunton SL, Cain N, Mihalas S, Kutz JN, Shea-Brown E. Feedback
through graph motifs relates structure and function in complex networks. Phys
Rev E. 2018;98(6): 062312. doi: 10.1103/PhysRevE.98.062312.

Hu Y, Trousdale J, Josic K, Shea-Brown E. Motif statistics and spike correlations
in neuronal networks. J Stat Mech. 2013. Available from:
https://stacks.iop.org/JSTAT/2013/P03012. doi:
10.1088/1742-5468/2013/03/P03012.

Ocker GK, Hu Y, Buice MA, Doiron B, Josic K, Rosenbaum R, Shea-Brown E.
From the statistics of connectivity to the statistics of spike times in neuronal
networks. Curr Opin Neurobiol. 2017;46: 109-119. doi:
10.1016/j.conb.2017.07.011.

Curto C, Langdon C, Morrison K Robust motifs of threshold-linear networks
arXiv:1902.10270 [q-bio.NC] [Preprint]. 2019. [cited 2019 Dec 19]. Available from:
https://arxiv.org/pdf/1902.10270.pdf.
