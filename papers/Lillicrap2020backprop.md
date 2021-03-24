# Backprop in the brain, 2020

Lillicrap, T. P., Santoro, A., Marris, L., Akerman, C. J., & Hinton, G. (2020). Backpropagation and the brain. Nature Reviews Neuroscience, 21(6), 335-346.
https://brainscan.uwo.ca/research/cores/computational_core/uploads/11May2020-Lillicrap_NatNeuroRev_2020.pdf

Parents: [[deepneuro]] / [[credit]]

#deepneuro #neuro #credit


Is there backprop in the brain? Changes of synaptic connections are a given, and there have to be some non-local aspect to these changes (even if the rules themselves are local), for the brain to learn anything useful. But in ANNs people use [[backprop]] that carries info from the end (loss function, error) through the network (even for self-supervised and reinforcement learning [[11_RL]]). The brain also has lots of feedback connections, but apparently used for different purpose, and most local learning seems unsupervised. How does it work then, and is backprop a useful metaphor for bio networks?

They claim that perhaps the brain emulates backprop, and that we should pay attention to methods they propose to call **neural gradient representation by activity differences** (NGRAD). It allows to separate backprop-y logic from implementation. They also explicitly note that they ignore the question of priors (see [[Zador2019pure]])

A spectrum of learning algorithms, from scalar to vector:
* **Hebbian**: at least for FF networks Hebbian learning is totally not helpful; it does nothing to help the network approach the solution, and in fact, if allowed, will bring the network away from the solution (_they don't comment on recurrent networks and [[stdp]]-like types of Hebbian learning tho_)
* **Perturbation	learning**: slow greedy algorithm that involves actually running the network several times, with one global reward-like feedback, and only retaining changes that improve the feedback. This actually works better if we perturb unit outputs and not weights, and then adjust weights to change neuron output in this direction.
* [[backprop]] and **backprop-like** approximations (see [[credit]] for some works on loosening backprop requirements)

**Perturbation learning** looks very plausible from the bio pov, and intuitively, it totally should be used by the brain somewhere, but it is curious that we actually never managed to do anything useful with it in practical ANNs. Which is troubling. But that's because it's unbelievably slow.

> If it's so slow, arguably, it can only work in recurrent self-supervised learning, so maybe [[echo]]-like networks and [[synfire]] chains may be a better candidate for using it? Unsupervised (or self-supervised) spontaneous running is the only chance to generate enough error signals to do anything meaningful with a greedy algorithm. Also, even Hebbian learning is at least somewhat useful in the context of RNNs (Hopfield networks [[hopfield]], Boltzmann machines), which is inspiring.

For good learning, we want robust **credit assignment**, and we want it to be as vector-like as possible (to decouple and parallelize learning of different parts of the network).

