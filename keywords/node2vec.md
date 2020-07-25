# node2vec

#graph #embedding

Parent: [[09_Graphs]] / [[graph_embedding]]
Related: [[word2vec]], [[knowledge_graph]], [[deep_graphs]]


We start with **random walks** around each node (see [[graph_embedding]]). A **neighborhood** $N_R(u)$ is a multi-set of all nodes that were visited during random walks starting at u following strategy R. As IRL most networks follow a power-law distribution, neighborhoods of each node quickly grow very large.

We want to find the vector embeddings z_u ∈ $R^d$ to maximize log-likelihood that nodes v observed in the neighborhood of u $N_R(u)$ would also be predicted to be be in this neighborhood based on their z_v. We define loss as $\displaystyle L = - \sum_{u ∈ V} \sum_{v ∈ N_R} \log P(v|z_u)$ , and eventually we'll be using SGD on it.

The probability here may be calculated using a sofmax: $P(v|z_u) = \exp(z_u^\intercal z_v) / Σ_i \exp(z_u^\intercal z_i)$

> At this point I don't have a good intuition for how it can help to identify motifs (nodes that are very far from each other, but are very similar in terms of their topological context).

The problem with the expression above is that we have to loop through too many nodes in the denominator here (normalization term). We cannot afford it, so we need to approximate it. Solution: **negative sampling**, aka **Noise-Contrastive Estimation** (NCE; Goldberg 2014; see [[word2vec]]):

$\displaystyle \log\frac{\exp(z_u^\intercal z_v)}{Σ_i \exp(z_u^\intercal z_i)} \approx \log(σ(z_u \cdot z_v)) - \sum_{i \in S} \log(σ(z_u \cdot z_i))$ 
here σ is a sigmoid function, and S is sampled from all nodes in some representative way. We normalize against a negative sample, which gives us linear complexity. How to sample? Typically, proportional to a node degree; sample sizes k~5-20. The sigmoids seem to come from an idea of iterative tree-like probabilistic descent, first introduced by [[hierarchical_softmax]], escept this procedure is more efficient.

It is also important to bound z_u, or trying to maximize dot-products, z vectors will try grow to inf, and we don't want it.

> Why sampling proportional to degree? Does it come from some info about how real random walks behave? Potentially, a good homework: check empirically that sampling proportional to node degree makes sense.

**How to perform random walks?** Easiest approach: fixed length. Cannot go beyond "nodes are close to each other in the graph". One possible solution: **node2vec** that uses biased 2nd order walks. Can be seen as a trade-off between BFS (very local, microscopic) and DFS (in real (small-world) graphs, quite global).

Two parameters:
* p (aka **return parameter**): probability of returning back to the previous node
* q (aka **in-out parameter**): a ratio of BFS-style and DFS-style transitions. 

In practice, they use unnormalized probability 1 for BFS-style step (sample another node that is connected both to the current node and to the previoud node), and 1/q for DFS step (go to a node that is no longer connected to the previous node). And an unnormalized probabiliy 1/p for a back-step. A distribution of co-occurrence of nodes in a random walk is now a function of these two parameters (p, q). Ref: Grover 2016.

> Still not sure how it can help with motifs. It probably cannot?

A cool thing is that both random walks implementation and minimization are parallelizable.

What to do next?
* **Label nodes**: Train a classifier z_v → label
* **Cluster** z_v and predict communities
* **Link prediction**: use some type of an aggregation function for 2 z-values, then train a classifier on them. Options for aggregators: concat, hadamard, sum/average, distance between two z. Depending on the task, some of these options may work much better than the others.

> Any good intuition for when one would be better than the other? Can we find 4 intuitive examples when each of these 4 aggregation options would work better than the other three?

# Refs

Original publication:
Grover, A., & Leskovec, J. (2016, August). node2vec: Scalable feature learning for networks. In Proceedings of the 22nd ACM SIGKDD international conference on Knowledge discovery and data mining (pp. 855-864). (3k citations)
https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5108654/

An attempt to make sampling more efficient (4-10 times faster) by using prior knowledge about netwok degree statistics (power law):
https://github.com/NilsFrahm/Node2vec