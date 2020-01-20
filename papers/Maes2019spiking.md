# Learning spatiotemporal signals using a recurrent spiking network that discretizes time

Early citation: Maes, A., Barahona, M., & Clopath, C. (2019). Learning spatiotemporal signals using a recurrent spiking network that discretizes time. arXiv preprint arXiv:1907.08801.
https://arxiv.org/abs/1907.08801

Later published in PLoS Comp Bio. (no ref yet?)

#neuro #synfire #spiking

A big problem with spiking networks is how to get from ms time scales to behaviors that take  seconds / minutes. (refs for motor tasks). Introduce sequential and stochastic dynamics as two main modes of operation (refs). Many ML-like approaches are not biologically plausible: either backprop in time, or non-local information.

Two part curriculum training: first activate groups of neurons together at high freq to make them form clusters (cliques? they call it a "temporal backbone"), then activate them sequentially (slowly), making them learn a sequence. Tight connectivity => reverberation => change in the order of magnitude for time. They use:

* Random init on an Erdos graph + check (I suspect that manually and by eye; they don't tell it) that the activity is chaotic, rather than oscillatory
* leaky integrate-and-fire neurons with strong adaptation (important to slow recurrent activity down); 
* feedback inhibition (to prevent seizures); 
* mildly asymmetric STDP (no punishment for co-activation, so bidirectional connections are possible - I don't think it's an important detail though);
* weight normalization (L1 norm: sum weights = const)
* a read-out layer with simplified neurons and more Hebbian plasticity

Eigenvalue analysis of resulting connectivity matrix. Can learn non-Markov sequences (transversing the same set in different directions), presumably by encoding the same state as several "synonymous" clusters (making it Markov). Can also learn 2 sequences in parallel (again, diff clusters, diff outputs).

Sensitivity analysis: larger clusters make slower networks (for them, 40 neurons per cluster = 500 ms for sequential activity period; 10 neurons per cluster = 300 ms period). Still it's a weaker dependency than I would have expected! Moreover, while adaptation constants affect sequential activity period, the dependency is very weak! For adaptation times from 50 to 150 ms, the period stays practically flat. Also show that period replay variability is mostly proportional to period value, which is apparently expected for Markov diffusion (it's also very small: about 1% only).

For plasticity, they use several different low-pass filters (one for spiking, 2 for voltage), all tuned to enable reasonably realistic LTP and LTD, but from their sensitivity analyses, it doesn't actually look like these are too critical for the model to work.

Also learn a bird song, as a demo proof of principle.

In the discussion, a few paragraphs on the evidence for hierarchical learning of sequences.