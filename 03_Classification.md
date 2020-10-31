# Classification

#classification

**Subtopics:**

Model assessment:
* [[validation]] - the process of
* [[confusion]] - confusion matrix and 
* [[roc]] - Receiver Operation Characteristic and AUC measure
    * [[youdens-j]] - a single measure to quantify ROC height

Methods and model types:
* [[knn]] - K Nearest Neighbors
* [[logreg]] - Logistic Regression
* [[svm]] - Support Vector Machine

# Linear separation

**Can regression be used for classification?** Aye, just set a threshold (0.5) for the output value. Aka fitting a **dummy variable**. If h(x) is a hyperplane in (x,y), h=const becomes a 1-d-lower hyperplane (aka **decision boundary**; in 2D case: a line) separating the space of X into 2 parts. **Linear separation**. Apparently, the best we can do for 2 overlapping Gaussians.

What about **multi-class classificaton**; can regression be used then? Also aye, but with a different loss (not L2), but rather with summing costs of all misclassifications (when a point from class Gk was erroneously classified as a point in Gl). We'll need a matrix of costs, and from it calculate a matrix of losses L(k,l). Most often though, one cost for all errors, and **Expected Prediction Error** EPE = E(L). We can try to optimize EPE point-wise: G = armin_g ∑ L(Gk counted as g) ∙ P(observing point from Gk | for X=x).

If all errors cost the same, G = argmin_g (1 - P(g | x)), known as likelihood optimization, or **Bayes classifier**: just go for the best guess of conditional probability P(class | observation). The error rate for it is called **Bayes rate**, and it is the lowest possible rate, coming from the variability of data itself, analogous to irreducible error for continuous data: [wiki](https://en.wikipedia.org/wiki/Bayes_error_rate). If data is deterministic (every x↔g), Bayes rate=0, but if same x can produce diff g, it's ≠0, because the model will never be able to perfectly overfit the data.

**Kernel tricks**: to perform separation in of non-linear data, or data in high D, add feature squares and cross-products into the set, as synthetic features (see [[04_Features]]). Then perform separation. Linear boundaries will transform to quadratic boundaries.

If each class dummy variable is fit with f_i = Xθ_i, then the decision boundary between 2 classes is a line f_i = f_j, and so X(θ_i - θ_j)=0 is also a line.  So we can take **indicator matrix** Y (each row is a class, one-hot encoded), and try fit each column of it using linear regression: H = XΘ = X(XᵀX)⁻¹XᵀY, where Θ is a matrix composed of many columns θ: one for every column in Y. This H would have a reasonable property of ∑f_k(x) = 1, when summing across all classes g_k, but individual f_k are linear, and so totally go outside of (0,1). This may be a problem.

Another problem is **class masking**: imagine 3 separable classes ABC lying roughly on a line. Decision boundaries between AB and BC will be roughly parallel, but when taking together, 3 linear functions will form only one boundary somewhere in the middle, leaving class B completely unresolved. With a quadratic regression one can resolve 3 classes, but cannot resolve 4, etc. To resolve more classes, one has to transform regression values with "increasingly non-linear" functions F(Xθ_k): they should be monotonous and fall down fast enough to offer fine class resolution. refs: [1](https://stats.stackexchange.com/questions/43867/why-does-the-least-square-solution-give-poor-results-in-this-case)

# Discriminant Analysis

As we'll see later, can be considered an alternative to logistic regression. We assume that each class density is a multivariate Gaussian:

$\displaystyle Φ_k(x) = \frac{1}{უ}\exp\big( -½ (x-μ_k)^Tტ^{-1}_k(x-μ_k) \big)$,

where $უ = 2π^{(p/2)}\sqrt{\det(C)}$, and ტ is a covariance matrix of this Gaussian, defining its shape (elongation) and orientation. If we don't put any extra assumptions into it, this approach would be called **Quadratic Discriminant Analysis (QDA)**, but if we assume that all gaussians have the same shape, in the sense that ტ is shared across all classes, we get **Linear Discriminant Analysis (LDA)**.

Apparently if you do log(P_k/P_l) for two classes k and l (decision boundary), you end up with an equation linear in x (ESL p108), making the decision boundary linear, because lots of terms cancel each other out. _I don't get the math, but have to park it for now._ As ტ is not necessarily spherical (σ²I would be spherical), bisectors of classes are not necessarily ⊥ to lines connecting the centroids.

The decision becomes a simple thersholding of a **linear discriminant function**: δ(x) = ⟨x,ω⟩ against a certain threshold c, where vector ω = ტ⁻¹μ, and thresholds c = ½ μᵀტ⁻¹μ - log n. Here μ is the mean for this class, estimated as μ = ∑x/n for all xi in this class; n is the number of elements in this class. Whichever δ (for whichever class) has the highest value for a given x, defines the predicted class: g = argmax_k δ_k (x). As ტ is assumed to be the same for all gaussians, the formula for it is similar to pooled variance: ტ = ∑_i (xi - μi)(xi-μi)ᵀ/(N-K), where μi is a matching mean for point xi; N is the number of points, and K is the number of classes. The thresholds can also be optimized explicitly, as hyperparameters, to achieve optimal class separation.

The results of LDA for 2 classes only match that of logistic regression if the number of points in all classes is the same.  For more than 2 classes, it doesn't match logistic regression.

One can also smoothly move betwen QDA and LDA, using **Regularized Discriminant Analysis**, in which case you calculate covariance matrices ტk for individual gaussians, and a pooled ტ, and then work with something in-between: αტk + (1-α)ტ, treating α as a hyperparameter. Alternatively, one can shrink ტk towards a scalar σ²I.

LDA can be used for direct **dimensionality reduction**, as **Reduced-Rank LDA**. The idea is that K centroids always lie in a hyperplane of dim = K-1, which is typically ≪ p. It means that once centroids are found, we can project X into the subspace Hk of centroids, and thus get a good lower-dim representation of the data. For K=3, we can also make a 2D plot. If K is ≫ 3, but we need a plot, we can also run PCA on the Hk space (instead of running it in the X space), obtaining so-callled **canonical coordinates**, aka **discriminant coordinates**. Running a 2D PCA is equivalent to finding a pair of coordinates that maximize the variance of μ, which in practice means spreading centroids as much as possible, which resonates with the idea of LDA. Unlike for PCA, the projection doesn't just try to spread all points in X, but separate them. 

> Computational math in ESL p113, and then p114-116. Parked for now.

Refs: ESL p109-113; [wiki](https://en.wikipedia.org/wiki/Linear_discriminant_analysis)

What's the practical **difference between LDA and Log Regression?** In both cases, the math boils down to log P(x ∈ gi)/P(x ∈ gk) = xᵀθ, but θ are different. In both cases joint P(x,g) = P(x)P(g|x), where P(x) is called marginal density of inputs x. P(g|x) has the same form for both LDA and LR, but P(X) is different. With LR, P(x) is free (assumed to be arbitrary), and we maximize P(g|x). With LDA, we explicitly maximize P(x,g), and insist that P(x,g=k) = Φ(μk,ტ) is a p-dimensional Gaussian. Apparently, if the assumption of LDA is true, it gives you a boost of ~30% efficiency on error rate (you get same performance with ~30% less data). But if you aren't sure, then LR is safer.