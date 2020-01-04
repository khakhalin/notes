# Classification
Classic classification example: [MNIST](http://yann.lecun.com/exdb/mnist/index.html) (link to a library of all thinkable approaches to classification, as applied to this dataset, each given with a test error rate)

# 1NN and KNN
Simplest archetypical approach: **Nearest Neighbor**. Just pick closest training case. This is an example of a **lazy** approach: instead of generating estimations upfront (that would be called **eager**), we only generate them at retrieval. A better. and more practical, approach: **K nearest neighbors** (aka KNN).

While KNN is lazy, for analysis purposes we can calculate predictions on a grid, and thus identify borders between areas "assigned" to different categories. For k=1 (simple Nearest Neighbor) we get a Voronoi tesselation between all training points (disconnected and jaggedy, as each point gets its own area). Which presents a case of extreme overfitting: all training data is correctly classified, but it looks scary. Higher k: smoother areas, making k a **hyperparameter**. But the **effective number of parameters** for KNN is higher (about N/k), as data points themselves serve as parameters.

**Can we use quadratic loss function for KNN?** No, coz it would go to 0 for k=1, so hyperparameter search would always recommend overfitting. We should use **Max Likelihood** instead: Say, you have data X and a qualitative output G with values g_k denoting several different classes. We can have P(G=g_k|X=x) = P_k(θ,X), and try to maximize ∏P for a given data. It's the same as maximising L = ∑log P(θ , g_i , x_i) in the space of θ.

KNN can be used for numerical predictions as well (beyond simple classification); just use the average of y-values for nearest k x-points (in fact, that's how ESLII introduces it).

How to find optimal k? We'll need a training and a testing set, then try different values of k, and find the point when the test error (or some other measure) is lowest for test data (ESLII Fig 2.4).

KNN is an archetype for many fancy methods. For example, if instead of "nearest" δi that are all or none we'll use fuzzy weights that are larger when you are close to each data w(distance), it's the same as just regressing on distance, which is a type of a **kernel method**. If we want some dimensions of X matter differently than others, we can use non-round kernels. **Local regression** is also similar in spirit (sorta a combinatino of linear regression and the idea of contextual locality). DL networks also imitate something like that by mixing and mashing linear transformations.

# Model assessement
**Model accuracy**: The most primitive measure = number of true statements / total number of statements. Obvious dependency on class balance (famous example, a statement of "today is not Christmas" has an accuracy of 99.7%). But may work for toy examples with very carefully balanced classes (like MNIST).

**Precision and recall**: Let's concentrate on detection only (positive predictions only). We want something that is proportinal to true positives (TP), but what to put in the denominator? Two options: 

* TP/(all positive predictions) = TP/(TP+FP) - how well can we believe positive predictions. Is called **Prediction**, and is related to false discovery rate (P=1-FDR). From the stats POV, it's about false-positives, or Type I.
* TP/(all actual positives) = TP/(TP+FN) - how well does the model actually detect cases, and how sure can we be that most positive cases were detected, that not much was left behind. **Recall**. From the stats POV, it's about false-negatives, or Type II.

Can we balance them somehow, to make sure the model is reasonably good on both accounts? Most popular approach: **F1 score**: F = 2∙(precision∙recall)/(precision+recall) = harmonic_mean(precision,recall) = inv(mean(inv(precision),inv(recall))).

An even better approach: **AUC** = **Area under the ROC (Receiver Operating Characteristic) curve**. With this approach, you take a threshold-like parameter of the model, and slide it through all possible values from ideally permissive to fully prohibitive. For max permissive we get maximal recall, or TPR=1, but also no true negatives at all: TNR=0 ⇒ FP=1 (that's how false-positives are defined, as 1-TNR). For max prohibitive, we get TPR=0, but also FPR=0, as TNR=1. For random probability, AUC=1/2, for perfect knowledge AUC=1.

Interestingly, AUC = P(score of a true example > score of a wrong example). _I wonder if proving this is hard?_

**Prediction bias**: a very simple measure of model validity: average value of all predictions - average value of all learning points. In a reasonable model, prediction bias should be close to 0. A way to assess it: build a **calibration plot** - bucket values, calculate predictions, then plot mean(predictions) against mean(values). May help to find areas where the model misbehaves.

## Validation and testing
Canonical system of names:
* **Training dataset** - obviously
* **Validation dataset** - used to calculate validation error; monitored alongside training dataset. The difference between error on validation and training sets is used for model selection, regularization, hyperparameters tuning.
* **Testing dataset** - ideally, whould never be seen by the model until the very end. But at the same time, as models are eventually evaluated based on their performance on the testing dataset, arguably, it indirectly participates in hyperparameter tuning and model selection. Still, try to minimize that.

**Cross-vallidation**: Instead of setting apart one validation dataset, generate lots of those by random splits. Recommendations for split are around 70/30 or 80/20, although extreme versions of 50/50, or "**leave one out**" also exist. Note that repeating random split 10 times is different from **10-fold cross-vallidation**, as there will be overlaps between random splits, while "repeated holdout" carefully cycles through hiding from the model each of the possible equally-sized validation blocks.

What's the current best practice?
* k=3 for big (slow, expensive) models, leave-one-out for small ones - [ref](https://medium.com/@george.drakos62/cross-validation-70289113a072?)
* k=5 or 10 just because feels like reasonable numbers - [ref](https://machinelearningmastery.com/k-fold-cross-validation/)

For unbalanced datasets and multi-class classification, it is honest to used **stratified cross-validation**, when each of the strata is split separately, and then mixed back, to avoid class imbalance.

# Linear separation

**Can regression be used for classification?** Aye, just set a threshold (0.5?) for the output value. Aka fitting a **dummy variable**. If h(x) is a hyperplane, h=const becomes a 1-d-lower hyperplane (in 2D case: line) separating the space of x into 2 halves. **Linear separation**. Apparently, the best we can do for 2 overlapping Gaussians.

**Can regression be used for categorical variables?** (Multi-class classification?) Also aye, but a different loss, summing costs of all misclassifications (when a point from class Gk was erroneously classified as a point in Gl). We'll need a matrix of costs, and from it calculate a matrix of losses L(k,l). Most often though, one cost for all errors, and **expected prediction error** EPE = E(L). We can try to optimize EPE point-wise: G = armin_g ∑ L(Gk counted as g) ∙ P(observing point from Gk | for X=x).

If all errors cost the same, G = argmin_g (1 - P(g | x)) - likelihood optimization? Aka **Bayes classifier** - just go for the best guess of conditional probability P(class | observation). The error rate for it is called **Bayes rate**: the lowest possible rate, coming from the variability of data itself (analogous to irreducible error for continuous data: [wiki](https://en.wikipedia.org/wiki/Bayes_error_rate))

# Logistic Regression

**Logistic function**: $\sigma(x) = \frac{1}{1+e^{-x}} = \frac{e^x}{1+e^x}$.

The inverse function is called **logit**: logit(p) $=\ln \frac{p}{1-p}$

Assumes that **log-odds** $\log \frac{p}{1-p}=a+bx$ , so is a linear function of x (that is generally a vector). It means that p/(1-p)=exp(ax+bx), which leads to p = σ(ax+b). It can be said that a logistic regression is just a linear regression of log-odds.

To find a solution, minimize **logloss**: Loss = $-\sum (y\cdot\log(\tilde y)+(1-y)\cdot\log(1-\tilde y))$, or, using p for y estimations, -∑( y∙log(p) + (1-y)∙log(1-p) ). As p→0, -log(p)→inf, so huge punishment for near-zero p. Because of that, if the data is too clean and some areas contain only points of one type, weights may explode (it's hard to fit infinity), making proper regularization extremely important.

For high-D non-linear data, works great if features cross-products are included into the set as synthetic features (see [[07_Features]]).

# SVM
Widest street approach: find a line, so that if you have a band around the line, it separates the positive from the negative examples as best as possible.

How to use the line? Describe it with a vector ω ⊥ line. Now whether a point u is far from the line can be described as uω. Let's say that b is a good projection value (u lies right smack on the central line), so we have u∙ω + b ≥ 0.

To find the best line, let's insist that for samples we don't just get above 0, but get xω+b above 1 for positive samples, and -1 of negative samples. 