# Subscpace Methods for Dim Reduction

#dim #features

Parent: [[04_Features]]

Local methods (like KNN) break down in high-dimensions to to the **curse of dimensionality** ([[curse_dim]]): it becomes impossible to find a good neighborhood to do local averaging. Instead, let's define a subspace, and assign vectors to classes to minimize the distance between these vectors and this subspace. Usually distance = acos cosine similarity = acos (v∙proj(v))/v² = acos vPv/v², where P is a projection matrix. PCA is one way to achieve it, but not the only one. But in all cases, the idea is that (Signals) = (Basis)∙(Scores), where Scores can be interpreted as representations.

Some popular approaches:

* **PCA** [[pca]] (principal component anlaysis) - optimal reconstruction (minimal reconstruction error, given a limit on basis size), maximal variance of projections. Eigenspace of XᵀX
* **LDA** [[lda]] (Linear Discriminant Analysis) - optimal separation of classes. At each step, maximize separation (distance) between classes, aka **Fisher Linear Discriminance**. Obviously, label-driven, and these labels are nominal.
* **CCA** (Canonical Correlation Analysis) - in a 1D case, finds a mix of X that is maximally correlated with a given output value y (similar to LDA for labels, but for numerical y). More generally, for two high-D datasets X and Y, finds directions (projections to low-D) in both X and Y such that the correlation between these directions is maximal. In an even more general case, develop a sequence of these correspondences, defining an "optimal rotation" from one space to another.
* **ICA** (Independent Component Analysis) - makes the basis statistically independent. Can be considered an extension of PCA that goes beyound 2nd order statistics (_What does it mean?_ #todo)
* **NMF** (Nonnegative Matrix Factorization) - keeps all elements in the basis non-negative, which often aids interpretability. Is often used with constraints (e.g. "orthogonality of weights")

# Refs

* [A presentation with a list of approaches](https://www.cc.gatech.edu/~hic/8803-Fall-09/slides/SubSpace-Learning.pdf)	, by Horst Bischof and Ales Leonardis

https://en.wikipedia.org/wiki/Canonical_correlation