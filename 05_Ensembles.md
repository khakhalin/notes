Ensembles are about combining (or gradually refining) lots of poor predictions (aka **weak learners**) into a very good prediction (aka "Wisdom of Crowds": popularized by a 2004 [book](https://en.wikipedia.org/wiki/The_Wisdom_of_Crowds) by Surowiecki).

# Bagging

Bootstrapping can be used to improve estimations: bootstrap the data repeatedly; each time build an estimator, then average predictions of these estimators. Why would it work though? For linear estimators (like linear regression) it wouldn't, as in this case E(h(x)) = h(E(x)) = h(full data). But for non-linear h(), like **regression trees**, for example, it may help to smoothen idiosyncrasies of individual h(x)-es.

How to best combine opinions of several classifiers into one output? One option is to one-hot encode output labels, average them up across models, produce a vector, then pick max coordinate (the most popular label). Another is to make classifiers output vectors of probabilities for different levels, without the last step (actual argmax-ing), and then average these outputs. It's smoother (lower variance), and also if each vector is ∑=1, then the final output would naturally ∑=1 as well.

> Bias is apparently unchanged (ESL p285, stated as a fact, followed by a page of confusing "illustration"). Is it because no additional constraints are introduced by the procedure? How to intuit it? In which cases the bias WOULD BE changed? What sorts of modifications of h(x) procedure change the bias, and which sorts do not?

> So from this part, it seems that the key power of bagging is that it takes a very discrete, run-down method that is by design jumpy, and also overfits like hell, and turns it into a smoothed landscape. Is this shifting from discrete to pseudo-continuous the main reason for why bagging works? Or is some hidden indirect serendipitous regularizaton also involved?

Note that bagging doesn't work if loss is heavily quantized (like in **0-1 loss**, which is essentially accuracy). In this case if you get a saturated classifier (all points→one class), there's no way to improve on it. Moreover, for a flat classifier (returns 0 with probability p0, and 1 with p1) nothing would work as well, and bagging would worsen things (as bagging will lead to const classifier for whatever p is greater, as it would win on average). _So for this to work, we need to make the classifier as uneven and moody as possible, right?_

A slightly more sophisticated counter-example: points uniformly fill a (0-1,0-1) R² square; 2 classes, with a diagonal split (ESL p288). Any subsampling from this will look like a blobby squary cloud with a diagonal; a split will fall kinda in the middle, so the edges of the diagonal will never be resolved.

> Great #todo : model that in a playground, just to get a feel of what it can and cannot do.

ESL p288 claims that bagged estimate is an approximate posterior Bayesina mean, which is something I don't get yet.

**Bayesian interpretaton:** Suppose you're trying to estimate ζ, given a training set Z, and we have a family of models {M}. Then we can marginalize posterior P(ζ|Z) over the family of models {M_i}: P(ζ|Z) = ∑P(ζ| M,Z)∙P(M|Z), summing across M in {M}. It means that posterior mean E(ζ) will also be a weighted average across models, weighted by probability of each of these models given the data: E(ζ|Z) = ∑E(ζ | M)P(M|Z), summing across M in {M}. In practice, **Committee methods** give equal weight to each observed model. (_I'm guessing it means they assume that with enough models samples, P(observing a model) will be taken care of just by design of this model-sampling procedure?_)

Alternatively, try to estimate P(M) somehow; say, if a model has a set of parameters θ, one could Bayes-flip P(M|Z) into P(Z|M), further marginalize P(Z|M) by these parameters as ∫P(Z|θ)P(θ|M)dθ, and compute it numerically. _Not sure what it means and how it helps..._

**Frequentist take**: let's do linear regression on results of all models, and find a mix of them that would minimize the L2 loss. If outputs of every model are f_i, together they form a matrix F, and we know how to do population regression to a true target Y: w = (FFᵀ)⁻¹FY. (**Population regression** just means that here instead of inner products, we imagine taking actual expectations E() over the true distribution that generages the data). As linear regression is a minimization of L2= var(Y-wF), then minimzed var(Y-wF) < var(Y-f_i) ∀i. In practice, population regression is of course impossible, so we can do the next best thing: linear regression on training data.

> But then they say something that I don't understand; how with different models having different complexity somehow everything would break. Why again? ESL p290

**Stacked generalization** or **Stacking**: Say you have a way of building various models f_m, and training each of them on a dataset X. Models f_m may belong to the same class, or to different classes, but the important part is that they sufficiently different; that is, they are defined both by the training set X, and by some hyperparameters that come with the model itself. So while each model is a function of a dataset, f(X), two models f_m1(X) and f_m2(X) should still be different.

Consider training each model f_m() on a dataset  X minus observation xi. It creates a family $f_m^{-i}(x)$ for each model class f_m(), providing a good way of assessing the accuracy of f_m() via cross-validation: ......................…

p290

Refs:
* ESL p282-on

# Boosting

A popular, archetypical approach: **AdaBoost**. Uses most simple decision trees possible: trees with single split, aka **decision stumps**. But with every next tree, we increase weights of those that weren't classified correctly by the previous classifier (starting with equal weights for the first tree). Predictions are made by the majority rule, weighted by the accuracy of individual trees.

> How exactly individual observations are weighted, for all trees after the 1st one? Does it happen in loss calculation? What is the formula?

# Gradient Boosting

**Gradient Boosting Machine**, or **GBM**. Work in problems where the output is numerical, and each decision tree outputs two values for its split. Later outputs of all trees are summed together, producing the output of the ensemble.

New trees are added in a greedy manner, based on the purity score. Trees, as weak learners, are strongly constrained in terms of their complexity (we want them simple).

Instead of assigning higher weights to misclassified elements, we introduce a differentiable loss function, and assign higher weights to elements that have most effect on this loss function, in terms of its gradient in respect to .… UNCLEAR!!!

> What is it that I don't get here: 1) Are we only talking about regression, or about classification as well? Sometimes (say, when they talk about custom losses) it feels like classification may also be covered, but then why insisting that these are regression trees that return real numbers?  2) How come the function is differentiable if it depends on splitting, and thus has to be discontinuous, as flipping an assignment for a point would give this function a jump? 3) What it is exactly that we are changing, for which we're calculating the gradient? 4) How do we calculate this gradient in practice? 5) How do we use it in practice?

