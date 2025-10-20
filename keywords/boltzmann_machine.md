# Boltzmann Machine

Parents: [[unsupervised]], [[rnn]]
Sister models: [[hopfield]], [[ising_model]]
See also: 

#unsupervised


One of the **energy-based models** (minimize a certain target value not during training, but during activity). It's an all-to-all weighted graph, with energy defined in the same way as for [[hopfield]]: $\displaystyle E = -\left( \sum_{i<j} w_{ij} s_i s_j + \sum_i θ_i s_i \right)$, where w_ij are connection weights, s_i are states (activities), that can be only 0 and 1 (as in [[ising_model]]), and θ_i are biases of some sort (threshold activations?). In practice the weight matrix is often taken to be symmetric.

Then there's some transition fro this definition to E to the effect of one neuron switching its s_i that I don't understand, and that introduces **temperature** T as a parameter. _Need to read more about it_ #todo

> And then we somehow try to minimize E I think? Without explicitly running BM as an RNN, but just optimize in whatever way we can? NOT SURE. #todo

What BMs can do is to learn a distribution (how???), and then denoise signals, or reconstruct missed parts of the signal. So it's a model of #memory, and maybe also a generative model?

Also there can be **restricted BMs** and **deep BMs** - ???? #todo

# Refs

https://en.wikipedia.org/wiki/Boltzmann_machine