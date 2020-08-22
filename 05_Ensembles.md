# Ensemble methods

#ensemble

Ensembles are about combining (or gradually refining) lots of poor predictions (aka **weak learners**) into a very good prediction (aka "Wisdom of Crowds": popularized by a 2004 [book](https://en.wikipedia.org/wiki/The_Wisdom_of_Crowds) by Surowiecki).

# Bagging

Bootstrapping can be used to improve estimations. **Bagging**: bootstrap the data repeatedly; each time build an estimator, then average predictions of these estimators. Why does it work though? For linear estimators (like linear regression) it doesn't, as E(h(x)) = h(E(x)) = h(full data). But for non-linear h(), like **regression trees**, it may smoothen idiosyncrasies of each individual h(x).

How to best combine opinions of several classifiers into one output? One option is to one-hot encode output labels, average them up across models, produce a vector, then pick the maximal score (the most popular label). Another is to make each classifier output vectors of probabilities for different levels, instead of levels themselves; then average these probabilities. This latter approach is smoother (lower variance), and if each vector is ∑=1, then the final output would naturally ∑=1, which is helpful.

> Bias is apparently unchanged (ESL p285, stated as a fact, followed by a page of confusing "illustration"). Is it because no additional constraints are introduced by the procedure? How to intuit it? In which cases the bias WOULD BE changed? What sorts of modifications of h(x) procedure change the bias, and which sorts do not?

> So from this part, it seems that the key power of bagging is that it takes a very discrete, run-down method that is by design jumpy, and also overfits like hell, and turns it into a smoothed landscape. Is this shifting from discrete to pseudo-continuous the main reason for why bagging works? Or is some hidden indirect serendipitous regularizaton also involved?

Note that bagging doesn't work if loss is heavily quantized (like in **0-1 loss**, which is essentially accuracy). In this case, if you get a saturated classifier (all points→one class), there's no way to improve on it. Moreover, for a random classifier with constant probabilities (returns 0 with probability p0, and 1 with p1) bagging would only worsen things (as bagging will lead to const classifier for whatever p is greater, as it would win on average). _So for this to work, we need to make the classifier as uneven and moody as possible, right?_

A slightly more sophisticated **counter-example**: points uniformly fill a (0-1,0-1) R² square; 2 classes with a diagonal split (ESL p288). Any subsampling from this will look like a blobby squary cloud with a diagonal; a split will fall kinda in the middle, and the edges of the diagonal will never be resolved.

Trees are also notoriouly bad in splitting 2D **XOR** in a unit square, as averaging across both x and y gives left≈right, and a greedy tree would just split at some random point, defined by data density variations, and not really by class borders.

> Great #todo : model that in a playground, just to get a feel of what it can and cannot do.

ESL p288 claims that bagged estimate is an approximate posterior Bayesian mean, which is something I don't get yet.

**Bayesian interpretaton:** Suppose you're trying to estimate ζ, given a training set Z, and we have a family of models {M}. Then we can marginalize posterior P(ζ|Z) over the family of models {M_i}: P(ζ|Z) = ∑P(ζ| M,Z)∙P(M|Z), summing across M in {M}. It means that posterior mean E(ζ) will also be a weighted average across models, weighted by probability of each of these models given the data: E(ζ|Z) = ∑E(ζ | M)P(M|Z), summing across M in {M}. In practice, **Committee methods** give equal weight to each observed model. (_I'm guessing it means they assume that with enough models samples, P(observing a model) will be taken care of just by design of this model-sampling procedure?_)

Alternatively, try to estimate P(M) somehow; say, if a model has a set of parameters θ, one could Bayes-flip P(M|Z) into P(Z|M), further marginalize P(Z|M) by these parameters as ∫P(Z|θ)P(θ|M)dθ, and compute it numerically. _Not sure what it means and how it helps..._

**Frequentist take**: let's do linear regression on results of all models, and find a mix of them that would minimize the L2 loss. If outputs of every model are f_i, together they form a matrix F, and we know how to do population regression to a true target Y: w = (FFᵀ)⁻¹FY. (**Population regression** just means that here instead of inner products, we imagine taking actual expectations E() over the true distribution that generages the data). As linear regression is a minimization of L2= var(Y-wF), then minimzed var(Y-wF) < var(Y-f_i) ∀i. In practice, population regression is of course impossible, so we can do the next best thing: linear regression on training data.

> But then they say something that I don't understand; how with different models having different complexity somehow everything would break. Why again? ESL p290

# Stacking

A practical way to create a reasonable linear combination of several non-linear models using cross-validation.

**Stacked generalization** or **Stacking**: Say you have a way of building various models f_m, and you train each of them on a dataset X. Models f_m may belong to the same class, or to different classes, but it is important that they are sufficiently different: that is, they need to be defined both by the training set X, and by some hyperparameters that come with the model itself. So while each model is a function of a dataset, f(X), two models f_m1(X) and f_m2(X) should still be different.

Consider training each model f_m() on a dataset  X minus observation xi. It creates a family of predictors $f_m^{-i}(x)$ for each model class f_m(), providing a good way of assessing the accuracy of m-type models f_m() via cross-validation: just predict y_i by each $f^{-i}$, and sum all errors. But instead of just picking the best model, we will build the best linear combination of all these models, by finding an optimal vector of weights w, so that
$\sum_i \big( y_i - \sum_m w_m f_m^{-i}(x_i)\big)^2$ is minimal. In other words, we find a set of coefficients w, so that the linear combination of all f_m gives the best total cross-validation across all "remove one x_i". It also helps to constrain w to w_i>0 ∀i and ∑w_i = 1, as it turns it into a quadratic problem.

Refs:
* ESL p282-290

# Bumping

A way to find better models by randomly moving in model space, avoiding local minima. Train a bunch of models on different subsamples of X, then test each on full X, and pick the best. As in this case optimization is kinda shifted towards the end, it's probably better to use quite undersampled training sets. Say, for a **XOR** case, if you test enough small subsamples, at least one of them will probably split the data decently, even though training on a full dataset may be hopeless for many types of models.

It's important to keep all models in  the comparison group similar in their complexity (for trees, same number of terminal nodes).

# Boosting (AdaBoost)

Boosting uses simplest decision trees possible:  for every coordinate we make a single split into 2 categories, aka **decision stumps**. But with every next series of trees, we increase the weights for those elements that weren't classified correctly by the previous generation of classifiers. (At the beginning of the process, for the very first tree, we start with equal weights). These weights may either be explicitly included in the error calculation, or be used as probabilities for each data point to appear in next training subset (mathematically, the results are the same). At the end, predictions are made by **weighted majority rule**: the average across all trees, weighted by the **accuracy of individual trees**, followed by argmax.

The most popular, archetypical approach: **AdaBoost**, aka **Adaptive Boosting**. For a binary discrete case:
1. **Start with** all points x_i having **identical weights** {W}. As $∑w_i$ = 1, we start with w_i = 1/n.
2. **For each coordinate, find the best split** (aka **stump**), with smallest total error $E = ∑w_i ϵ_i$, where $ϵ_i = 1\cdot(h(x_i)==y_i)$. Here $h(x)$ is the predictor function for this split.
3. Now **across all coordinates**, find a split with minimal [[gini]] index (one that achieves best separation of classes). For all loops after the 1st (once the weights are different), either use **weighted gini index**, or randomly resample points with draw probability P ∝ w (draw with repetition, keeping the total number of points at each split constant).
4. Calculate the **weight of this stump** as $α = ½ \log((1−E)/E)$ , where E again is the error. This formula →+∞ for E→0 (perfect split), →-∞ for E→1 (perfectly erroneous split), and ~0 for splits that perform near chance level, when E=1-E, and so α≈log(1). In practice, to avoid ∞, a tiny ε is added to both numerator and denominator of the fraction under the log().
7. Reject trees with accuracy less than 50%. _Not all descriptions mention that._
9. **Produce an average of tree outputs, weighted by α**. In the end, this will be our final classifier.
10. If all points are classified correctly (or if some pre-agreed threshold accuracy is reached, in case of early termination), consider the task completed. This weighted average from the previous step is our model.
11. But if we are not happy yet, **increase weights of misclassified samples**: $w_i ← w_i \exp(α)$; decrease weights of correctly classified samples by multiplying them by exp(−α); then normalize all weights again to $∑w = 1$. Then go to step 2 and generate a new split for our collection.

> Is it true that in practice randomly resampling points is better than using a weighted formula? Nobody says it openly, but if it weren't the case, why would people repeat this whole resampling story in each tutorial?

Because each split is performed along a single variable (axis), it is || to all other variables. Because of that, the final weighted decision border looks like a combination of these planes (lines). But all of them are ⊥ or || to each other, just shifted; there are no curves or non-right angles there. So the final decision border looks kinda like a crystalline maze; like a crystal of bysmuth or salt.

For multi-class classification, either create lots of binary classifiers (each class against all others), or encode each class as a superposition of several binary "features" that may be present or absent (say, bunnies are cute and jumpy, cats are cute but not jumpy; crickets are jumpy but not cute etc.), then use AdaBoost to identify the presence of each of these features separately ([ref](https://engineering.purdue.edu/kak/Tutorials/AdaBoost.pdf)).

In many ways, AdaBoost goes against the conventional wisdom for classification: it doesn't try to build a good classifier, but instead uses a lot of crappy ones. It does not try to identify important variables, or isolate important features using dimensionality reduction, but instead, it thrives in this high-D multi-variable space.

It is also possible to use boosting with custom loss functions: for example, one can put more weight in false-positives compared to false-negatives, or ther other way around.

Refs:
* [Akash Desarda](https://towardsdatascience.com/understanding-adaboost-2f94f22d5bfe)
* https://en.wikipedia.org/wiki/AdaBoost
* [Tommi Jaakkola](http://people.csail.mit.edu/dsontag/courses/ml12/slides/lecture13.pdf) lecture note;
* [Explaining AdaBoost](http://rob.schapire.net/papers/explaining-adaboost.pdf) by Robert Schapire;
* [YouTube video by StatQuest](https://www.youtube.com/watch?v=LsK-xG1cLYA) (good)
* [Slides by Avinash Kak, Purdue](https://engineering.purdue.edu/kak/Tutorials/AdaBoost.pdf) (good).

# Gradient Boosting

#todo #halfthere
* https://explained.ai/gradient-boosting/index.html
* https://explained.ai/gradient-boosting/L2-loss.html
* https://explained.ai/gradient-boosting/L1-loss.html
* https://explained.ai/gradient-boosting/descent.html

**Gradient Boosting Machines**, or **GBM**, work in problems where the output is numerical rather than categorical. GBM recursively approximates Y as a series of models {F_k], with each next model F_k  improves over the previous one: $F_k = F_{k-1} + f_k(x)$, where f_k() is some sort of weak learner. The basic algorithm, therefore, is at each step to iteratively approximate the difference between Y and the previous best model F_k (aka the residual) with a new function f_k(x).

In practice, the most popular type of a weak learner f() for GB is a **regression tree**, or more precisely a **regression tree stump**: the simplest decision tree that consists of one basic split over one variable (one coordinate of X), and produces two different values (levels) on each side of this split. Once the fitting is complete, all regression tree stumps are summed together, to produce the final output of the ensemble. 

To find the best split (best model improvement) at each step, we need to first introduce a smooth differentiable loss function (usually L2, if there are no outliers, or L1 if outliers are common). GB then performs a stepwise gradient descent to minimize this loss function. Depending on the function, descent can be performed in several different ways:
* One approach, is to use f(x) to fit Y-F_k: a vector of differences between each y_i and its best current estimation F_k(x_i), as described above. This leads to the minimization of L2.
* Alternatively, one can fit sign(Y-F_k): some sort of Manhattan-style normalized direction towards the gradient. This leads to the minimization of L1.

Often instead of applying each impovement f_k() in full, it is multiplied by a coefficient η < 1 (typically between 0.5 and 1.0), called the **learning rate**. This smoothens the descent.

The initial (0th) optimization step is to calculate the mean of all data (for L2 loss), or its median (for L1).

As GBM is a greedy algorithm, and is prone to overfitting, and so requires **hyperparameter tuning** of the maximal number of stages M, and learning rate η. GBM can also be **regularized**: for example, stochastic GBM would only consider a part of the data for each decision tree (kinda like in bagging).

Refs:
* [How to explain gradient boosting](https://explained.ai/gradient-boosting/index.html), Parr, Howard. Very good description!
* [Custom loss functions for Gradient Boosting](https://towardsdatascience.com/custom-loss-functions-for-gradient-boosting-f79c1b40466d), Prince Grover, 2018
* Friedman, J. H. (2001). Greedy function approximation: a gradient boosting machine. Annals of statistics, 1189-1232. - Mathy paper with 10k citations explaining everything about gradient boosting. https://statweb.stanford.edu/~jhf/ftp/trebst.pdf
* A bunch of confusing explanations that I either didn't like, or that explain boosting in general rather than gradient boosting specifically:
    * [GB from scratch](https://medium.com/mlreview/gradient-boosting-from-scratch-1e317ae4587d)
    * [Gentle Introduction to the Gradient Boosting Algorithm](https://machinelearningmastery.com/gentle-introduction-gradient-boosting-algorithm-machine-learning/)
    * [Understanding Gradient Boosting Machines](https://towardsdatascience.com/understanding-gradient-boosting-machines-9be756fe76ab)

## Bagging vs Boosting comparison

* Bagging tends to decrease variance, but not necessarily bias. Boosting tends to reduce both variance and bias.
* Conversely, Bagging does not overfit, while Boosting can overfit easily, by slicing space very thin around every point.
* Except for the very last step, bagging can happen in parallel, while boosting is by definition sequential, and so not easily parallelizable.
* Compared to many other methods, both bagging and boosting are very fast.

Refs:
* https://quantdare.com/what-is-the-difference-between-bagging-and-boosting/
* https://towardsdatascience.com/understanding-adaboost-2f94f22d5bfe

# Random forest

The idea: construct many full trees, by bagging (partial data), but also by providing to every tree a random subset of features (aka **feature bagging**; typically √p features out of p total, or something like max(5, p/3), ref: [wiki](https://en.wikipedia.org/wiki/Random_forest#From_bagging_to_random_forests)). This is an improvement upon bagging, as it makes trees less correlated, more diverse. In this approach, trees are always built to maximal depth, which makes them quite overfitted, but it's OK, as there are many of them.

> Not sure if the sequence in which different values are used for splits is also randomized for each tree, or allowed to be optimal. I'd expect that both approaches could be possible, depending on dimensionality; is it true?

All trees usually have an equal vote in the final classification.

Random forests are great for ranging features in terms of their usefulness, as one can see at the average performance of trees that include a certain feature, and compare it to the average performance of all trees in a forest (aka **Feature Importance**). Scikit-Learn, for example, computes these scores automatically.

A similar approach: **Extra Trees** of **Extremely Randomized Trees**, where for each tree the first split is made at random (random feature, random point), then the rest of a tree is allowed to be optimized. Sometimes performs better than a standard RF, sometimes not.

# Refs

What's better, Gradient Boosted Trees, or Random Forests?
http://fastml.com/what-is-better-gradient-boosted-trees-or-random-forest/
Apparently they are so close in performance that it's hard to tell which one is better. Usually boils down to parameter choice and tuning. But maybe in some cases GBTs are just a tiny little bit better.