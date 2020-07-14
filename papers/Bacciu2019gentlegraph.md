# Gentle intro to Deep Learning for Graphs

Bacciu, D., Errica, F., Micheli, A., & Podda, M. (2019). A Gentle Introduction to Deep Learning for Graphs. arXiv preprint arXiv:1912.12693.
https://arxiv.org/abs/1912.12693

#graphs #dl #review

Parents: [[09_Graphs]]
See also: [[07_RNNs]], [[algos_graph]], [[echo_state]]

This paper pays special attention to **early attempts**, to establish pioneering works, and avoid wheel reinvention.

Graphs are everywhere. However applying DL to them is challenging:
* Input size varies
* Discrete, hard to optimize (no differentiation)
* Combinatoric issues (have to be invariant to reordering of nodes)
* Have **cycles** (become a problem for most naive approaches)

But RNNs kinda inherit to graphs in spirit, and a bunch of guarantees have been achieved. A recurrent structure essentially **climbs** through the graph, and computs the **embedding**. (And that's where cycles become a problem). Earliest models from ~2009. From back then, two main approaches: recurrent and FF (typically, convolutional).

Introduce a bunchof terminology, including **labels** (vectors) for both nodes and edges. Their default graph is directed (good!). **Topological ordering**: only if a graph is acyclic. **Ordered** graphs have all edges adjacent to every node sorted (ordered); **positional** graphs have every node have some named "valences" of sorts, and edges "connect" to them. Node **neighborhood**: all nodes that have edges pointing from them AT this node (like a flow basin).

Ultimately we want to describe the role of each node within a graph as a vector (aka **representation**, or **state**). These representations shouldn't depend on the order in which nodes were considered.

They propose three names for three branches of models:
* Deep **Neural** graph networks (either recurrent, or FF)
* Deep **Bayesian** graph networks
* Deep **Generative** graph networks

How to beat graph problems, starting with **variable size**? Work locally, from the node level. Another benefit is that it turns into something like a convnet, as parameters for learning node properties can be reused across all nodes. But even then the combinatorial problem stands, as for a typical (unordered) graph there's no way to allocate "roles" among the nodes in the neighborhood; how to even encode them? All functions that deal with with combining info from neighboring nodes should be **permutation invariant** (e.g. mean, sum, product). There's a theorem that shows that any possible permutation invariant function can be represented as $Ψ(Z) = ϕ(Σ_z ψ(z))$ where {z} are the neighborhood, and ϕ and ψ are certain continuous functions.

What about **cycles**? It means that a calculation of node states is technically infinite, so in practice it has to be made **iterative**, and it has to **converge**. (Or, in some models, instead of an interation, they use layers of a FF network, which is slightly different, but similar in spirit).

To represent the entire graph (ascend from node-level to graph-level) we need **context diffusion**. A combination of **message dispatching** (using the node state and edge info to calculate the influence on nodes nearby) and **state update**.

Three possible architectures here:
* **Recurrent**. Examples: **Graph Neural Network**, **Graph Echo State Network**, **Gated Graph Neural Network**, **Fast and Deep Graph Neural Network** (apparently, a variety of Echo State actually?).
* **Feed-Forward**. Examples: **Neural networks for graphs**. But really it's not that dissimilar to RRNs, just the rules of proparagion change from one layer to another, creating receptive fields.
* **Constructive Architectures**: similar to FF, but with greedy training of layers (layer by layer). Each layer is assigned (and trained for) a dedicated role. (_By nature, interpretable then? Not a black box?_)

> The notation they use in this paper is really annoying and seems needlessly complicated. A cost for trying to express everything in the most general way possible.

**Attention**: similar to how it works for text, one can separately estimate how important different parts of the story are, and use these estimations as weights when propagating averages (in this case: estimating which neighbors of each node are more  important). See: [[transformers]].

**Sampling** of neighbors is apparently a problem, so in some solutions only some nodes are considered (also helps to make input shapes more predictable). This can also be done via **pooling** with clustering layers. See: [[clustering]]

> What the heck are clustering layers? They reference some "DNGN layer", but Google isn't aware of this term. Is it a typo? Citation given: Zhitao Ying, Jiaxuan You, Christopher Morris, Xiang Ren, Will Hamilton, and Jure Leskovec. Hierarchical graph representation learning with differentiable pooling. In Proceedings of the 32nd Conference on Neural Information Processing Systems (NeurIPS), 2018

Another type of pooling: **Node aggregation** (seems to be the same as **graph condensation**). _But this probably doesn't work for all graphs? Unless you somehow encode the motif-like like properties as a feature vector for each aggregated meta-node. Right?_

Learning criteria:
* Graph classification
* Unsupervised classification / clustering
* Distribution of properties on a graph
* Link prediction
* Generative learning / graph reconstruction from representation: GANs and auto-encoder
* Mutual information between pairs of graphs
* Community detection
* Node classification

Open problems (_a sublist of those  I related to_)
* Time-evolving graphs
* Hypergraphs (algebraic topology?)

# Key Refs

List of reviews: 7, 19, 48, 55, 144, 130, 143

**Graph Neural Network** classic link:
Franco Scarselli, Marco Gori, Ah Chung Tsoi, Markus Hagenbuchner, and
Gabriele Monfardini. The graph neural network model. IEEE Transactions on Neural Networks, 20(1):61–80, 2009. Publisher: IEEE.

**Graph Echo State Network** historical link:
Claudio Gallicchio and Alessio Micheli. Graph echo state networks. In
Proceedings of the International Joint Conference on Neural Networks
(IJCNN), pages 1–8. IEEE, 2010