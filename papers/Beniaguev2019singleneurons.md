# Beniaguev2019deepneurons

David, Beniaguev, Segev Idan, and London Michael. "Single Cortical Neurons as Deep Artificial Neural Networks." bioRxiv (2019): 613141. https://www.biorxiv.org/content/10.1101/613141v1.full.pdf

Parents: [[deepneuro]] / [[dendritic_comp]]
Related: [[Jones2020neuromnist]]

#dendritic


**Gist:** to replicate a detailed biophysical model of a Layer 5 cortical pyramidal neuron with AMPA and GABA inputs, you need a 1-hidden-layer ANN. But if NMDA conductances are added, one needs 7 hidden layers (!). And moreover, one can use this strained ANN to better understand what a neuron "does".

Good (longer) intro into the history of the question.

They don't do a binary (as [[Jones2020neuromnist]]) or even rate-code approximation, but honestly look at a spike-train, using a **temporal convolution network**. 1 ms time bins, 1 for spike and 0 for no spike, 80 ms window, 100 inputs, 5000 total recording. Because of this type of spike representation, 2 nodes in the output layer: "silence" node, and "spike" node.

At first modeled a single integrate-and-fire neuron. Single hidden input is enough. Correctly learns that excitatory inputs are positive, and inhibitory are negative. AUC of 0.997, whichi is a good fit.

Then considered a detailed biophysical model with non-linear membrane, somatic spiking, and calcium spikes in the apical dendrites. But first considered only AMPA and GABA synaptic channels. To model this, 1 hidden layer with 128 units is enough.

Then as an intermediate step, considered 9 synapses only, on a single branch, but with NMDA receptors. Already need 4 units in a hidden layer to model this.

Finally, a full neuron with NMDA channels could only be modeled with 7 layers, 128 units each, and in a deep convolutional, rather than fully connected way. With batch normalization. And a learning schedule.

> I'm actually not sure I understand the architecture they used, and it doesn't seem to be presented. Did they have several independent channels with deep filters? Running in parallel? Of what dimensions? And then a fully connected softmax at the end? Why so late? Doesn't a neuron start integrating branches earlier?

# My confusions

They  don't describe the criteria of when "a good fit" is achieved, and their process of looking for a good fit. What was the space of their architectures? Did they try to go wide first, and if it didn't work, went deep? Or something else? Did they only consider "rectangular" models with the same number of units in each layer? (They end up presenting a rectangular model of 7×128, but why?) Did they only consider fully convolutional (deep convolutional) networks? But then why not for the first 3 exercises? 

What was the criterion for when to stop the search? Did they separately optimize hyperparameters for each architecture? (it seems so, but it's not stated directly.)

Also their evaluation seems unusual, as they actually use some sort of threshold? Wouldn't they just compare softmax outputs to true labels?

All in all, maybe these problems are because it's a pre-print and once it is reviewed properly, we'll know the truth?