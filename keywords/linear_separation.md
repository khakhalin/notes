# Linear separation

Parents: [[03_Classification]], [[04_Features]]
Related: [[lda]], [[svm]], [[logreg]]

#classification


A preliminary step (both conceptual, and, in some cases, practical) before fancier kinds of classification (like logistic regression [[logreg]], for example).

Can linear regression be used for classification? In some sense, yes: we can just set a threshold (0.5) for the output value. Aka fitting a **dummy variable** (class indicator) to the dataset X. If h(x) is a hyperplane in (x,y), then h=const becomes a 1-d-lower hyperplane (aka **decision boundary**, such as a line in 2D case), separating the space of X into 2 parts. This would be called **Linear separation**, and it works reasonably well if all you need to do is to separate 2 partially overlapping Gaussians.

What about **multi-class classificaton**, can regression be used then for it? Kinda, but we'll have to use a different loss (not L2, as in normal [[linear_regression]]), but rather sum up the costs of all misclassifications (when a point from class $G_k$ is erroneously classified as a point in $G_l$). We'll need a matrix of costs, and from it to calculate a matrix of losses $L(k,l)$. Most often one cost funciton is used for all errors, which is called the **Expected Prediction Error** EPE = E(L). We can try to optimize EPE point-wise: 

G = $\text{argmin}_g ∑$ L($G_k$ counted as g) ∙ P(observing a point from $G_k$ | for X=x).

If all errors costs are the same, then G = $\text{argmin}_g (1 - P(g | x))$, known as likelihood optimization, or **Bayes classifier**. In this case we just calculate conditional probabilities of $P(g|x)$, or the probability of the data point being originating from the class g, given its observation x, and then go with the highest probability. The error rate for this scenario is called **Bayes rate**, and it is the lowest possible rate, coming from the uncertainty of data itself, analogous to the irreducible error for [[regression]]. If data is deterministic or points are never overlapping, and $x↔g$, then the Bayes rate=0. If however the same observatoin x can be produced by different classes g, then the Bayes rate is ≠ 0, as the model will never be able to perfectly overfit the data.

**Kernel tricks**: to perform separation in of non-linear data, or data in high D, add feature squares and cross-products to the set, as synthetic features (see [[04_Features]]). Then perform separation. Linear boundaries for this derivative set will correspond to quadratic boundaries for the original set.

If each class dummy variable is fit with a linear function $f_i = Xθ_i$, then the decision boundary between 2 classes is a line $f_i = f_j$ (that is, a line where score-functions for belonging to both classes i an j will be the same). Simplifying, the solution of $X(θ_i - θ_j)=0$ has to be a line. So we can take an **indicator matrix** $Y$, in which each row is a one-hot encoded class indicator, and try to fit each column of this matrix using linear regression: $H = XΘ = X(XᵀX)⁻¹XᵀY$, where $Θ$ is a matrix composed of many columns of $θ$: one column for every column in $Y$. This $H$ would have a reasonable property of $∑f_k(x) = 1$, when summing across all classes $g_k$, but individual $f_k$ will be groing linearly with points moving away form decision boundaries, and will also totally go outside of $(0,1)$, so they can't be interpreted as probabilities. Which is sad.

Another problem is **class masking**: imagine 3 nicely separable Gaussian-ish classes A, B, and C, lying roughly on a line, one after another. Decision boundaries between AB and BC will be roughly parallel, but when taking together, 3 linear functions will form only one boundary somewhere in the middle, leaving class B completely unresolved. With a quadratic regression 🔥 we'll be able to resolve 3 classes, but won't be able to resolve 4, etc. To resolve more classes, we'll have to transform regression values with some  "increasingly non-linear" function $F(Xθ_k)$ that needs to be monotonous and fall down fast enough to offer fine class resolution. 

That's why logistics regresson ([[logreg]]) is so useful.

Footnotes:
* https://en.wikipedia.org/wiki/Bayes_error_rate
* https://stats.stackexchange.com/questions/43867/why-does-the-least-square-solution-give-poor-results-in-this-case)