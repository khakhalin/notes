Features, Dim reduction, Clustering
===
#features

# Linear Basis Expansion
The idea is to go beyond "linear" in linear regression, but retain the spirit of replacing complex and noisy X with some simple, probably smooth functions of fewer parameters.

f(X) = ∑βj hj(X), 
where X is our data, β are coefficients, and {h_j(X)} is a set of functions (transformations), each projecting ℝ^p → ℝ. So a non-linear transformation of X, followed by a linear model in these new coordinates.

> So, this shows that fitting and smoothing can also be considered a case of basis transformations, where you have from a very high dimensionality (lots of consecutive points in a signal, treated as one long vector) into a low dim of parameter space (say, spline coefficients), from which the original high-D signal may be approximately reconstructed.

**Some examples:**
* h_j(X) = x_j. Linear regression
* h_j(X) = x_j∘x_i (potentially, for all p(p-1) pairs of coordinates, including squares). **Cross-products**, aka **polynomial kernel** or **polynimal expansion**. Cheap and fast way to perform non-linear modeling with linear methods. Higher polynomials are also possible, but computationally expensive. 
* h_j(X) = indicator(x ∈ area): **binning** in p-dimensions. Emulates cluster analysis without actually running cluster analysis, as clusters are more likely to be covered by the same bin. Can also be used to transform a continuous variable into a pseudo-nominal one.
* **Kernel tricks**: use various functions (kernels) to calculate new synthetic features from old features. For example, distance from a well chosen point in a high-D space would make a useful kernel (see [[RBF]]).

The biggest problems with polynomials is that they are global (non-local), cannot extrapolate, and are unstable. Some alternative that combine mathematical simplicity with good behavior: **piecewise-polynomials**, **splines**, and **wavelets**. All these methods produce a very large **dictionary** of basis functions, and so need further constraints to keep the complexity of the model in check, such as **restriction** (e.g. always the same number of terms in a sum, for addittive functions), **selection** (only retain functions with large enough coefficients), or **regularization**.

## Splines
Consider **Piecewise polynomials**. The simplest example ever: downsampling, which is the same as piecewise constant, or just replacing values with their means. Better: piecewise linear fit. Even better: piecewise linear continuous at knots. Continuity implies limitations (as in a+bx = c+dx), and so a lower number of parameters to fit.  For **Splines**, not only f(x), but also first derivative f'(x) are continuous on the edges of segments. Usually cubic, but not necessarily. There's apparently an efficient way to calculate them, called a **B-spline basis**, but park for now. (ESL p144, referencing Esl chapter 5 appendix) 

