# Features, Dim reduction, Clustering

#features

Subtopics:
* [[variable_selection]] - selecting and constructing features
* [[linear_separation]] - how to pick a good coordinate system for classification
* [[basis_expansion]] - projecting to a meaningful basis can help
* [[smoothing]] - as a type of feature transformation
* [[clustering]] - methods in data clustering
* [[fairness]] - on the practical choice and interpretation of features

**Dim Reduction and Manifold Learning**

#todo: https://jakevdp.github.io/PythonDataScienceHandbook/05.10-manifold-learning.html

* [[subspace_methods]] - linear; finding a projection to an optimal subspace
    * [[pca]] - Principle Component Analysis - the mother of all
    * [[lda]] - Linear Discriminant Analysis - classification-driven
* lle - Local Linear Embedding
* [[mds]] - spectral embedding
* Laplacian Eigenmaps
    * https://www.cs.cmu.edu/~aarti/Class/10701/slides/Lecture21_1.pdf
* [[isomap]] - first popular non-linear method: geodesic distances followed by mds
* Diffusion Maps
    * https://en.wikipedia.org/wiki/Diffusion_map
* Sammon Mapping
    * https://en.wikipedia.org/wiki/Sammon_mapping
* [[tsne]] - aka t-SNE; probability of connectedness as a function of distance
* [[umap]] - UMAP, simplicies, tricky way to create a graph, easier parametric control
    * Umap vs tSNE #todo: https://pair-code.github.io/understanding-umap/

Related concepts:
* [[metric]] - different ways of calculating distances
* [[curse_dim]] - Dimensionality curse

# Refs

### Manifold learning

Good summary:
https://jakevdp.github.io/PythonDataScienceHandbook/05.10-manifold-learning.html

### Variable selection

[[Guyon2003variable]] - review on feature selection and construction