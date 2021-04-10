# Can single bio neurons solve MNIST?

Jones, I. S., & Kording, K. P. (2020). Can Single Neurons Solve MNIST? The Computational Power of Biological Dendritic Trees. arXiv preprint arXiv:2009.01269.
https://arxiv.org/abs/2009.01269

Parent: [[dendritic_comp]]
Related: [[mnist]]

#neuro #dendritic #benchmarks


Oldest models of bio neurons assumed that they linearly sum inputs, and apply a non-linear transformation (McCulloch and Pitts, 1943) - which is oddly what ANNs do still! But dendrites are non-linear (Antic et al., 2010; Schiller et al., 2000; Branco and Häusser, 2011, Mel, 2016; London and Häusser, 2005; Agmon-Snir et al., 1998; Koch et al., 1983), and perform logical operations (at least AND, but sometimes more). Recent modeling of biophysical neurons with ANNs favors a 2-layer ANN.

An interesting property of bio neurons is that input axons make on average 4 different synapses on 4 different parts of the dendritic tree, which means that inputs to an ANN modeling a bio neuron had to be distributed peculiarly (via a sparse input matrix). Moreover, LTP doesn't just change the strength of existing synapses, but increases the number of synapses between 2 neurons.

Here instead of working with a biophysical neuron, or approximating a biophysical neuron with a general-purpose ANN, they created a k-times binary tree ANN: a single output node, approximating a soma, getting inputs from k binary trees, each with depth m (e.g. for m=3, we have 2 consecutive branchings, so the total number of nodes in one tree = 1 + 2 + 4 = 7 = $2^3-1$ nodes). Repeated inputs (to different parts of trees). The number of parameters in this network, assuming n leafs (highest-level inputs) in each tree = k(2n-1).

Binary classification task (so not all possible digits, but only deciding between 2 digits). And despite the title, they tried other datasets as well: Fashion-mnist, EMNIST (letters), Kuzushiji-mnist (old Japanese cursive Kuzushiji characters), CIFAR-10 (cats from dogs), SVHN (street house numbers), USPS (number digits, an alternative to mnist, it appears).

As benchmarks:
* Lower benchmark: linear classifier with n parameters (just inputs converging on an output)
* Higher benchmark: fully connected network with 1 hidden layer size h. A total of h(n+1) parameters, and by setting h to 2k we can make the number of parameters match that in the "tree-based" model. Which makes an honest comparison.

Results: 
* on average, a 1-tree (k=1) performs much better than linear classifier, but worse than a classic network, and super-varible for some reason.
* With increasing k, the difference between tree-based and classic ANNs becomes smaller. They present it as a model of "repeated inputs" that happen in biology (as described above).