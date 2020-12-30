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
* [[subspace_methods]] - linear; finding a projection to an optimal subspace
    * [[pca]] - Principle Component Analysis - the mother of all
    * [[lda]] - Linear Discriminant Analysis - classification-driven
* lle - Local Linear Embedding
* isomap
* Laplacian Eigenmaps
* Diffusion Maps
* MDS
* Sammon Mapping
* [[tsne]] - aka t-SNE
* [[umap]] - UMAP, simplicies, algebraic topology

Related concepts:
* [[metric]] - different ways of calculating distances
* [[curse_dim]] - Dimensionality curse

# Refs

### Manifold learning

Good summary:
https://jakevdp.github.io/PythonDataScienceHandbook/05.10-manifold-learning.html

### Variable selection

[[Guyon2003variable]] - review on feature selection and construction