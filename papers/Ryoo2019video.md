# Video Architecture Search

Michael S. Ryoo, Google, 2019
https://ai.googleblog.com/2019/10/video-architecture-search.html

Parent: [[archsearch]]
See also: [[gen_alg]]

#archsearch #evo #video #blog #gen_alg

A mini-review of 3 papers from the same group.

Videos are hard to process, as they combine "what" (appearance) with motion. Most architectures either try to expand image-based 2D architectures to 3d, or to create 2 information flows that are then manually fused.

Instead, they try three architecture evolution algorithms (links to arxiv papers)
* [EvaNet](https://arxiv.org/abs/1811.10636)
* [AssembleNet](https://arxiv.org/abs/1905.13209)
* [TinyVideoNet](https://arxiv.org/abs/1910.06961)

One nice side-product of using an evolutionary search is that you end up with several different architectures of similar performance, that can all be trained, run in parallel, and made into an ensemble.

**AssembleNet**: their goal was to discover something like multisensory integration (or at least multi-stream integration), and they developed graphical representation of architecture with this goal in mind. Also some innovation in mutation?

**Tiny Video Networks**: the goal was to find networks that can run realtime, by including runtime (total computer) in the objective function. 

> " Interestingly the learned model architectures have fewer convolutional layers than typical video architectures: Tiny Video Networks prefers lightweight elements, such as 2D pooling, gating layers, and squeeze-and-excitation layers."