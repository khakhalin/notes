# Backprop in the brain, 2020

Lillicrap, T. P., Santoro, A., Marris, L., Akerman, C. J., & Hinton, G. (2020). Backpropagation and the brain. Nature Reviews Neuroscience, 21(6), 335-346.
https://brainscan.uwo.ca/research/cores/computational_core/uploads/11May2020-Lillicrap_NatNeuroRev_2020.pdf

Parents: [[deepneuro]] / [[credit]]

#deepneuro #neuro #credit #review


Is there backprop in the brain? Changes of synaptic connections are a given, and there have to be some non-local aspect to these changes (even if the rules themselves are local), for the brain to learn anything useful. But in ANNs people use [[backprop]] that carries info from the end (loss function, error) through the network (even for self-supervised and reinforcement learning [[11_RL]]). As ANNs, The brain has lots of feedback connections, but apparently they are used for different purpose (not for backprop), and most local learning seems unsupervised. How does it work then, and is backprop even a useful metaphor for bio networks?

They claim that perhaps the brain emulates backprop, and that we should pay attention to methods they call **neural gradient representation by activity differences** (NGRAD). It allows separating backprop-y logic from actual implementation of backprop. They also explicitly note that they ignore the question of priors (but see [[Zador2019pure]] for a discussion of meaningful priors)

A spectrum of learning algorithms, from scalar to vector:
* **Hebbian**: at least for FF networks Hebbian learning is totally not helpful; it does nothing to help the network approach the solution, and in fact, if allowed, will bring the network away from the solution (_they don't comment on recurrent networks and [[stdp]]-like types of Hebbian learning tho_)
* **Perturbation	learning**: slow greedy algorithm that involves actually running the network several times, with one global reward-like feedback, and only retaining changes that improve the feedback. This actually works better if we perturb unit outputs and not weights, and then adjust weights to change neuron output in this direction.
* [[backprop]] and **backprop-like** approximations (see [[credit]] for work on loosening backprop requirements)

**Perturbation learning** looks very plausible from the bio pov, and intuitively it totally should be used by the brain somewhere, but it is curious that we actually never managed to do anything useful (practical) with it in ANNs. Which is troubling. But that's because it's unbelievably slow.

> My thoughts: If it's so slow, then arguably it can only work in recurrent self-supervised learning, so maybe [[echo]]-like networks and [[synfire]] chains may be a better candidate for using it? Also [[sleep]]. Unsupervised (or self-supervised) spontaneous running is the only chance to generate enough error signals to do anything meaningful with a greedy algorithm. Also, even Hebbian learning is at least somewhat useful in the context of RNNs (see Hopfield networks: [[hopfield]], Boltzmann machines), which is inspiring.

For good learning, we want robust **credit assignment**, and we want it to be as vector-like as possible (to decouple and parallelize learning of different parts of the network).

An interesting feature of backprop in ANNs is that it finds good representations (see [[embedding]]) in intermediate layers: useful and reusable (generalizable) vector space. Which really resonates with neuro work on representations. And also the idea of feedback connections isnt' foreign to the brain; on the contrary (think visual system; see [[vision]] that is full of top-down connections going interacting with the bottom-up information flow). _But aren't V2→V1 connections mostly for attention and/or priors during computation, not during leraning?_

A naive backprop (with error using Wᵀ to propagate) is of course impossible, but it was recently shown that any matrix (including random) would work here. There was also older work on the so-called **weight transport problem**, and various types of retrograde signaling (refs 84-88), but now this line of thinking is considered a false-start, and a dead-end.

Footnotes for this claim: 
* Lillicrap, T. P., Cownden, D., Tweed, D. B. & Akerman, C. J. Random synaptic feedback weights support error backpropagation for deep learning. Nat. Commun. 7, 13276 (2016).
* Cadieu, C. F. et al. Deep neural networks rival the representation of primate IT cortex for core visual object recognition. PLoS Comput. Biol. 10, e1003963 (2014).
* Yamins, D. L. et al. Performance-optimized hierarchical models predict neural responses in higher visual cortex. Proc. Natl Acad. Sci. USA 111, 8619–8624 (2014)]

Finally, there are two more indirect and vulnerable, but cool arguments: 
1. Backprop-based ANNs are quite decent in predicting how cortex is tuned (refs from 68 to 72, including motor and sensorimotor regions), *unlike* computationally effective, but not-ANN-based models, which is super-inspiring.
2. Backprop-based ANNs are for now the only algorithm that approaches human- (animal)-like visual recognition (say, on Imagenet). Can backprop-like logic be, at least to some extent, inevitable? _At least if you loosen the definition enough to include any kind of parallelized vector-based descent?_

Next point: If there's backprop in the cortex, then somebody needs to calculate errors, right? Guess what, there are neurons in the cortex that compute mismatches between predicted and observed sensory events!

Footnotes:
* Zmarz, P. & Keller, G. B. Mismatch receptive fields in mouse visual cortex. Neuron 92, 766–772 (2016). 80. 
* Issa, E. B., Cadieu, C. F. & DiCarlo, J. J. Neural dynamics at successive stages of the ventral visual stream are consistent with hierarchical error signals. eLife 7, e42870 (2018).

