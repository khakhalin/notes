# Neocortical plasticity: an unsupervised cake but no free lunch
Eilif B. Muller 

#review #neuro

https://openreview.net/pdf?id=S1g_N7FIUS 

mini-Review / opinion

When training a network to do something good, how can we train it to *also* not do something bad? (They call it a "Monkey's Paw effect" after some horrid story from the 1910). One option could be to explicitly tell the model what is bad, and try to exhaust bad cases (they call it: "inject adversarial examples into learning data stream", and "specifying the negative space of the objective"). But that's bad.

Instead, try to "disentangle the underlying sources of variation" in an unsupervised manner (Bengio 2012), which amounts to "learning a good #representation".

How can it happen in the cortex? NMDA spikes, clusters of co-coding synapses + cliques of neurons. Inhibition puts a limit the length (and thus spatial propagation) of NMDA spikes, and so defines the size of cliques. It also suppresses activation of competing cliques.

Then quote LeCun, from NIPS 2016 talk apparently:

 “If intelligence is a cake, the bulk of the cake is unsupervised learning, the icing on the cake is supervised learning, and the cherry on the cake is reinforcement learning.” 