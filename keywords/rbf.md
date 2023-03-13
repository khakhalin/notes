# RBF = Radial Basis Function

#features #classification

Parents: [[kernels]], [[02_Regression]], [[03_Classification]]
See also: [[loess]]

The idea is to go from a Cartesian space X to a space defined by each point's proximity to a set of archetypical origin points {μ}. So for each point x_i you define a new set of values $\{K\}_i$, quantifying its s position, for example K_ij = exp(-γ ∙ dist(x_i, μ_j)). Then you optimize the set of {μ}; γ becomes a hyperparameter, and if you're doing a classification, you also still have to optimize decision boundaries using some classification method (such as SVM, for example).

> So it feels like 3 nested optimization problems: find optimal decision boundary (SVM?), assujming a set of points (μ), that themselves are optimized given a certain γ.

Refs:
* [wiki](https://en.wikipedia.org/wiki/Radial_basis_function_kernel)
* On their use with SVMs: [a blog post by Ajay Yadav](https://towardsdatascience.com/support-vector-machines-svm-c9ef22815589)
