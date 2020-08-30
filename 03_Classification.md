# Classification

#classification

Subtopics:
* [[svm]] - Support Vector Machine

# 1NN and KNN

Simplest archetypical approach: **Nearest Neighbor**. Just pick the closest training case. This is an example of a **lazy** approach: instead of generating estimations upfront (that would be called **eager**), we only generate them at retrieval. A better. and more practical, approach: **K nearest neighbors** (aka KNN).

While KNN is lazy, for analysis purposes we can calculate predictions on a grid, and thus identify borders between areas "assigned" to different categories. For k=1 (simple Nearest Neighbor) we get a Voronoi tesselation between all training points (disconnected and jaggedy, as each point gets its own area). Which presents a case of extreme overfitting: all training data is correctly classified, but it looks scary. Higher k: smoother areas, making k a **hyperparameter**. But the **effective number of parameters** for KNN is higher (about N/k), as data points themselves serve as parameters. 

KNN can be used for numerical predictions as well (beyond simple classification); just use the average of y-values for nearest k x-points (in fact, that's how ESL introduces it).

**Can we use quadratic loss function for KNN?** No, coz it would go to 0 for k=1, so hyperparameter search would always recommend overfitting. We should use **Max Likelihood** instead: Say, you have data X and a qualitative output G with values g_k denoting several different classes. We can have P(G=g_k|X=x) = P_k(θ,X), and try to maximize ∏P for a given data. It's the same as maximising L = ∑log P(θ , g_i , x_i) in the space of θ.

How to find optimal k? We'll need a training and a testing set, then try different values of k, and find the point when the test error (or some other measure) is lowest for test data (ESL Fig 2.4).

> OK, there's something I don't understand here. If we introduce training and testing sets, why cannot we use quadratic loss? For a smooth enough underlying function, k=1 (overfitting) would almost certainly lead to higher L2 loss than a more decent "averaging" k. Right? Please check. #todo

KNN is an basic archetype for a whole family of fancy "local" methods. For example, if instead of "nearest" δi that are all or none we use fuzzy weights that are larger when you are close to each data w = K(distance), it's the same as just regressing on distance, which is a type of a **kernel method**. If we want some dimensions of X matter differently than others, we can use non-round kernels. **Local regression** is also similar in spirit (sorta a combination of linear regression and the idea of contextual locality). DL networks with RELU also imitate something like that by having different subnetworks effectively enabled for different inputs (depending on which neurons inactivate) _(is it true?)_.

# Model assessement

**Model accuracy**: The most primitive measure = number of true statements / total number of statements. Obvious dependency on class balance (famous example, a statement of "today is not Christmas" has an accuracy of 99.7%). But may work for toy examples with carefully balanced classes (like [[mnist]] for example).

**Precision and recall**: Let's concentrate on detection only (positive predictions only). We want something that is proportinal to true positives (TP), but what to put in the denominator? Two options: 

* TP/(all positive predictions) = TP/(TP+FP) - how well can we believe positive predictions. Is called **Precision**, and is related to false discovery rate (P=1-FDR). From the stats POV, it's about false-positives, or Type I. The metaphor for the name: imprecise estimates are hard to believe.
* TP/(all actual positives) = TP/(TP+FN) - how well does the model actually detect cases, and how sure can we be that most positive cases were detected, that not much was left behind. **Recall**. From the stats POV, it's about false-negatives, or Type II. THe metaphor for the name: you show actual positives; can the model recognize ("recall") them?

Can we balance them somehow, to make sure the model is reasonably good on both accounts? Most popular approach: **F1 score**: F = 2∙(precision∙recall)/(precision+recall) = **harmonic_mean**(precision , recall) = inv(mean(inv(precision),inv(recall))).

An even better approach: **AUC** = **Area under the ROC (Receiver Operating Characteristic) curve**. With this approach, you take a threshold-like parameter of the model, and slide it through all possible values from ideally permissive to fully prohibitive. Then we connect the dots and build the **ROC** in TPR/FPR coordinates (aka Sensitivity vs 1-Specificity). For max permissive we get maximal recall, or TPR=1, but also no true negatives at all: TNR=0 ⇒ FPR=1 (that's how false-positive rate is defined, as 1-TNR). For max prohibitive, we get TPR=0, but also FPR=0, as TNR=1. And we look at the area under this curve. For random probability, AUC=1/2, for perfect knowledge AUC=1.

> So FPR and TPR are not at all the opposites of each other; they have different everything! TPR = 1-FNR = TP/(TP + FN), while FPR = 1-TNR = FP/(FP+TN). Don't get confused here. Note that the denominator is always the numerator + its opposite, and if you use some common sense, you can always "reconstruct" what the negative is, as with incorrect classification both the label and the truthfulness of this label are negated.

Interestingly, AUC = P(score of a true example > score of a wrong example). _I wonder if proving this is hard?_ #todo But at least for two extreme cases this statement can be "felt" inuitively: for perfect classification (where score is sorted, with all class 0 points having lower scores that all class 1 points) AUC=1, as for very low thresholds only FPR changes (all TP are classified correctly, but not all TN), and then at some point it reverses (all TN are classified incorrectly, but not all TP), so AUC makes a sharp bend through the top left corner of the square. And for random scoring (where, for a large N, class 0 and class 1 are about uniformly distributed), AUC draws a diagonal.

**Confusion matrix**: a good diagnostic tool for multiclass classification. Take all true classes, and tally into which classes they are misclassified. Identify the issues. _Is it better to do it on the validation set?_ It's better to zero the diagonal, as most cases will be classified correctly, but we're interested in the errors!

Canonical system of names for **Validation and Testing**:
* **Training dataset** - obviously
* **Validation dataset** - used to calculate validation error; monitored alongside training dataset. The difference between error on validation and training sets is used for model selection, regularization, hyperparameters tuning.
* **Testing dataset** - ideally, whould never be seen by the model until the very end. But at the same time, as models are eventually evaluated based on their performance on the testing dataset, arguably, it indirectly participates in hyperparameter tuning and model selection. Still, try to minimize that.

**Cross-vallidation**: Instead of setting apart one validation dataset, generate lots of those by random splits. Recommendations for split are around 70/30 or 80/20, although extreme versions of 50/50, or "**leave one out**" also exist. Note that repeating random split 10 times is different from **10-fold cross-vallidation**, as there will be overlaps between random splits, while "repeated holdout" carefully cycles through hiding from the model each of the possible equally-sized validation blocks.

What's the current best practice? Some opinions:
* k=3 for big (slow, expensive) models, leave-one-out for small ones - [ref](https://medium.com/@george.drakos62/cross-validation-70289113a072?)
* k=5 or 10 just because feels like reasonable numbers - [ref](https://machinelearningmastery.com/k-fold-cross-validation/)

For unbalanced datasets and multi-class classification, it is proper and honest to used **stratified cross-validation**, when each of the strata is split separately, and then mixed back, to avoid class imbalance.

# Linear separation

**Can regression be used for classification?** Aye, just set a threshold (0.5) for the output value. Aka fitting a **dummy variable**. If h(x) is a hyperplane in (x,y), h=const becomes a 1-d-lower hyperplane (aka **decision boundary**; in 2D case: a line) separating the space of X into 2 parts. **Linear separation**. Apparently, the best we can do for 2 overlapping Gaussians.

What about **multi-class classificaton**; can regression be used then? Also aye, but with a different loss (not L2), but rather with summing costs of all misclassifications (when a point from class Gk was erroneously classified as a point in Gl). We'll need a matrix of costs, and from it calculate a matrix of losses L(k,l). Most often though, one cost for all errors, and **Expected Prediction Error** EPE = E(L). We can try to optimize EPE point-wise: G = armin_g ∑ L(Gk counted as g) ∙ P(observing point from Gk | for X=x).

If all errors cost the same, G = argmin_g (1 - P(g | x)), known as likelihood optimization, or **Bayes classifier**: just go for the best guess of conditional probability P(class | observation). The error rate for it is called **Bayes rate**, and it is the lowest possible rate, coming from the variability of data itself, analogous to irreducible error for continuous data: [wiki](https://en.wikipedia.org/wiki/Bayes_error_rate). If data is deterministic (every x↔g), Bayes rate=0, but if same x can produce diff g, it's ≠0, because the model will never be able to perfectly overfit the data.

**Kernel tricks**: to perform separation in of non-linear data, or data in high D, add feature squares and cross-products into the set, as synthetic features (see [[04_Features]]). Then perform separation. Linear boundaries will transform to quadratic boundaries.

If each class dummy variable is fit with f_i = Xθ_i, then the decision boundary between 2 classes is a line f_i = f_j, and so X(θ_i - θ_j)=0 is also a line.  So we can take **indicator matrix** Y (each row is a class, one-hot encoded), and try fit each column of it using linear regression: H = XΘ = X(XᵀX)⁻¹XᵀY, where Θ is a matrix composed of many columns θ: one for every column in Y. This H would have a reasonable property of ∑f_k(x) = 1, when summing across all classes g_k, but individual f_k are linear, and so totally go outside of (0,1). This may be a problem.

Another problem is **class masking**: imagine 3 separable classes ABC lying roughly on a line. Decision boundaries between AB and BC will be roughly parallel, but when taking together, 3 linear functions will form only one boundary somewhere in the middle, leaving class B completely unresolved. With a quadratic regression one can resolve 3 classes, but cannot resolve 4, etc. To resolve more classes, one has to transform regression values with "increasingly non-linear" functions F(Xθ_k): they should be monotonous and fall down fast enough to offer fine class resolution. refs: [1](https://stats.stackexchange.com/questions/43867/why-does-the-least-square-solution-give-poor-results-in-this-case)

# Logistic Regression

See also: [[softmax]]

By Bayes theorem, our best guess about P(g|x) woudl be P(g|x) = f(x)p/∑f(x)p , where p is prior probability, and sum goes through all classes. Not only it looks a more like a probability, but actually leads to the math behind logistic regression.

**Logistic function**: $\displaystyle \sigma(x) = \frac{1}{1+e^{-x}} = \frac{e^x}{1+e^x}$. The inverse function is called **logit**: logit(p) $=\ln \frac{p}{1-p}$. **Logistic regeresion assumes that log-odds** are linear: log p/(1-p) = Xθ, which means that p/(1-p)=exp(Xθ), leading to p = σ(Xθ). This shape is nice and doesn't suffer from class masking.

For a multi-class case, the log-odds for each class g_i are fit linearly: log P(x∈gi)/P(x∈gk) = xᵀθi. Here g_k is some reference class (often the last one on the list); it doesn't really matter which one exactly; what matters is that, if you check it, with this formula we always get ∑P(gi)=1.

To find a solution, use max-likelihood (maximize conditional P(g|x) ), which is equivalent to **minimizing log-loss**. For two classes, log-loss = -∑(y log(P(1|x)) + (1-y)log(1-P(1|x))) , with the sum taken over all training points x_i and their labels y_i∈{0,1}. As with p→0, we have −log(p)→inf, we can expect numerical problems when the data is too clean and some areas only contain points of one type, as weights can explode (it's hard to fit infinity!), making  **regularization** extremely important. 

The expression for loss can be simplified: 
L(θ) = -∑( y log(p) + (1-y)log(1-p) ) =
-∑( y (log(p) - log(1-p)) + log(1-p)) =\
-∑( y log(p/(1-p)) + log(1-p) ) = …

...Now log(p/(1-p)) is just plain Xθ by definition above. For log(1-p), substitute p=sigmoid(xᵀθ) = 1/(1+exp(-xᵀθ)), then calculate 1-p, mutiply both numerator and denominator by exp(xᵀθ), take a log, use log(1/a) = -log(a), resulting in log(1-p) = log(1+exp(xᵀθ)). Put both in the formula above, get:

L(θ) = … = -∑( yxᵀθ - log(1+exp(1+xᵀθ)) ). 

Differentiate this by θ, set to 0. Get ∂L/∂θ = −∑ (xy − x∙exp(xᵀθ)/(1+exp(xᵀθ))) = −∑x(y-p) = 0. Here each x (each point) and $\vec 0$ on the right side are both vectors length ndim+1 (one extra dim for the intecept $θ_0$). 

> ESL p120-121 gives a solution for the updating (descent) procedure that I skip for now. Also there's a weighted self-consistency formula that ties θ, x, y, and p together, and can apparently be used to achieve some numerical shortcuts.

## Regularized Log Regression

**L1 penalty** (lasso) is good for this: take the previous L=-∑ y log(p) + (1-y) log(1-p), and add to it λ∑|θj| , where the sum runs by dimention (variable). Flip signs if you like maximizing stuff, as ESL does. Apparently, if you do the math, you get the following link between everything: xjᵀ(y-p) = λ∙sign(θj) for each dimension j.

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