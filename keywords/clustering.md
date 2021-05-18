# Clustering

#bib #clustering #features

Parents: [[04_Features]]

Methods:
* [[spectral_clustering]]
* [[mean_shift]]

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