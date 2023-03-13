# Visualizing a  future of neuro and engineering 2021

Zenke, F., Bohté, S. M., Clopath, C., Comşa, I. M., Göltz, J., Maass, W., ... & Goodman, D. F. (2021). Visualizing a joint future of neuroscience and neuromorphic engineering. Neuron, 109(4), 571-575. https://igi-web.tugraz.at/PDF/258.pdf

Parents: [[neuromorphic]], [[deepneuro]]
See also: [[Richards2019deepneuro]]

#neuromorphic #review


Collective opinion piece / perspective / conference white paper for a focus meeting named "**‘Spiking neural
networks as universal function approximators**". Primarily European researchers.

Start with a contrast between biologically realistic models (that are super-hard to model, but are also totally not useful), and ANNs (that are useful, for the first time ever, but not really brain-like). Nicely, use **high-dim nonlinear transformations** as the most basic way to describe what's happening in the brain (_isn't it the conceptual shift we all were waiting for so long? When this becomes the default language?_) However, at the implementation levels ANNs rely on the flow of error gradients, and it doesn't work in spiking networks (but see [[Richards2019deepneuro]] for possibilities to cop-out).

Novel avenues / trends:
* Temporal spiking dynamics (_like, as in not just rate-code?_)
* Computational value of neuronal heterogeneity
* Biologically plausible learning rules that still approximate gradient-based framework (_And that's valuable not because of biological realism per se, but because biological rules are local, right?_)

> Incidentally, it also means a radical embracing of spiking networks, as opposed to "half-there" gradient-based networks. Which is interesting philosophically, as arguably spikes exist before networks, and they a discrete not necessarily because they are the best way to compute things, but because they (active processes, dynamic-system-based propagation) are the best way to communicate quickly at long distances, for biological hardware. Which is not the case for silicon-based on scifi optic systems. But on the other hand, they may also happen to be optimal, who knows. Maybe action potentials were a mathematical serendipity of sorts?

What allows this progress?
* Precisely timed spikes can communicate / approximate gradients (Neftci 2019, Göltz 2019, Kheradpisheh 2020)
    * But also it's possible to work with instantaneous spike-rates (Nicola 2017) _kinda as a back-up mental model?_
* We can harness neuronal heterogeneity and tune things like time constants together with connectivity (Yin 2020).

Götz and Kheradpisheh solutions rely on extremely sparse precisely time coding though (single spikes), so that's a bit of a problem.

Dunk on [[bptt]] (backprop through time) (_which is great, as it's extremely unrealistic, right?_). Instead, we offer some real-time recurrent learning algorithms (Zenke 2021, Bellec 2020). Where would learning signals come from? In Bellec2020 they have a sub-network learn and generate these signals (_so [[gan]]-like dynamics here? Or what?_)

Plans for the future; what do we need?
* Learning to draw parallels between networks (including realistic and classic ANN networks)
* Learn from biology:
    * cell diversity,
    * connectivity constraints,
    * circuit motifs (_pre-trained architectures, essentially?_)
* unsupervised learning in bio-like networks (all these works above are supervised)

# Refs

* [[Neftci2019surrogate]] - Surrogate gradient learning through time-spiking:

Learning with precisely timed single spikes:
* Göltz, J., Kriener, L., Baumbach, A., Billaudelle, S., Breitwieser, O., Cramer, B., ... & Petrovici, M. A. (2019). Fast and deep: energy-efficient neuromorphic learning with first-spike times. arXiv preprint arXiv:1912.11443. https://arxiv.org/abs/1912.11443
* Kheradpisheh, S. R., & Masquelier, T. (2020). Temporal backpropagation for spiking neural networks with one spike per neuron. International Journal of Neural Systems, 30(06), 2050027. https://arxiv.org/pdf/1910.09495.pdf

Learning with instant spike-rates:
* Nicola, W., & Clopath, C. (2017). Supervised learning in spiking neural networks with FORCE training. Nature communications, 8(1), 1-15. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5738356/

Practical brain-inspired learning algorithms:
* Bellec, G., Scherr, F., Subramoney, A., Hajek, E., Salaj, D., Legenstein, R., & Maass, W. (2020). A solution to the learning dilemma for recurrent networks of spiking neurons. Nature communications, 11(1), 1-15. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7367848/
* Zenke, F., & Neftci, E. O. (2021). Brain-inspired learning on neuromorphic substrates. Proceedings of the IEEE. https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=9317744

Harnessing neuronal variability, and tuning non-synaptic neuronal properties:
* Yin, B., Corradi, F., & Bohté, S. M. (2020, July). Effective and efficient computation with multiple-timescale spiking recurrent neural networks. In International Conference on Neuromorphic Systems 2020 (pp. 1-8). https://arxiv.org/pdf/2005.11633.pdf