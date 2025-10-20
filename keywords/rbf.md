# RBF = Radial Basis Function

Parents: [[kernels]], [[regression]], [[classification]]
See also: [[loess]]

#features #classification


The idea is to go from a Cartesian space X to a space defined by each point's proximity to a set of archetypical origin points ${μ}$. So for each point $x_i$ , instead of its original Cartesian coordinates, we'll use a new set of values $\{K\}_i$ , quantifying its distance from this points (typically not a linear distance, but something like $K_{ij} = \exp (-γ \cdot \text{dist}(x_i, μ_j))$. Then you can also optimize the set of {μ}; and γ becomes a hyperparameter. And if you're doing a classification, you also still need to optimize decision boundaries using some classification method (such as SVM, for example), but now in this new K-space.

So this seems to entail like 3 nested optimization problems: find optimal decision boundary (SVM?), assuming a set of points (μ), then repeat it again and again while looking for a best set of points {µ}. And then finally, one can also challenge the hyperparameter $γ$, and consider finding the best one.

# Refs

https://en.wikipedia.org/wiki/Radial_basis_function_kernel

On their use with SVMs: a blog post by Ajay Yadav
https://towardsdatascience.com/support-vector-machines-svm-c9ef22815589
