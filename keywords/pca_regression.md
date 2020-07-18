# Principal Components Regression

#stats

Parents: [[02_Regression]]
See also: pca

1. Standardaze columns of X.
2. Calculate **PCA** of X: Z=XV, where columns of Z are orthonormal.
3. Regress y on Z: Zξ~y. As z_i are orthonormal, it's like running p univariate regressions.
4. Recalculate θ from ξ: θ = Vξ, but only using first m biggest eigenvectors.

Similar to Shrinkage, in the sence that Shrinkage also ends up shrinking components with small eigenvalues more. With the principal components regression, we just zero them altogether. Unlike ridge, it can amplify "unworthy" coefficients in the original X space though, what would have been shrunk by both Ridge and Lasso.

# Partial Least Squares

1. Standardize columns of X
2. Start cycle i=1
3. Calculate φij = ⟨x_j , y⟩ for each i. Synthesize z_i = ∑j φij xj
4. Regress (project) y by z_i, get ξ_i.
5. Switch from y to residual (y-(ξi z_i) = res: res ⊥ z_i)
6. Orthogonalize all x_j to z_i (simiarly, project to ⊥ z_i)
7. Repeat 3-6 m times (m<p)
8. Project ξ back to θ

The idea behind is to let y pull signal from X, regardless of how this signal is encoded (mixed?) in columns of X. ESL claims though that "variance aspect tends to dominate", so in practice this method doesn't behave too differently than Ridge regression. Seems to also be similar in spirit to "Canonical Correlation Analysis" (see [[04_Features]] section). Same as PCR above, may inflate weaker dimensions, making it unstable. Which all together is probably why it is not that widely used? 

# Refs

ESL p79, p81

[wiki](https://en.wikipedia.org/wiki/Partial_least_squares_regression)