**Natural cubic spline**: is forced to become linear on all knots (with f''=0), with no curvature. Three benefits: 1) same polynomial can be safely extrapolated outside the range, as it fits edge data with a reasonable straight line (instead of a crazy overshoot that cubic splines always want to do), 2) two fewer degrees of freedom per spline to deal with, 3) arguably, look quite nice. Formulas may be solved explicitly, it is just annoying. [ref](https://towardsdatascience.com/numerical-interpolation-natural-cubic-spline-52c1157b98ac)

## Smoothing splines
Instead of picking knot points (for splines), let's use regularization. Loss = ∑(y-f(x))² + λ∫(f''(x))²dt, where λ is a **smoothing parameter**. Then apply it to a natural spline sequence with knots in unique values of x_i (that apparently happens to be an optimal function for this type of task). Apparently, there also exists an optimal basis of natural splines F (B-splines? _Or are B-splines different?_), so f = ∑Fi(x)θi, and after combining it with the equation fo loss, we get a generalized ridge regression:

L(θ, λ) = (y-Fθ)ᵀ(y-Fθ) + λθᵀΩθ, where Ω is a matrix with $Ω_{jk} = \int F'' _ j(t)F''_ k(t)dt$. It gives the solution: θ = (FᵀF + λΩ)⁻¹Fᵀy. There exist computationally effective way to calculate that all. 

An approximation for y can now be produced from here, by doing h = Fθ = F(FᵀF + λΩ)⁻¹Fᵀy = Sy, where S is a **smoother matrix**, or smoother operator. Linear in respect to y. This matrix S = F(FᵀF + λΩ)⁻¹Fᵀ is similar to a standard projection operator B(BᵀB)⁻¹Bᵀ; it's also symmetric positive semidefinite, but while H² = H, S² ≤ S (because of the shrinkage). As for a (reasonable?) projection operator tr(H) = the number of coordinates remaining in place = dimention of the projection space, we can use df$_ λ$= tr(S) an estimate for the **effective degrees of freedom**.

Another way to write S is to split it in a so-called **Reinsch form**: S = (I+λK)⁻¹, where K is not depended on λ, and is called a **penalty matrix**. (_I'm not immediately sure why it is possible._) The igenvectors can be found (ESL p155), and they don't depend on λ. The **Bias-Variance Tradeoff** is quite prominent in this case (ESL p159)

> #todo : ESL p161 to p181 skipped for now: Nonparametric Logistic Regression, Multididimensional splines, Reproducing Kernel Hilbert Spaces (RKHS), 

## Wavelets

> Also parked for now: Wavelet smoothing (around ESL p170)

# Kernel Smoothing Method
> ESL 192

# Variable selection
Key references:
* [[Guyon2003variable]] - review on feature selection (as well as some info on construction). 
 
Basic checklist:
1. Use domain knowledge
2. Normalize variables (where appropriate)
3. If variables aren't independent, construct **conjunctive features**
4. If need to prune for external reasons (compute?) create **disjunctive**, or weighted
5. Get baseline by assessing features independently
6. Detect and handle outliers and dirty data
7. Start with the simplest predictor (usually linear)
8. If you have better ideas, implement, then compare (benchmark)
9. Check stability by bootstrapping (cross-validation)

# Dimensionality reduction
Local methods (like KNN) break down in high-dimensions to to the **curse of dimensionality**: it becomes impossible to find a good neighborhood to do local averaging. In high-dim, volume grows so fast with linear dimension (r^K for K dim) that most of volume lies in the shell, not around the center. Say, to find a subcube with 1% of total volume inside a unit-cube in 10-dim, we'd have to take a subcube with a side of 0.6 (because 0.6^10 ≈ 0.01). Meaning that this neighborhood won't be local in any meaningful set (it will span 0.6 of total range for each of the dimensions!). And if we keep the side length (r) small, we won't capture any meaningful examples. Similarly, if points are distributed uniformly within a unit volume, the closest point to any given point is expected to be half-way to the boundary. Especially for highly non-linear functions (say, exponents) it presents a huge problem, as mean(neighbors) no longer resembles the "true value". (ESL p23-25)

## Subspace Methods
**Overview**: define subspaces and assign vectors to classes to minimize the distance between these vectors and these subspaces. Usually distance = acos cosine similarity = acos (v∙proj(v))/v² = acos vPv/v², where P is a projection matrix. PCA is one way to achieve it, but not the only one. But in all cases, the idea is that (Signals) = (Basis)∙(Scores), where Scores can be interpreted as representations.

Several possible possible approaches:
* **PCA** (principal component anlaysis) - optimal reconstruction (minimal reconstruction error, given a limit on basis size), maximal variance of projections. Eigenspace of XᵀX
* **LDA** (Linear Discriminant Analysis) - optimal separation of classes. At each step, maximize separation (distance) between classes, aka **Fisher Linear Discriminance**. Obviously, label-driven, and these labels are nominal. Refs: [wiki](https://en.wikipedia.org/wiki/Linear_discriminant_analysis); detailed descriptoin in [[03_Classification]].
* **CCA** (Canonical Correlation Analysis) - in 1D case, finds a mix of X, optimally correlated with a given output value y (similar to LDA for labels, but for continuous y). More generally, for two high-D datasets X and Y, finds directions (projections to low-D) in both X and Y such that the correlation between these directions is maximal. In even more general case, develop a sequence of these correspondences, defining an "optimal rotation" from one space to another. Refs: [wiki](https://en.wikipedia.org/wiki/Canonical_correlation)
* **ICA** (Independent Component Analysis) - makes the basis statistically independent. Can be considered an extension of PCA that goes beyound 2nd order statistics.
* **NMF** (Non-negative Matrix Factorization) - keeps all elements in the basis non-negative, which often aids interpretability.

Refs:
* [A lecture with a list of approaches](https://www.cc.gatech.edu/~hic/8803-Fall-09/slides/SubSpace-Learning.pdf)	, by Horst Bischof and Ales Leonardis

## PCA
Simplest type of eigenvalue-based (spectral) factor analysis. X=TΛQᵀ, where X is the original data, T is a set of orthogonal signals (**scores** - like in how much each point scored, therefore dim scores = dim points), and ΛQ is a p×p matrix of weigts that transform basis T into the basis of original data, called **loadings** (like, how hidden factors load into the observed variables, thus dim loadings = p×p, as for basis transformation).

**Intuitions for the matrix form**: covariance matrix C = Cov(X) = XᵀX/N is symmetric, so it can be spectrally decomposed not just in PDP⁻¹, but in QDQᵀ, where Q is orthonormal. Essentially, rotation, followed by independent scaling, followed by backwards rotation. If I get it right, it means that if you get p independent normal variables into a matrix Z (such that Cov(Z) = E (ZᵀZ)/N=I), scale them by Λ (where Λi=sqrt(Di)) by multiplying by Λ on the right, and then rotate by Qᵀ, you'll get data Y=ZΛQᵀ with the same expected Cov as the original data: C' = YᵀY/N = (ZΛQᵀ)ᵀ(ZΛQᵀ)∙1/N = QΛᵀZZΛQᵀ∙1/N = QΛIΛQᵀ = QDQᵀ. In other words, if our data Y is made of X by scaling followed by rotation, then eigenvaluedecomposition of Cov matrix into QDQᵀ helps to recover this scaling (Λ=sqrt(D)) and rotation Q.

Cov(X)Q = QDQ⁻¹Q = QD = each q_i scaled by d_ii, ⇒ columns of Q are eigenvectors of Cov(X) and XᵀX.

And as Q rotates TΛ towards X (in X=TΛQᵀ), the rows of Qᵀ are the directions of old axes of ellipsoid TΛ in new coordinates of X. (If you do (1,0,...,0)Qᵀ,  you'll just get the first row of Qᵀ, etc.) But rows of Qᵀ are columns of Q. Therefore, the columns of Q, that we had just shown above to be the eigenvectors of Cov(X), point along axes of the ellipsoid of X.

Why do these axes maximize the variance along them? (Which is the usual definition of PCA, but something we haven't mentioned yet). Let's look for the first axis first. For that, let's consider all unitary vectors w (unitary ⇒ wᵀw =1), and find the one that captures highest variance. A vector of projections of X (N×p) on w (p×1) is Xw (N×1). The variance of it is (Xw)ᵀ(Xw) = wᵀXᵀXw ∙ 1/N = wᵀCw. We want to maximize wᵀCw with a constraint of wᵀw=1, which is achieved via Largange multiplier λ and unconstrained optimization for wᵀCw - λ(wᵀw-1). Differentiate by w, set to 0: ge 2Cw - 2λw = 0; differentiate by λ and set to 0: wᵀw = 1. From that we see that Cw = λw, and so w that maximizes variance of projectsions of X on it is an einvector of C. ([ref](https://www.stat.cmu.edu/~cshalizi/uADA/12/lectures/ch18.pdf)) From this example with one vector, using a greedy procedure, we can build the entire basis, kinda by induction.

**Varimax rotation**: orthogonal rotation of PCA results (so components stay orthogonal) that maximizes the share of near-zero coordinates (scores) for points. It tries to rotate points (scores) to the axes, and away from "innards" of each quadrant. Makes points commit to "mostly one axis", instead of being torn between many axes.

**Promax rotation**: similar to varimax, but oblique (with shears, making components non-orthogonal), which allows higher contrasting, and is also cheaper computationally.

## tSNE
"The art of using tSNE" from Nature:
[https://www.nature.com/articles/s41467-019-13056-x](<https://www.nature.com/articles/s41467-019-13056-x>)

## UMAP
ToRead:
* [https://pair-code.github.io/understanding-umap/](<https://pair-code.github.io/understanding-umap/>)
* [https://towardsdatascience.com/how-exactly-umap-works-13e3040e1668](<https://towardsdatascience.com/how-exactly-umap-works-13e3040e1668>)
* [https://towardsdatascience.com/how-to-program-umap-from-scratch-e6eff67f55fe](<https://towardsdatascience.com/how-to-program-umap-from-scratch-e6eff67f55fe>)

# Clustering

# Very High Dimensions
For high dimensions (say, word counts in a document) simple Eucledian distance doesn't work that well, as it becomes impossible to compare long documents to short documents. One way would be to normalize vectors before calculating Eucledian distance, but there's an easier way: use **cosine similarity** (described above). It sorta auto-normalizes lengths, is easier to calculate, and ordering of proximity is the same for normalized Eucledian and cosine similarity. (_unproved here, but apparently it's the case, hmm_)

**Specifics of working with very high dimensions:**
(Genomic data in this case)
* https://towardsdatascience.com/no-true-effects-in-high-dimensions-1f56360182cd
* https://towardsdatascience.com/pitfalls-of-data-normalization-bf05d65f1f4c
* https://towardsdatascience.com/how-to-cluster-in-high-dimensions-4ef693bacc6