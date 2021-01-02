# t-SNE

#clustering #dim #features

Parents: [[04_Features]]
Related: [[curse_dim]], [[umap]], [[spectral_clustering]]

**t-SNE**, or **tSNE**, stands for **t-distributed Stochastic Neighbor Embedding**. A rather finicky and unpredictable method of non-linear embedding. Not-reproducible (even tho we can fix the random seed); sensitive to small changes in the data.

> For now (Jan 2021) it seems that it was largely superseded by [[umap]]. But maybe not in all fields?

**Logic**: defines distances between points, and also measures distances on a projection. Then defines a certain "probability of similarity" (something alike to connectedness, or "being a neighbor") that declines with distance (Gaussian kernel), and is normalized for every point. Finally, minimize the KL-divergence ([[kl]]) of these "probabilities of being a neighbor" between the original set, and in the projection, using gradient descent.

Single most important hyperparameter: **perplexity** (typically set "between 5 and 50") controlling the balance between global and local aspects of data, and can be thought of as a rough guess about the number of meaningful neighbors for every point. Best reconstruction of the original data is achieved somewhere in-between, so neither extreme would work. If perplexity is too small, you'll get lots of fragmented clusters, with no global picture; if it is too large, everything will be just mixed together into one cluster. Also, in general, perplexity may have to be increased slightly we get more data, even if the underlying structure of the data doesn't change (if we still sample from the same distribution).

> Supposedly this "target perplexity" somehow defines a Gaussian kernel that is used to move from distances to "probabilities of being a neighbor", but I'm not sure I get what it means exactly.

Iterating until convergence is important (early stopping is really not recommended here). Rule of thumb: if clusters have a weird shape (like all elongated in the same direction), it's a sign that we stopped too early.

General patterns of behaiov: 
* **Small perplexity** (<10): clusters are too many, and have wisp-like quality. These wisp-like tiny clusters arise even if the data is random, and doesn't actually have any fine-level structure. For low-D manifolds embedd into a high-D space (ropes), tends to untie and untangle these ropes (doesn't notice that they are knotted).
* **Large perplexity** (~50-100): Concentrates on large scale structure. Tends to project a high-D Gaussian into a uniform field (which is arguably a valid approximation, as very hidh-D Gaussian is close ot being uniform on a sphere; see [[curse_dim]]). If clusters are well-defined, may project an entire cluster essentially into one point. Doesn't  untangle knots.

There's nothing shameful about working with several projections (with different perplexities) at the same time, as each of them may capture some unique features of the original distribution (at different scales).

# Refs

How to use t-SNE effectively
https://distill.pub/2016/misread-tsne/

"The art of using tSNE for transcriptomics" from Nature:
[https://www.nature.com/articles/s41467-019-13056-x](<https://www.nature.com/articles/s41467-019-13056-x>)

https://scikit-learn.org/stable/modules/generated/sklearn.manifold.TSNE.html