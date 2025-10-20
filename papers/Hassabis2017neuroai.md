# Neuroscience-inspired artificial intelligence

Hassabis, D., Kumaran, D., Summerfield, C., & Botvinick, M. (2017). Neuroscience-inspired artificial intelligence. Neuron, 95(2), 245-258.
https://www.sciencedirect.com/science/article/pii/S0896627317305093

#neuro #review

Parents: [[deepneuro]]

Quite popular (~700k)

In 1950s-80s AI and Neuro collaborated a lot (some links), but recently they parted ways. And that's bad because the space of solutions is probably quite sparse, and it would be silly not to use those handed to us by evolution. But on top of this **inspiration**, there's also **validation**: if an algo discovered in ML is later found in bio brains, it suggests that it is robust, runs on different hardware, and thus is likely to be useful in the long-term.

I really like this bold terminology: "Throughout this article, we employ the terms “neuroscience” and “AI.” We use these terms in the widest possible sense. When we say neuroscience, we mean to include all fields that are involved with the study of the brain, the behaviors that it generates, and the mechanisms by which it does so, including cognitive neuroscience, systems neuroscience and psychology."

Brief history. DL grew from interest to parallel distributed computing, in the time (80s) when most classic AI was sequential. And that was totally neuro-inspired. Similarly [[convnet]]s grew from Hubel & Wiesel.

Footnotes:
* About convnets: Yamins, D. L., & DiCarlo, J. J. (2016). Using goal-driven deep learning models to understand sensory cortex. Nature neuroscience, 19(3), 356-365. http://brainmind.umin.jp/PDF/wt17/Yamins3.pdf
* On [[dropout]]: Hinton, G. E., Srivastava, N., Krizhevsky, A., Sutskever, I., & Salakhutdinov, R. R. (2012). Improving neural networks by preventing co-adaptation of feature detectors. arXiv preprint arXiv:1207.0580. https://arxiv.org/pdf/1207.0580.pdf
Looks like a proto-drop-out paper that already mentions the word, but doesn't yet explain it as well as they did in their main 2014 paper.

Reinforcement learning ([[rl]]) was also originally inspired by animal behavior (conditioning) experiments.

## Attention

A nice review paragraph, but it's not quite clear from it if it also speaks about [[transformers]]-like attention, or whether it only thinks about larger-scale phenomena. I suspect they may be talking about RNNs mostly, but this needs to be investigated. Is Transformers attention similar to this attention?

Footnotes: 
* Xu, K., Ba, J., Kiros, R., Cho, K., Courville, A., Salakhudinov, R., ... & Bengio, Y. (2015, June). Show, attend and tell: Neural image caption generation with visual attention. In International conference on machine learning (pp. 2048-2057). PMLR. (6k citations)
* Mnih, V., Heess, N., Graves, A., & Kavukcuoglu, K. (2014). Recurrent models of visual attention. arXiv preprint arXiv:1406.6247. (2k citations)

## Episodic memories

They link episodic memories to RL (deep Q-networks, or DNQ, like those used for Atari games), as their learning is based, among other things, on "experience replay": remembering past plays, and learning from the offline. They call this replay buffer "a very primitive hippocampus".

> But see, that's actually different than imitating a network brain system with an ANN; it's more of a prosthetic algorithmic device that plays a similar architectural function, no? It's still Neuro-inspired, I guess, but only at a systems level, not a a unit level.

Footnotes: 
* Mnih, V., Kavukcuoglu, K., Silver, D., Rusu, A. A., Veness, J., Bellemare, M. G., ... & Hassabis, D. (2015). Human-level control through deep reinforcement learning. nature, 518(7540), 529-533. (13k citations)
* Silver, D., Huang, A., Maddison, C. J., Guez, A., Sifre, L., Van Den Driessche, G., ... & Hassabis, D. (2016). Mastering the game of Go with deep neural networks and tree search. nature, 529(7587), 484-489. (9k citations)

**Working memory**: compare it to [[lstm]].

**Continual learning**: how to learn new things without forgetting old things. They briefly describe the logic of learning & forgetting in bio systems, and then semi-link it to 1 ANN paper (Kirkpatric2017; see [[catastrophic_forgetting]])

## Program for the future

Each point is a paragraph with links:

Efficient learning, Transfer, Imagination and planning	

**Virtual brain analytics** - related to [[interpretability]]: learning to analyze how ANNs do what they do. Activity maximization: supernormal stimuli ([[superstimulus]]), [[illusions]]. Dimensionality reduction.

**AI as a model for Neuro**

* Visual system (see [[vision]])
* Memory: * O'Reilly, R. C., & Frank, M. J. (2006). Making working memory work: a computational model of learning in the prefrontal cortex and basal ganglia. Neural computation, 18(2), 283-328.