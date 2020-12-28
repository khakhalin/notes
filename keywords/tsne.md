# t-SNE

#clustering #dim #features

Parents: [[04_Features]]
Related: [[curse_dim]], [[umap]]

**t-SNE**, or **tSNE**, stands for **t-distributed Stochastic Neighbor Embedding**. A rather finicky and unpredictable method of non-linear embedding. Unless you fix the random seed, not-reproducible (and sensitive to small changes in the data). Has a single most important hyperparameter: **perplexity** (which is typically set "between 5 and 50"), which controls the balance between global and local aspects of data, and can be thought of as a rough guess about the number of meaningful neighbors for every point. Best reconstruction of the original data is achieved somewhere in-between, so neither extreme would work. If perplexity is too small, you'll get lots of fragmented clusters, with no global picture; if it is too large, everything will be just mixed together into one cluster.

Iterating until convergence is important (no early stopping). If clusters have a weird shape, probably we stopped too early.

Cluster sizes and (if the )

# Refs

How to use t-SNE effectively
https://distill.pub/2016/misread-tsne/

"The art of using tSNE for transcriptomics" from Nature:
[https://www.nature.com/articles/s41467-019-13056-x](<https://www.nature.com/articles/s41467-019-13056-x>)

https://scikit-learn.org/stable/modules/generated/sklearn.manifold.TSNE.html