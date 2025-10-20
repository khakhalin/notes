# Linear Discriminant Analysis (LDA)

#classification

Parents: [[classification]], [[features]]
Related: [[logreg]], [[svm]]

As we'll see later, LDA can be considered an alternative to logistic regression. We assume that each class density is a multivariate Gaussian:

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