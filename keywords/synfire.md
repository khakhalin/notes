# Synfire chains

#synfire #echo #stdo

Parents: [[12_Neuro]], [[07_RNNs]], [[network_neuro]]
Related: [[echo_state]], [[stdp]]

A type of process on graphs in neuroscience. Neurons that are connected together by their activity (possibly through [[stdp]]-type interactions), so now their connections encode this activity. If you activate it, you get a replay. If there's a resonance between a temporal input and the encoded activity - you have a strong response, and thus they act as a discriminator.

**Papers:**
* [[Maes2019spiking]] - Deiberately encoding activity (patterns) in connectivity
* [[Fiete2010stdp]] - stdp builds chains

Litwin-Kumar, A., & Doiron, B. (2014). Formation and maintenance of neuronal assemblies through synaptic plasticity. Nature communications, 5, 5319.
Model. Reverberation stabilizes learned architecture.  Homeostatic inhibitory plasticity with symmetric STDP (hat-shaped). Excitation had asymmetric STDP. Tuned recurrent excitation as a big part of stimulus selectivity. (Which may support the idea that synfire-chains resonate with inputs and feed-forward feed to "detector neurons")