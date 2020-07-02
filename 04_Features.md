# Features, Dim reduction, Clustering

#features

Subtopics:
* [[clustering]]
* [[bib_fairness]] - on practical choice and interpretation of features
* [[smoothing]] - as a type of feature transformation

Dim Reduction
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
* **PCA** (principal component anlaysis) - optimal reconstruction (minimal reconstruction error, given a limit on basis size), maximal variance of projections. Eigenspace of XᵀX
* **LDA** (Linear Discriminant Analysis) - optimal separation of classes. At each step, maximize separation (distance) between classes, aka **Fisher Linear Discriminance**. Obviously, label-driven, and these labels are nominal. Refs: [wiki](https://en.wikipedia.org/wiki/Linear_discriminant_analysis); detailed descriptoin in [[03_Classification]].
* **CCA** (Canonical Correlation Analysis) - in 1D case, finds a mix of X, optimally correlated with a given output value y (similar to LDA for labels, but for continuous y). More generally, for two high-D datasets X and Y, finds directions (projections to low-D) in both X and Y such that the correlation between these directions is maximal. In even more general case, develop a sequence of these correspondences, defining an "optimal rotation" from one space to another. Refs: [wiki](https://en.wikipedia.org/wiki/Canonical_correlation)
* **ICA** (Independent Component Analysis) - makes the basis statistically independent. Can be considered an extension of PCA that goes beyound 2nd order statistics (_What does it mean?_ #todo)
* **NMF** (Nonnegative Matrix Factorization) - keeps all elements in the basis non-negative, which often aids interpretability. Is often used with constraints (e.g. "orthogonality of weights")

Refs:
* [A presentation with a list of approaches](https://www.cc.gatech.edu/~hic/8803-Fall-09/slides/SubSpace-Learning.pdf)	, by Horst Bischof and Ales Leonardis

## PCA
Simplest type of eigenvalue-based (spectral) factor analysis. X=TΛQᵀ, where X is the original data, T is a set of orthogonal signals (**scores** - like in how much each point scored, therefore dim scores = dim points), and ΛQ is a p×p matrix of weigts that transform basis T into the basis of original data, called **loadings** (like, how hidden factors load into the observed variables, thus dim loadings = p×p, as for basis transformation).

**Intuitions for the matrix form**: covariance matrix C = Cov(X) = XᵀX/N is symmetric, so it can be spectrally decomposed not just in PDP⁻¹, but in QDQᵀ, where Q is orthonormal. Rotation, followed by independent scaling of each coordinate, followed by backwards rotation. It means that if you get p independent normal variables into a matrix Z (this matrix would have Cov(Z) = E (ZᵀZ)/N=I), but scale them by Λ by multiplying by Λ on the right (where Λ_i=sqrt(D_i)), and then rotate with Qᵀ, you'll get data Y=ZΛQᵀ with the same expected Cov as the original data: C' = YᵀY/N = (ZΛQᵀ)ᵀ(ZΛQᵀ)∙1/N = QΛᵀZZΛQᵀ∙1/N = QΛIΛQᵀ = QDQᵀ. In other words, if our data Y happens to be made from Z by scaling with D followed by rotation, then eigen-value-decomposition of Cov matrix into QDQᵀ helps to recover these scaling factors (Λ=sqrt(D)), as well as the rotation Q.

Now the cool thing about Q is that the following is true: Cov(X)Q = QDQ⁻¹Q = QD, or in other words each q_i gets scaled by D_ii. Which means that columns of Q are eigenvectors of Cov(X), and so of XᵀX as well.

Further, as Qᵀ rotates ZΛ towards X (as in X=ZΛQᵀ), the rows of Qᵀ are the directions of axes of ellipsoid ZΛ in new coordinates of X. (If you multiply an index vector by Qᵀ, as in (1,0,...,0)Qᵀ,  you'll just get the first row of Qᵀ, etc.) But rows of Qᵀ are also columns of Q. Therefore, the columns of Q, that we had just shown above to be the eigenvectors of Cov(X), also point along axes of the ellipsoid of X.

Why projecting to these ellipsoid-aligned axes maximizes the variance along this projection? (Which is the usual definition of PCA that I haven't mentioned yet). Let's look for the first axis, maximizing the variance of projection on this axis. Consider all unitary vectors w (unitary ⇒ wᵀw =1), and find one that captures highest variance. A vector of projections of X (N×p) on w (p×1) is Xw (N×1). The variance of this projection is (Xw)ᵀ(Xw) / N = wᵀXᵀXw ∙ 1/N = wᵀCw. We want to maximize wᵀCw with a constraint of wᵀw=1, which is achieved via Largange multiplier λ and unconstrained optimization for wᵀCw - λ(wᵀw-1). Differentiate by w (by each coordinate separately, but then written in a vector form), set it to be equal to zero-vector (one vector equation = a system of scalar equations): 2Cw - 2λw = 0. From that we see that Cw = λw, and so w that maximizes variance of projectsions of X on it, happens to be an einvector of C. ([ref](https://www.stat.cmu.edu/~cshalizi/uADA/12/lectures/ch18.pdf)) From this example with one vector, using a greedy procedure, we can build the entire basis, kinda by induction.

**Varimax rotation**: orthogonal rotation of PCA results (so components stay orthogonal) that maximizes the share of near-zero coordinates (scores) for points. It tries to rotate points (scores) to the axes, and away from "innards" of each quadrant. Makes points commit to "mostly one axis", instead of being torn between many axes.

**Promax rotation**: similar to varimax, but oblique (with shears, making components non-orthogonal), which allows higher contrasting, and is also cheaper computationally.