And the problem of random feedback projections was surprisingly "solved" through an observed phenomenon of **feedback alignment**, where random feedback matrix just shapes feedforward matrix to "match" the given feedbacks. It may not work for deeper networks, but apparently there are tricks (3 refs, shown below in footnotes).

Footnotes: 
* Lillicrap, T. P., Cownden, D., Tweed, D. B. & Akerman, C. J. Random synaptic feedback weights support error backpropagation for deep learning. Nat. Commun. 7, 13276 (2016).
* [[Murray2019local]]
* Amit, Y. Deep learning with asymmetric connections and Hebbian updates. Front. Comput Neurosci. 13,
18 (2019). 
* Bartunov, S. et al. in Adv. Neural Inf. Process. Syst. 9390–9400 (NIPS, 2018).
* Akrout, M., Wilson, C., Humphreys, P. C., Lillicrap, T. & Tweed, D. Using weight mirrors to improve feedback
alignment. (2019). Preprint at https://arXiv.org/1904.05391 

One problem with bio backprop is that errors are supposed to have a sign, but except for cerebellum, most (or at least many) parts of  the brain don't seem to be working with signed info.

But feedback in the brain alters brain activity a lot (in the cortex, it can outright drive activity, not even just alter it), while most ANNs currently in use are feed-forward. There's this theory of hierarchical Bayesian inference of perception. But essentially, maybe it's our current ANN solutions that are not there yet, and not the brain that is weird.

Footnotes:
* Hinton, G. E., Dayan, P., Frey, B. J. & Neal, R. M. The ‘wake–sleep’ algorithm for unsupervised neural networks. Science 268, 1158–1161 (1995).
* Ahissar, M. & Hochstein, S. The reverse hierarchy theory of visual perceptual learning. Trends Cognit. Sci. 8, 457–464 (2004). 
* Lee, T. S. & Mumford, D. Hierarchical Bayesian inference in the visual cortex. J. Opt. Soc. Am. A Opt Image Sci. Vis. 20, 1434–1448 (2003). 
* Lewicki, M. S. & Sejnowski, T. J. in Adv. Neural Inf. Process. Syst. 529–535 (NIPS, 1997).
* Knill, D. C. & Richards, W. Perception as Bayesian Inference (Cambridge Univ. Press, 1996).
* Dayan, P., Hinton, G. E., Neal, R. M. & Zemel, R. S. The Helmholtz machine. Neural Comput. 7, 889–904 (1995)

> So does it imply  that greedy learning, similar to **deep belief networks** [[dbn]] may still have a come-back? Just this time learning continuously, in recurrent ([[rnn]]), self-supervised ([[08_Unsupervised]]) mode?

**NGRAD hypothesis**: Neural Gradient Representation through Activity Differences. The motivation is to avoid having 2 different information types - activity and errors - and somehow encode errors in the activities themselves. Loosely inspired by [[boltzmann_machine]]s.

Consider [[autoencoder]]s: a bottleneck in the middle forces a good representation ([[embedding]]). Then we do **target propagation**: something that looks very similar to a "realtime" deep belief network, with a series of autoencoders, each one telling the previous one what input was expected (what input would have driven the autoencoder optimally). Except while in Deep Belief we have (input→hidden→output)→(=input→hidden→out) ..., here each layer is a hidden layer for step i, but also an input layer for step i+1. If the feed-forward process carries data to the output, but also a series of feed-back inverses generate what the data should have been, then at each layer, the difference between feed-forward and feed-back activity is the vector of errors that can be used for weight change. The problem here of course is that perfect inverses are biologically implausible.

But wait, we can **learn inverses** via tiny backprop-like optimizations!! At each step (for each layer) we do output→feedback weights→estimated input (as activity), then compare estimated input to real input and use it to bring feedback weights closer to the inverse of feed-forward weights. Aka **Difference target propagation** (**DTP**). It actually doesn't perform that well on ImageNet (people tried), but at least it shows that stuff is possible in principle.

> So let me rephrase it. Here comes input x. It's transformed into output y. Then the network is told what the output should have been (υ). This υ is reverse transformed to should-have-been input ξ. Now direct weights are changed to move y towards υ (like the first step of normal backprop), while reverse weights are moved to make ξ close to x (back-backprop, aka let's keep reverse weights closer to an inverse). This should eventually converge. Right?

> The good thing here is that the feedback can be interpreted as "expectation" of what next layer had from previous layers, which matches what we supposedly know about the brain. The problem, at least the way I understand it now is that from the information processing pov it's good to sum (average) ff and fb information flows, but from the error propagation pov (and thus backprop pov) you want to look at the difference between ff and fb flows. So info processing expects ff and fb to happen at the same time, while DTP expects them to happen consecutively. Right? Or do I misunderstand things?

How to implement something like that in the **brain**? We can have error-reporting neurons, but perhaps it is enough to have error-reporting fibers, and error-dedicated dendrites (distal dendrites in cortical pyramidal neurons, aka **apical tufts dendrites** see #cortex) that are not strongly electrically coupled with somatic dendrites, but can affect them via **plateau potentials**, or be affected by them via **backpropagating APs**. (Lots of good biological refs here)