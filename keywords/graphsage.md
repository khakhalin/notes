# Graph Sage

#dl #graph

Parents: [[09_Graphs]] / [[graph_embedding]] / [[deep_graphs]]
Related: [[cnn]]

Let's generalize the simple idea from [[deep_graphs]] to a case of
1. more general aggregation function
2. concatenation instead of summation

$\displaystyle h^k_v = ϕ \left( \left[A_k \cdot \bigotimes_{u∈N(v)}h^{k-1}_u \; , \; B_k h^{k-1}_v \right] \right)$
* ⨂ - some generalized aggregation function. We can try mean, but also pooling (element-wise min/max), or even something learnable like LSTM over several inputs (except that in this case we would have to try several random orderings of inputs and average the results, to keep it order-invariant).
* $[ \cdot \, , \cdot ]$ - concatenation

Many aggregators can be rewritten in a vector form as sparse matrix multiplication (A is an adjacency matrix):
* For example, the mean, where you just sum and divide by degree: $H^k = D^{-1}AH^{k-1}$
* Another example from (Kipf 2017): $H^k = D^{-1/2}A D^{-1/2}H^{k-1}$

> The lecture has a whole slide with small-font bibliography. Parse?

# Attention on graphs

What if locally, some neighbors are more important than the other? Introduce $α_{vu}$ - the importantce of v for u. Can we learn it?

We can calculate α based on the messages passed by different nodes. 
First look at unnormalized "imporance": $e_{vu} = a(W_k h^{k-1}_u, W_k h^{k-1}_v)$
where $W_k$ is again some transformation matrix, and $a()$ is some attention mechanism.

Then do softmax of these importances: $\displaystyle α_{vu} = \frac{exp(e_{uv})}{\sum_{k ∈ N(v)} exp(e_{vk})}$

And then we can use these importance weights during aggregation:

$h^k_v = ϕ\left( \sum_u α_{vu} W_k h^{k-1}_u \right)$

How to learn a good $a()$? Maybe use a simple network and learn its weights?

> Find a formula that defines a() as a linear network?

# Refs

Hamilton, W., Ying, Z., & Leskovec, J. (2017). Inductive representation learning on large graphs. In Advances in neural information processing systems (pp. 1024-1034).
https://papers.nips.cc/paper/6703-inductive-representation-learning-on-large-graphs.pdf
(2k citations in 3 years!)

Veličković, P., Cucurull, G., Casanova, A., Romero, A., Lio, P., & Bengio, Y. (2017). Graph attention networks. arXiv preprint arXiv:1710.10903.
https://arxiv.org/pdf/1710.10903.pdf
Multi-head attention on graphs

Ying 2018 - PinSage - learning image classification from sparce bipartite graph of these images placed in different thematic collections by Pinterest users