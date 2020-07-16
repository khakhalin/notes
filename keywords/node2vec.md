# node2vec

#graph #embedding

Parent: [[09_Graphs]] / [[graph_embedding]]
Related: [[word2vec]], [[knowledge_graph]]

Main publication:
Grover, A., & Leskovec, J. (2016, August). node2vec: Scalable feature learning for networks. In Proceedings of the 22nd ACM SIGKDD international conference on Knowledge discovery and data mining (pp. 855-864).
(3k citations)

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