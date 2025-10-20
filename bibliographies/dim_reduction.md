# Dimensionality Reduction and manifold learning

Parent: [[features]]
See also: [[clustering]]

#bib #dim


Topics and methods:
* [[subspace_methods]] - linear; finding a projection to an optimal subspace
    * [[pca]] - Principle Component Analysis - the mother of all
    * [[lda]] - Linear Discriminant Analysis - classification-driven

Local linear methods: 
* lle - Local Linear Embedding
* [[mds]] - spectral embedding
* [[eigenmaps]] - Laplacian Eigenmaps

Non-linear methods:
* [[isomap]] - first popular non-linear method: geodesic distances followed by mds
* [[tsne]] - aka t-SNE; probability of connectedness as a function of distance
* [[umap]] - UMAP, simplicies, tricky way to create a graph, easier parametric control
    * Umap vs tSNE #todo: https://pair-code.github.io/understanding-umap/

Related concepts:
* [[metric]] - different ways of calculating distances
* [[curse_dim]] - dimensionality curse
* [[johnson-lindenstrauss]] - an important lemma about embedding in lower-D

# To-Read

Diffusion Maps: https://en.wikipedia.org/wiki/Diffusion_map

Sammon Mapping: https://en.wikipedia.org/wiki/Sammon_mapping