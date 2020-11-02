# Features, Dim reduction, Clustering

#features

Subtopics:
* [[clustering]] - Methods in data clustering
* [[bib_fairness]] - on practical choice and interpretation of features
* [[smoothing]] - as a type of feature transformation

**Dim Reduction**
On dim reduction proper, see below.
On working with very high dimensions, see:
* [[curse_dim]] - Dimensionality curse
* [[umap]] - one key method of clustering
* [[tsne]] - t-SNE - another key method of clustering

# Linear Basis Expansion
The idea is to go beyond "linear" in linear regression, but retain the spirit of replacing complex and noisy X with a combination of simple, probably smooth functions of fewer parameters. $f(X) = ∑β_j h_j(X)$, where X is our data, β are coefficients, and {h_j(X)} is a set of functions (transformations), each projecting from X-like space to y-like values (ℝ^p → ℝ). So a non-linear transformation of X, followed by a linear model in these new coordinates.

> So, this shows that fitting and smoothing can also be considered basis transformations, where you go from a very high dimensionality (lots of consecutive points in a signal, treated as one long vector) into a low dim of parameter space (say, spline coefficients), from which the original high-D signal may be approximately reconstructed.

**Some examples:**
* h_j(X) = x_j. Linear regression
* h_j(X) = x_j∘x_i (**Cross-products**; potentially, for all p(p-1) pairs of coordinates, including self-products, aka squares). Aka **Polynomial kernel** or **polynimal expansion**. Cheap and fast way to perform non-linear modeling with linear methods. Higher polynomials are also possible, but computationally expensive. 
* h_j(X) = indicator(x ∈ area): **binning** in p-dimensions. Emulates cluster analysis without actually running cluster analysis, as clusters are more likely to be covered by the same bin. Can also be used to transform a continuous variable into a pseudo-nominal one.
* **Kernel tricks**: use various functions (kernels) to calculate new synthetic features from old features. For example, distance from a well chosen point in a high-D space would make a useful kernel (see [[RBF]]).

The biggest problems with polynomials is that they are global (non-local), cannot extrapolate, and are unstable. Some alternative that combine mathematical simplicity with good behavior: **piecewise-polynomials**, **splines**, and **wavelets**. All these methods produce a very large **dictionary** of basis functions, and so need further constraints to keep the complexity of the model in check, such as **restriction** (e.g. always the same number of terms in a sum, for addittive functions), **selection** (only retain functions with large enough coefficients), or **regularization**.

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

Refs:
* [[Guyon2003variable]] - review on feature selection (as well as some info on construction). 

# Dimensionality reduction methods

Local methods (like KNN) break down in high-dimensions to to the **curse of dimensionality** ([[curse_dim]]): it becomes impossible to find a good neighborhood to do local averaging.

## Subspace Methods

**Overview**: define subspaces and assign vectors to classes to minimize the distance between these vectors and these subspaces. Usually distance = acos cosine similarity = acos (v∙proj(v))/v² = acos vPv/v², where P is a projection matrix. PCA is one way to achieve it, but not the only one. But in all cases, the idea is that (Signals) = (Basis)∙(Scores), where Scores can be interpreted as representations.

Several possible possible approaches:
* **PCA** [[pca]] (principal component anlaysis) - optimal reconstruction (minimal reconstruction error, given a limit on basis size), maximal variance of projections. Eigenspace of XᵀX
* **LDA** (Linear Discriminant Analysis) - optimal separation of classes. At each step, maximize separation (distance) between classes, aka **Fisher Linear Discriminance**. Obviously, label-driven, and these labels are nominal. Refs: [wiki](https://en.wikipedia.org/wiki/Linear_discriminant_analysis); detailed descriptoin in [[03_Classification]].
* **CCA** (Canonical Correlation Analysis) - in 1D case, finds a mix of X, optimally correlated with a given output value y (similar to LDA for labels, but for continuous y). More generally, for two high-D datasets X and Y, finds directions (projections to low-D) in both X and Y such that the correlation between these directions is maximal. In even more general case, develop a sequence of these correspondences, defining an "optimal rotation" from one space to another. Refs: [wiki](https://en.wikipedia.org/wiki/Canonical_correlation)
* **ICA** (Independent Component Analysis) - makes the basis statistically independent. Can be considered an extension of PCA that goes beyound 2nd order statistics (_What does it mean?_ #todo)
* **NMF** (Nonnegative Matrix Factorization) - keeps all elements in the basis non-negative, which often aids interpretability. Is often used with constraints (e.g. "orthogonality of weights")

Refs:
* [A presentation with a list of approaches](https://www.cc.gatech.edu/~hic/8803-Fall-09/slides/SubSpace-Learning.pdf)	, by Horst Bischof and Ales Leonardis