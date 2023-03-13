# Linear separation

#classification

Parents: [[03_Classification]] / [[logreg]], [[04_Features]]
Related: [[lda]], [[svm]]

A preliminary step (both conceptual, and, in some cases, practical) before fancier kinds of classification (like logistic regression, for example).



Can linear regression be used for classification? Aye, just set a threshold (0.5) for the output value. Aka fitting a **dummy variable**. If h(x) is a hyperplane in (x,y), h=const becomes a 1-d-lower hyperplane (aka **decision boundary**; in 2D case: a line) separating the space of X into 2 parts. This would be called **Linear separation**, and it works reasonably well if all you need to do is to separate 2 partially overlapping Gaussians.

What about **multi-class classificaton**, can regression be used then for it? Kinda, but we'll have to use a different loss (not L2), but rather with sum up the costs of all misclassifications (when a point from class $G_k$ is erroneously classified as a point in $G_l$). We'll need a matrix of costs, and from it calculate a matrix of losses $L(k,l)$. Most often one cost is used for all errors, which is called the **Expected Prediction Error** EPE = E(L). We can try to optimize EPE point-wise: G = armin_g ∑ L(Gk counted as g) ∙ P(observing point from Gk | for X=x).

If all errors cost the same, G = argmin_g (1 - P(g | x)), known as likelihood optimization, or **Bayes classifier**: just go for the best guess of conditional probability P(class | observation). The error rate for it is called **Bayes rate**, and it is the lowest possible rate, coming from the variability of data itself, analogous to irreducible error for continuous data: [wiki](https://en.wikipedia.org/wiki/Bayes_error_rate). If data is deterministic (every x↔g), Bayes rate=0, but if same x can produce diff g, it's ≠0, because the model will never be able to perfectly overfit the data.

**Kernel tricks**: to perform separation in of non-linear data, or data in high D, add feature squares and cross-products into the set, as synthetic features (see [[04_Features]]). Then perform separation. Linear boundaries will transform to quadratic boundaries.

If each class dummy variable is fit with f_i = Xθ_i, then the decision boundary between 2 classes is a line f_i = f_j, and so X(θ_i - θ_j)=0 is also a line.  So we can take **indicator matrix** Y (each row is a class, one-hot encoded), and try fit each column of it using linear regression: H = XΘ = X(XᵀX)⁻¹XᵀY, where Θ is a matrix composed of many columns θ: one for every column in Y. This H would have a reasonable property of ∑f_k(x) = 1, when summing across all classes g_k, but individual f_k are linear, and so totally go outside of (0,1). This may be a problem.

Another problem is **class masking**: imagine 3 separable classes ABC lying roughly on a line. Decision boundaries between AB and BC will be roughly parallel, but when taking together, 3 linear functions will form only one boundary somewhere in the middle, leaving class B completely unresolved. With a quadratic regression one can resolve 3 classes, but cannot resolve 4, etc. To resolve more classes, one has to transform regression values with "increasingly non-linear" functions F(Xθ_k): they should be monotonous and fall down fast enough to offer fine class resolution. refs: [1](https://stats.stackexchange.com/questions/43867/why-does-the-least-square-solution-give-poor-results-in-this-case)