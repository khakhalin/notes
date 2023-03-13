# t-SNE

#clustering #dim #features

Parents: [[04_Features]]
Related: [[curse_dim]], [[spectral_clustering]]

See also: [[umap]] has a whole section on differences between tSNE and UMAP.

**t-SNE**, or **tSNE**, stands for **t-distributed Stochastic Neighbor Embedding**. A rather finicky method of non-linear embedding. Not-reproducible (at least with random init); sensitive to small changes in the data.

> For now (Jan 2021) there are claims that it was largely superseded by [[umap]], but there are also rebuttals.

#todo:
How to use t-SNE effectively
https://distill.pub/2016/misread-tsne/

# Logic

tSNE defines distances between data points, and also measures distances on a projection. Then defines a certain "probability of similarity" (something alike to connectedness, or "being a neighbor") that declines with distance (Gaussian kernel), and is normalized for every point. Finally, minimize the KL-divergence ([[kl]]) of these "probabilities of being a neighbor" between the original set, and in the projection, using gradient descent.

Single most important hyperparameter: **perplexity** (typically set "between 5 and 50") controlling the balance between global and local aspects of data, and can be thought of as a rough guess about the number of meaningful neighbors for every point. Best reconstruction of the original data is achieved somewhere in-between, so neither extreme would work. If perplexity is too small, you'll get lots of fragmented clusters, with no global picture; if it is too large, everything will be just mixed together into one cluster. Also, in general, perplexity may have to be increased slightly we get more data, even if the underlying structure of the data doesn't change (if we still sample from the same distribution).

> Supposedly this "target perplexity" somehow defines a Gaussian kernel that is used to move from distances to "probabilities of being a neighbor", but I'm not sure I get what it means exactly.

Iterating until convergence is important (early stopping is really not recommended here). Rule of thumb: if clusters have a weird shape (like all elongated in the same direction), it's a sign that we stopped too early.

General patterns of behavior: 
* **Small perplexity** (<10): clusters are too many, and have wisp-like quality. These wisp-like tiny clusters arise even if the data is random, and doesn't actually have any fine-level structure. For low-D manifolds embedd into a high-D space (ropes), tends to untie and untangle these ropes (doesn't notice that they are knotted).
* **Large perplexity** (~50-100): Concentrates on large scale structure. Tends to project a high-D Gaussian into a uniform field (which is arguably a valid approximation, as very hidh-D Gaussian is close ot being uniform on a sphere; see [[curse_dim]]). If clusters are well-defined, may project an entire cluster essentially into one point. Doesn't  untangle knots.

There's nothing shameful about working with several projections (with different perplexities) at the same time, as each of them may capture some unique features of the original distribution (at different scales).

# Limitations

A "web-consensus" list of limitations (although see below):
* Doesn't scale well to really large data (in terms of speed)
* cannot meaningfully project to more than 1-3 dimensions (at the very least, UMAP is faster for higher-D)
* doesn't represent (preserve) large-scale organization of data well (see [[umap]] for some attempts to explain why)
* cannot work with very-high-D data directly, needs to first be projected to lower-D with something like PCA or an autoencoder. (Supposedly, this one is no longer true if we don't use random init, and switch to a meaningful PCA-like init)

Footnotes for a list of limitations:
* https://towardsdatascience.com/how-exactly-umap-works-13e3040e1668
* See also: [[umap]]

#todo
Footnotes for sources that argue with these limitations:
* Böhm, J. N., Berens, P., & Kobak, D. (2020). A Unifying Perspective on Neighbor Embeddings along the Attraction-Repulsion Spectrum. arXiv preprint arXiv:2007.08902. https://arxiv.org/abs/2007.08902
* Critique: Initialization is critical for preserving global data structure in both t-SNE and UMAP. Kobak, Linderman. 2021. Nature Biotechnology
* Twitter thread with some comments on the topic and relevant links: https://twitter.com/hippopedoid/status/1356911439022350337

# Implementations

As of Jan 2021, the standard implementation in `sklearn` is dated and slow.

A better implementation is `FIt-SNE`. An even better (current top): `openTSNE` https://github.com/pavlin-policar/openTSNE

# Refs

Recommendations for better default parameters for t-SNE for Dmitry Kobak:
https://github.com/scikit-learn/scikit-learn/issues/18018

"The art of using tSNE for transcriptomics" from Nature:
[https://www.nature.com/articles/s41467-019-13056-x](<https://www.nature.com/articles/s41467-019-13056-x>)

https://scikit-learn.org/stable/modules/generated/sklearn.manifold.TSNE.html