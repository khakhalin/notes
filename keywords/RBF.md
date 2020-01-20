# RBF = Radial Basis Function
#features #classification

The idea is to go from Cartesian space X to a space defined by each point's proximity to a set of archetypical origin points {μ}. So you define a new set of values parameters K, quantifying each point location, such as K_ij = exp(-γ ∙ dist(x_i, μ_j)). Then you optimize for {μ}, γ becomes a hyperparameter, and you also still have to optimize decision boundaries using some classification method (such as SVM, for example).

> So it feels like 3 nested optimization problems: find optimal decision boundary (SVM?), assujming a set of points (μ), that themselves are optimized given a certain γ.

Refs:
* [wiki](https://en.wikipedia.org/wiki/Radial_basis_function_kernel)
* On their use with SVMs: [a blog post by Ajay Yadav](https://towardsdatascience.com/support-vector-machines-svm-c9ef22815589)