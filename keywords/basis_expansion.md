# Linear Basis Expansion

#features

Parent: [[features]]


The idea is to go beyond "linear" in linear regression, but retain the spirit of replacing complex and noisy X with a combination of simple, probably smooth functions of fewer parameters. $f(X) = ∑β_j h_j(X)$, where X is our data, β are coefficients, and {h_j(X)} is a set of functions (transformations), each projecting from X-like space to y-like values (ℝ^p → ℝ). So a non-linear transformation of X, followed by a linear model in these new coordinates.

> So, this shows that fitting and smoothing can also be considered basis transformations, where you go from a very high dimensionality (lots of consecutive points in a signal, treated as one long vector) into a low dim of parameter space (say, spline coefficients), from which the original high-D signal may be approximately reconstructed.

**Some examples:**
* h_j(X) = x_j. Linear regression
* h_j(X) = x_j∘x_i (**Cross-products**; potentially, for all p(p-1) pairs of coordinates, including self-products, aka squares). Aka **Polynomial kernel** or **polynimal expansion**. Cheap and fast way to perform non-linear modeling with linear methods. Higher polynomials are also possible, but computationally expensive. 
* h_j(X) = indicator(x ∈ area): **binning** in p-dimensions. Emulates cluster analysis without actually running cluster analysis, as clusters are more likely to be covered by the same bin. Can also be used to transform a continuous variable into a pseudo-nominal one.
* **Kernel tricks**: use various functions (kernels) to calculate new synthetic features from old features. For example, distance from a well chosen point in a high-D space would make a useful kernel (see [[RBF]]).

The biggest problems with polynomials is that they are global (non-local), cannot extrapolate, and are unstable. Some alternative that combine mathematical simplicity with good behavior: **piecewise-polynomials**, **splines**, and **wavelets**. All these methods produce a very large **dictionary** of basis functions, and so need further constraints to keep the complexity of the model in check, such as **restriction** (e.g. always the same number of terms in a sum, for addittive functions), **selection** (only retain functions with large enough coefficients), or **regularization**.