As a greedy algorithm, GB is prone to overfitting, but can be **regularized**. Stochastic GB only considers part of the data for each decision tree. 

Can (and often should) be used with custom loss functions: for example, weighing false-positives and false-negatives (undershoots and overshoots) very differently (see Grover 2018 below).

References:
* [Custom loss functions for Gradient Boosting](https://towardsdatascience.com/custom-loss-functions-for-gradient-boosting-f79c1b40466d), Prince Grover, 2018
* [Gentle Introduction to the Gradient Boosting Algorithm](https://machinelearningmastery.com/gentle-introduction-gradient-boosting-algorithm-machine-learning/), Jason Brownlee, 2016. Confusing.
* [Understanding Gradient Boosting Machines](https://towardsdatascience.com/understanding-gradient-boosting-machines-9be756fe76ab),  Harshdeep Singh, 2018. Confusing.

# Random forest

The idea: construct many trees, by bagging (partial data), but also by providing to every tree a random subset of features (aka **feature bagging**; typically √p features out of p total, or something like max(5, p/3), ref: [wiki](https://en.wikipedia.org/wiki/Random_forest#From_bagging_to_random_forests)). This is an improvement upon bagging, as it makes trees less correlated, more diverse. _Not sure if the sequence in which different values are used for splits is randomized for each tree, or allowed to be optimal. I'd expect that both approaches could be possible, depending on dimensionality; is it true? _

Alternatives: **Extra Trees** of **Extremely Randomized Trees**: for each tree, make the first split at random (random feature, random point), then allow the tree be optimized for the rest of splits.