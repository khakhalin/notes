# PCA: Principal Component Analysis

Parents: [[04_Features]]
Related: [[svg]], [[pca_regression]], [[pagerank]]

#linalg #stats


Simplest type of eigenvalue-based (spectral) factor analysis. X=TΛQᵀ, where X is the original data, T is a set of orthogonal signals (**scores** - like in how much each point scored, therefore dim scores = dim points), and ΛQ is a p×p matrix of weigts that transform basis T into the basis of original data, called **loadings** (like, how hidden factors load into the observed variables, thus dim loadings = p×p, as for basis transformation).

**Intuitions for the matrix form**: covariance matrix C = Cov(X) = XᵀX/N is symmetric, so it can be spectrally decomposed not just in PDP⁻¹, but in QDQᵀ, where Q is orthonormal. Rotation, followed by independent scaling of each coordinate, followed by backwards rotation.

It means that if you get p independent normal variables into a matrix Z (this matrix would have Cov(Z) = E (ZᵀZ)/N=I), but scale them by Λ by multiplying by Λ on the right (where Λ_i=sqrt(D_i)), and then rotate with Qᵀ, you'll get data Y=ZΛQᵀ with the same expected Covariance matrix as the original data: C' = YᵀY/N = (ZΛQᵀ)ᵀ(ZΛQᵀ)∙1/N = QΛᵀZZΛQᵀ∙1/N = QΛIΛQᵀ = QDQᵀ. In other words, if our data Y happens to be made from Z by scaling with D, followed by rotation, then eigen-value-decomposition of Cov matrix into QDQᵀ helps to recover these scaling factors Λ=sqrt(D), as well as the rotation Q.

Now the cool thing about Q is that the following is true: Cov(X)Q = QDQ⁻¹Q = QD, or in other words each q_i gets scaled by D_ii. Which means that columns of Q are eigenvectors of Cov(X), and so of XᵀX as well!

Further, as Qᵀ rotates ZΛ towards X (as in X=ZΛQᵀ), the rows of Qᵀ are the directions of axes of ellipsoid ZΛ in new coordinates of X. (If you multiply an index vector by Qᵀ, as in (1,0,...,0)Qᵀ,  you'll just get the first row of Qᵀ, etc.) But rows of Qᵀ are also columns of Q. Therefore, the columns of Q, that we had just shown above to be the eigenvectors of Cov(X), also point along axes of the ellipsoid of X.

Why projecting to these ellipsoid-aligned axes **maximizes the variance along this projection**? (Which is in fact the most commonly used definition of PCA that I just haven't mentioned yet). Let's look for the first axis, maximizing the variance of projection on this axis. Consider all unitary vectors w (unitary ⇒ wᵀw =1), and find one that captures highest variance. A vector of projections of X (dimensions N×p) on w (p×1) is Xw (N×1). The variance of this projection is (Xw)ᵀ(Xw) / N = wᵀXᵀXw ∙ 1/N = wᵀCw. We want to maximize wᵀCw with a constraint of wᵀw=1, which is achieved via Lagrange multiplier λ and unconstrained optimization for wᵀCw - λ(wᵀw-1). Differentiate by w (by each coordinate separately, but then written in a vector form), set it to be equal to zero-vector (one vector equation = a system of scalar equations): 2Cw - 2λw = 0. From that we see that Cw = λw, and so w that maximizes variance of projectsions of X on it, happens to be an einvector of C. From this example with one vector, using a greedy procedure, we can build the entire basis, kinda by induction. Refs: [cmu](https://www.stat.cmu.edu/~cshalizi/uADA/12/lectures/ch18.pdf)

**How to calculate PCA in practice?** Iteratively:

1. Use **power iteration** to find eigenvector e_i matching the largest eigenvalue λ_i ( see [[pagerank]] for details )
2. Project everything to the space ⟂ to e_i
3. Repeat

# Rotations

> What is the motivation for rotations? Only interpretability, or something else?

**Varimax rotation**: orthogonal rotation of PCA results (so components stay orthogonal) that maximizes the share of near-zero coordinates (scores) for points. It tries to rotate points (scores) to the axes, and away from "innards" of each quadrant. Makes points commit to "mostly one axis", instead of being torn between many axes.

**Promax rotation**: similar to varimax, but oblique (with shears, making components non-orthogonal), which allows higher contrasting, and is also cheaper computationally.

# In Python

Archetypical use of pca in [[python]] / [[numpy]]. Input is a matrix, where every row is a value, and each column is a coordinate.

```python
import sklearn.decomposition
pca = sklearn.decomposition.PCA(n_components=2)
pca.fit(X)
scores = pca.transform(X)
loadings = pca.inverse_transform(np.eye(2)) # This command is somehow really not obvious from the manual
```

# Refs

Intuitions behind PCA (a great write-up by Peter Bloem): https://peterbloem.nl/blog/

A very nice practical introduction to PCA in Python: https://jakevdp.github.io/PythonDataScienceHandbook/05.09-principal-component-analysis.html