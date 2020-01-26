# Ensemble methods

Ensembles are about combining (or gradually refining) lots of poor predictions (aka **weak learners**) into a very good prediction (aka "Wisdom of Crowds": popularized by a 2004 [book](https://en.wikipedia.org/wiki/The_Wisdom_of_Crowds) by Surowiecki).

# Bagging

Bootstrapping can be used to improve estimations: bootstrap the data repeatedly; each time build an estimator, then average predictions of these estimators. Why does it work though? For linear estimators (like linear regression) it doesn't, as E(h(x)) = h(E(x)) = h(full data). But for non-linear h(), like **regression trees**, it may smoothen idiosyncrasies of each individual h(x).

How to best combine opinions of several classifiers into one output? One option is to one-hot encode output labels, average them up across models, produce a vector, then pick max coordinate (the most popular label). Another is to make classifiers output vectors of probabilities for different levels, without the last step (actual argmax-ing), and then average these outputs. It's smoother (lower variance), and also if each vector is ∑=1, then the final output would naturally ∑=1 as well.

> Bias is apparently unchanged (ESL p285, stated as a fact, followed by a page of confusing "illustration"). Is it because no additional constraints are introduced by the procedure? How to intuit it? In which cases the bias WOULD BE changed? What sorts of modifications of h(x) procedure change the bias, and which sorts do not?

> So from this part, it seems that the key power of bagging is that it takes a very discrete, run-down method that is by design jumpy, and also overfits like hell, and turns it into a smoothed landscape. Is this shifting from discrete to pseudo-continuous the main reason for why bagging works? Or is some hidden indirect serendipitous regularizaton also involved?

Note that bagging doesn't work if loss is heavily quantized (like in **0-1 loss**, which is essentially accuracy). In this case, if you get a saturated classifier (all points→one class), there's no way to improve on it. Moreover, for random classifier with constant probabilities (returns 0 with probability p0, and 1 with p1) bagging would only worsen things (as bagging will lead to const classifier for whatever p is greater, as it would win on average). _So for this to work, we need to make the classifier as uneven and moody as possible, right?_

A slightly more sophisticated **counter-example**: points uniformly fill a (0-1,0-1) R² square; 2 classes with a diagonal split (ESL p288). Any subsampling from this will look like a blobby squary cloud with a diagonal; a split will fall kinda in the middle, and the edges of the diagonal will never be resolved.

Trees are also notoriouly bad in splitting 2D **XOR** in a unit square, as averaging across both x and y gives left≈right, and a greedy tree would just split at some random point, defined by data density variations, and not really by class borders.

> Great #todo : model that in a playground, just to get a feel of what it can and cannot do.

ESL p288 claims that bagged estimate is an approximate posterior Bayesina mean, which is something I don't get yet.

**Bayesian interpretaton:** Suppose you're trying to estimate ζ, given a training set Z, and we have a family of models {M}. Then we can marginalize posterior P(ζ|Z) over the family of models {M_i}: P(ζ|Z) = ∑P(ζ| M,Z)∙P(M|Z), summing across M in {M}. It means that posterior mean E(ζ) will also be a weighted average across models, weighted by probability of each of these models given the data: E(ζ|Z) = ∑E(ζ | M)P(M|Z), summing across M in {M}. In practice, **Committee methods** give equal weight to each observed model. (_I'm guessing it means they assume that with enough models samples, P(observing a model) will be taken care of just by design of this model-sampling procedure?_)

Alternatively, try to estimate P(M) somehow; say, if a model has a set of parameters θ, one could Bayes-flip P(M|Z) into P(Z|M), further marginalize P(Z|M) by these parameters as ∫P(Z|θ)P(θ|M)dθ, and compute it numerically. _Not sure what it means and how it helps..._

**Frequentist take**: let's do linear regression on results of all models, and find a mix of them that would minimize the L2 loss. If outputs of every model are f_i, together they form a matrix F, and we know how to do population regression to a true target Y: w = (FFᵀ)⁻¹FY. (**Population regression** just means that here instead of inner products, we imagine taking actual expectations E() over the true distribution that generages the data). As linear regression is a minimization of L2= var(Y-wF), then minimzed var(Y-wF) < var(Y-f_i) ∀i. In practice, population regression is of course impossible, so we can do the next best thing: linear regression on training data.

> But then they say something that I don't understand; how with different models having different complexity somehow everything would break. Why again? ESL p290

**Stacked generalization** or **Stacking**: Say you have a way of building various models f_m, and training each of them on a dataset X. Models f_m may belong to the same class, or to different classes, but it is important that they are sufficiently different: that is, they need to be defined both by the training set X, and by some hyperparameters that come with the model itself. So while each model is a function of a dataset, f(X), two models f_m1(X) and f_m2(X) should still be different.

Consider training each model f_m() on a dataset  X minus observation xi. It creates a family of predictors $f_m^{-i}(x)$ for each model class f_m(), providing a good way of assessing the accuracy of m-type models f_m() via cross-validation: just predict y_i by each f^{-i}, and sum all errors. But instead of just picking the best model, we will build the best linear combination of all these models, by finding an optimal vector of weights w, so that
$\sum_i \big( y_i - \sum_m w_m f_m^{-i}(x_i)\big)^2$ is minimal. In other words, we find a set of coefficients w, so that the linear combination of all f_m gives the best total cross-validation across all "remove one x_i". It also helps to constrain w to w_i>0 ∀i and ∑w_i = 1, as it turns it into a quadratic problem. That's called "stacking".

Refs:
* ESL p282-290

# Bumping

A way to find better models by randomly moving in model space, avoiding local minima. Train a bunch of models on different subsamples of X, then test each on full X, and pick the best. As in this case optimization is kinda shifted towards the end, it's probably better to use quite undersampled training sets. Say, for a **XOR** case, if you test enough small subsamples, at least one of them will probably split the data decently, even though training on a full dataset may be hopeless for many types of models.

It's important to keep all models in  the comparison group similar in their complexity (for trees, same number of terminal nodes).

# Boosting

Boosting uses simplest decision trees possible:  single split into 2 categories, aka **decision stumps**. But with every next group of trees, we increase the weights for those elements that weren't classified correctly by the previous generation of classifiers (starting with equal weights at the beginning of the process, for the first tree). These weights may either be explicitly included in the error calculation, or be used as probabilities of each data point  appearing in next training subset (the result is the same). At the end, predictions are made by **weighted majority rule**: weighted average by the **accuracy of individual trees**, followed by argmax.

The most popular, archetypical approach: **AdaBoost**, aka **Adaptive Boosting**. For a binary discrete case:
1. Start with all points x_i having identical weights {W}. ∑wi = 1, so w_i = 1/n.
2. For each available coordinate, find the best split (aka stump), with smallest total error E = ∑wi ϵi, where ϵi = 1∙(h(xi)==yi).
3. Across all coordinates, find a split that minimizes [[gini]] index for this split. For all loops after 1st (when weights are different), either use weighted gini index, or randomly draw points with p ∝ w (with repetition, keeping the total number of points about constant).
4. Calculate the weight of this stump as α = ½ log((1−E)/E) . This formula →+∞ for E→0, →-∞ for E→1, and 0 around chance, when E=1-E. In practice, to avoid ∞, a tiny ε is added both to both numerator and denominator.
5. Increase the weights of misclassified samples: w_i ← w_i ∙ exp(α); decrease weights of correctly classified samples by multiplying by exp(−α); then normalize to ∑w = 1.
6. Go to step 2.

> Is it true that in practice randomly resampling points is better than using a weighted formula? Nobody says it openly, but if it weren't the case, why would people tell this whole resampling story?

7. Once everything is classified, rejects trees with accuracy less than 50%. _Not all descriptions mention that._
8. Produce an average of tree outputs, weighted by α .

Because each split is a single plane (line), || to all other variables, the decision border looks like a combination of these planes (lines). But all are ⊥; there are no curves or non-right angles there. 

For multi-class classification, either create lots of binary classifiers (each class against all others), or encode each class as a superposition of several binary "features" that may be present or absent (say, bunnies are cute and jumpy, cats are cute but not jumpy; crickets are jumpy but not cute etc.), then use AdaBoost to identify the presence of features ([ref](https://engineering.purdue.edu/kak/Tutorials/AdaBoost.pdf)).

In many ways, AdaBoost goes against the conventional basis for classification: it doesn't try to build a good classifier (but instead uses a lot of crappy ones); it does not try to identify important variables, or isolate important features using dimentionality reduction (instead, thrives in this multi-var space).

Refs: [Akash Desarda](https://towardsdatascience.com/understanding-adaboost-2f94f22d5bfe); [wiki](https://en.wikipedia.org/wiki/AdaBoost); [Tommi Jaakkola](http://people.csail.mit.edu/dsontag/courses/ml12/slides/lecture13.pdf) lecture note; [Explaining AdaBoost](http://rob.schapire.net/papers/explaining-adaboost.pdf) by Robert Schapire; [YouTube video by StatQuest](https://www.youtube.com/watch?v=LsK-xG1cLYA) (good); [Slides by Avinash Kak, Purdue](https://engineering.purdue.edu/kak/Tutorials/AdaBoost.pdf) (very good).

# Gradient Boosting

**Gradient Boosting Machine**, or **GBM**. Work in problems where the output is numerical, and each decision tree outputs two values for its split. Later outputs of all trees are summed together, producing the output of the ensemble.

New trees, strongly constrained in terms of their complexity (we want them simple), are added in a greedy manner, based on the **purity score**: [[gini]].

Instead of assigning higher weights to misclassified elements, we introduce a differentiable loss function, and assign higher weights to elements that have most effect on this loss function, in terms of its gradient in respect to .… **UNCLEAR!!!**

#todo

https://medium.com/mlreview/gradient-boosting-from-scratch-1e317ae4587d

> What is it that I don't get here: 1) Are we only talking about regression, or about classification as well? Sometimes (say, when they talk about custom losses) it feels like classification may also be covered, but then why insisting that these are regression trees that return real numbers?  2) How come the function is differentiable if it depends on splitting, and thus has to be discontinuous, as flipping an assignment for a point would give this function a jump? 3) What it is exactly that we are changing, for which we're calculating the gradient? 4) How do we calculate this gradient in practice? 5) How do we use it in practice?

As a greedy algorithm, GB is prone to overfitting, but can be **regularized**. Stochastic GB only considers part of the data for each decision tree. 

Can (and often should) be used with custom loss functions: for example, weighing false-positives and false-negatives (undershoots and overshoots) very differently (see Grover 2018 below).

References:
* [GB from scratch](https://medium.com/mlreview/gradient-boosting-from-scratch-1e317ae4587d) by Prince Grover
* [Custom loss functions for Gradient Boosting](https://towardsdatascience.com/custom-loss-functions-for-gradient-boosting-f79c1b40466d), Prince Grover, 2018
* [Gentle Introduction to the Gradient Boosting Algorithm](https://machinelearningmastery.com/gentle-introduction-gradient-boosting-algorithm-machine-learning/), Jason Brownlee, 2016. Confusing.
* [Understanding Gradient Boosting Machines](https://towardsdatascience.com/understanding-gradient-boosting-machines-9be756fe76ab),  Harshdeep Singh, 2018. Confusing.

## Bagging-Boosting comparison

* Bagging tends to decrease variance, but not necessarily bias. Boosting tends to reduce both variance and bias.
* Conversely, Bagging does not overfit, while Boosting can overfit easily, by slicing space very thin around every point.
* Except for the very last step, bagging can happen in parallel, while boosting is by definition sequential, and so not easily parallelizable.
* Compared to many other methods, both bagging and boosting are very fast.

Refs: [1](https://quantdare.com/what-is-the-difference-between-bagging-and-boosting/), [2](https://towardsdatascience.com/understanding-adaboost-2f94f22d5bfe)

# Random forest

The idea: construct many full trees, by bagging (partial data), but also by providing to every tree a random subset of features (aka **feature bagging**; typically √p features out of p total, or something like max(5, p/3), ref: [wiki](https://en.wikipedia.org/wiki/Random_forest#From_bagging_to_random_forests)). This is an improvement upon bagging, as it makes trees less correlated, more diverse. 

> Not sure if the sequence in which different values are used for splits is randomized for each tree, or allowed to be optimal. I'd expect that both approaches could be possible, depending on dimensionality; is it true?

Variant: **Extra Trees** of **Extremely Randomized Trees**, where for each tree the first split is made at random (random feature, random point), then the rest of a tree is allowed to be optimized.

All trees usually have an equal vote in the final classification.