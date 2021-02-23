# Clustering

#bib #clustering #features

Parents: [[04_Features]]

Density-based clustering:
https://towardsdatascience.com/a-gentle-introduction-to-hdbscan-and-density-based-clustering-5fd79329c1e8

Methods:
* [[spectral_clustering]]
* [[mean_shift]]

# Refs

Priebe, C. E., Park, Y., Vogelstein, J. T., Conroy, J. M., Lyzinski, V., Tang, M., ... & Bridgeford, E. (2019). On a two-truths phenomenon in spectral graph clustering. Proceedings of the National Academy of Sciences, 116(13), 5995-6000. https://www.pnas.org/content/116/13/5995
It describes how two different types of spectral clustering [[spectral_clustering]] (Laplacian Spectral Embedding, aka "affinity", vs Adjacency Spectral Embedding, aka "core-periphery") give you two opposite answers on a simple horseshoe-like system (where two clusters are closer to each other in space, but are connected to each other only though a core structure). Both answers make sense in some context, but it's important to be aware of the context (essentially, they run on different graphs, right? Distance graph, as opposed to some other pre-defined graph?)