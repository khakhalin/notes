# Features, Dim reduction, Clustering

#features

Subtopics:
* [[linear_separation]] - how to pick a good coordinate system for classification
* [[clustering]] - methods in data clustering
* [[basis_expansion]] - projecting to a meaningful basis can help
* [[smoothing]] - as a type of feature transformation
* [[fairness]] - on the practical choice and interpretation of features

**Dim Reduction methods**
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
* [[umap]] - based on algebraic topology
* See also:
    * [[curse_dim]] - Dimensionality curse

# Variable selection
 
Basic checklist for variable selection:
1. Use domain knowledge
2. Normalize variables (where appropriate)
3. If variables aren't independent, construct **conjunctive features**
4. If need to prune for external reasons (compute?) create **disjunctive**, or weighted
5. Get baseline by assessing features independently
6. Detect and handle outliers and dirty data
7. Start with the simplest predictor (usually linear)
8. If you have better ideas, implement, then compare (benchmark)
9. Check stability by bootstrapping (cross-validation)

* [[Guyon2003variable]] - review on feature selection (as well as some info on construction). 

# Refs

### Manifold learning

https://jakevdp.github.io/PythonDataScienceHandbook/05.10-manifold-learning.html