# Clustering

Parents: [[04_Features]]
See also: [[python]], [[03_Classification]], [[dim_reduction]]

#bib #clustering #features


A nice overview of common clustering methods (from the [[sklearn]] documentation):
https://scikit-learn.org/stable/modules/clustering.html#overview-of-clustering-methods

Methods (subtopics):
* K-means - iteratively minimizes within-cluster sum of squares. The result looks like a voronoi diagram. Simple, but fast and parallelizable.

* [[spectral_clustering]] - STUB
* [[mean_shift]] - an iterative proccess related to hierarchical clustering (transient clusters, but a trivial convergence point)
* Affinity propagation - make points iteratively convince each other to join their clusters, by picking representatives ("exemplars") that are "most typical" for this cluster, and making points vote for them. Slow and I don't quite understand it yet, but picks the number of clusters automatically based on two other hyperparameters that define the dynamics of this process.

# Theory

.

# Metrics

A nice list of metrics for clustering is available here:
https://scikit-learn.org/stable/modules/clustering.html#clustering-performance-evaluation

Most of them compare the results of clustering to the ground-truth, in which case it's nice to have several options to choose form, but it does not feel too tricky. A tricker part is to measure the quality of clustering when the ground truth is not known.

One of the simpler metrix is the **Silhouette Coefficient**: roughly, by how much distances between clusters exceed distances within a cluster. In more detail:
* For every point, find next nearest cluster
* Calculate the average distance between this point and all points in this nearest cluster (b)
* Calculate the average distance between this point and points within its own cluster (a)
* Calculate (b-a)/max(a,b)
* Average these values across all points in the sample

There are also other measures on the list (that seem to be slightly more computationally expensive) that are trying to capture the same intuition (we hope to see that "between > within"). Most of these measures don't work too well with density-based clusters, as well as with weirdly shaped clusters, as when clusters different in density, or when they have interesting shape the between-within dichotomy becomes less intense.

# Implementation in Python

Minimal code:
```python
from sklearn.preprocessing import scale as sklearn_scale
from sklearn.cluster import KMeans as KM
from sklearn.metrics import silhouette_score as silhouette

X = sklearn_scale(df.values) # Center + unit variance
labels = KM(n_clusters=n_clusters, n_init='auto').fit(X).labels_
quality = silhouette(X, labels, metric='euclidean')
```
Notes:
* `silhouette` chokes at about 10 000 points (becomes too slow), so it make sense to test on a sampled test set

# To Read

https://scikit-learn.org/stable/modules/clustering.html
* Including a nice compact description of the Silhouette Coefficient - the most popular way of quantifying clustering quality when ground truth is not available. The one calculated with `sklearn.metrics.silhouette_score`

http://www.homepages.ucl.ac.uk/~ucakche/presentations/cqualitybolognahennig.pdf

Density-based clustering:
https://towardsdatascience.com/a-gentle-introduction-to-hdbscan-and-density-based-clustering-5fd79329c1e8

Comparison of different clustering algorithms:
https://scikit-learn.org/stable/auto_examples/cluster/plot_cluster_comparison.html#sphx-glr-auto-examples-cluster-plot-cluster-comparison-py

A presentation on various measures of clustering quality: http://www.homepages.ucl.ac.uk/~ucakche/presentations/cqualitybolognahennig.pdf

# Refs

Priebe, C. E., Park, Y., Vogelstein, J. T., Conroy, J. M., Lyzinski, V., Tang, M., ... & Bridgeford, E. (2019). On a two-truths phenomenon in spectral graph clustering. Proceedings of the National Academy of Sciences, 116(13), 5995-6000. https://www.pnas.org/content/116/13/5995
It describes how two different types of spectral clustering [[spectral_clustering]] (Laplacian Spectral Embedding, aka "affinity", vs Adjacency Spectral Embedding, aka "core-periphery") give you two opposite answers on a simple horseshoe-like system (where two clusters are closer to each other in space, but are connected to each other only though a core structure). Both answers make sense in some context, but it's important to be aware of the context (essentially, they run on different graphs, right? Distance graph, as opposed to some other pre-defined graph?)