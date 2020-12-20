# STDP: Spike-Time-Dependent Plasticity

#bib #neuro #synfire

Parent: [[12_Neuro]]
Related: [[echo_state]]


Names to look for:
* Stephen Grossberg (Boston University Cognitive & Neural Systems)

Dean, P., & Porrill, J. (2014). Decorrelation learning in the cerebellum: computational analysis and experimental questions. In Progress in brain research (Vol. 210, pp. 157-192). Elsevier.

Linster, C., & Cleland, T. A. (2010). Decorrelation of odor representations via spike timing-dependent plasticity. Frontiers in computational neuroscience, 4, 157.
https://www.frontiersin.org/articles/10.3389/fncom.2010.00157/full

Ilan, L. B., Gidon, A., & Segev, I. (2011). Interregional synaptic competition in neurons with multiple STDP-inducing signals. Journal of neurophysiology.
https://journals.physiology.org/doi/pdf/10.1152/jn.00612.2010
Some model of cortical neurons. If I got it right, it proposes 2 types of STDP co-existing, and the use of backpropagating action potentials to somehow introduce synaptic competition.

Gilson, M., Burkitt, A., & Van Hemmen, L. J. (2010). STDP in recurrent neuronal networks. Frontiers in computational neuroscience, 4, 23.
https://www.frontiersin.org/articles/10.3389/fncom.2010.00023/full

Soltoggio, A., & Stanley, K. O. (2012). [From modulated Hebbian plasticity to simple behavior learning through noise and weight saturation](https://dspace.lboro.ac.uk/dspace-jspui/bitstream/2134/16988/1/soltoggioStanley2012NNs.pdf). Neural Networks, 34, 28-41.
On calculations in a saturated network (weights driven to max through positive feedback, as in our case; can be refed here). Stability. Their model shows some bistable switching between excitation and inhibition that is irrelevant for my case. Use anti-hebbian phases and noise to allow network to reconfigure.

Florian, R. V. (2007). Reinforcement learning through modulation of spike-timing-dependent synaptic plasticity. Neural Computation, 19(6), 1468-1502.

Frémaux, N., Sprekeler, H., & Gerstner, W. (2010). Functional requirements for reward-modulated spike-timing-dependent plasticity. Journal of Neuroscience, 30(40), 13326-13337.

Legenstein, R., Chase, S. M., Schwartz, A. B., & Maass, W. (2010). A reward-modulated hebbian learning rule can explain experimentally observed network reorganization in a brain control task. Journal of Neuroscience, 30(25), 8400-8410.

Pfeiffer, M., Nessler, B., Douglas, R. J., & Maass, W. (2010). Reward-modulated hebbian learning of decision making. Neural Computation, 22(6), 1399-1444.

Porr, B., & Wörgötter, F. (2007). Learning with “relevance”: using a third factor to stabilize hebbian learning. Neural Computation, 19(10), 2694-2719.

Izhikevich, E. M. (2007). Solving the distal reward problem through linkage of STDP and dopamine signaling. Cerebral cortex, 17(10), 2443-2452.

# Biology of it

Shulz, D. E., & Feldman, D. E. (2013). Spike timing-dependent plasticity. Rubenstein JLR, Rakic P. Comprehensive developmental neuroscience: neural circuit development and function in the healthy and diseased brain. San Diego, CA: Elsevier, 155-81.
(A chapter in a book. No good computational chapters, but lots of biology. Mostly in-vitro though.)

# Refs

Nice summary:
http://www.yger.net/spike-timing/
Quotation (links are not available, so would have to be reconstructed): According to bibcite key=Toyoizumi2007, Chechik2003, STDP could be seen as an attempt, by the neurons, to maximize the transmission of information and therefore the mutual information between inputs and outputs. For bibcite key=Bohte2005b, STDP is more a way to reduce the variability of the output knowing the input:  (with  being the entropy). We can cite other examples such as slow feature analysis bibcite key=Sprekeler2006, where STDP aims to decompose the signals into a basis of signals, slowly varying in time, or the predictive coding bibcite key=Rao2001 theory, where STDP is used to encode only time differences. 