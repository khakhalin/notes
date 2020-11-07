# Bagging

#ensemble

Parent: [[05_Ensembles]]
Related: [[boosting]]

# Bagging

Bootstrapping can be used to improve estimations. **Bagging**: bootstrap the data repeatedly; each time build an estimator, then average predictions of these estimators. Why does it work though? For linear estimators (like linear regression) it doesn't, as E(h(x)) = h(E(x)) = h(full data). But for non-linear h(), like **regression trees**, it may smoothen idiosyncrasies of each individual h(x).

How to best combine opinions of several classifiers into one output? One option is to one-hot encode output labels, average them across models, then pick the maximal score (the most popular label). Another is to make each classifier output vectors of probabilities for different levels, instead of levels themselves; then average these probabilities. This latter approach is smoother (lower variance), and if each vector is ∑=1, then the final output would naturally ∑=1, which is helpful.

> Bias is apparently unchanged (ESL p285, stated as a fact, followed by a page of confusing "illustration"). Is it because no additional constraints are introduced by the procedure? How to intuit it? In which cases the bias WOULD BE changed? What sorts of modifications of h(x) procedure change the bias, and which sorts do not?

> So it seems that the key power of bagging is that it takes a very discrete, run-down method that is by design jumpy, and overfits like hell, and turns it into a smoothed landscape. Is this shifting from discrete to pseudo-continuous the main reason for why bagging works? Or are there some hidden indirect serendipitous regularizatons involved?

Note that bagging doesn't work if loss is heavily quantized, like for [[01loss]], aka hit/miss indicator. In this case, if you happen to get a bunch of saturated classifiers (all points→one class), there's no way to improve on these predictions by averaging them, as the average of a bunch of consts is still a const. Which means that for bagging to work, classifiers should be moody: either continuousy, or randomy.

A slightly more sophisticated **counter-example**: points uniformly fill a (0-1,0-1) R² square; 2 classes with a diagonal split (ESL p288). Any subsampling from this will look like a blobby squary cloud with a diagonal; a vertical/horizontal split will always fall kinda in the middle, and so the edges of the diagonal will never be classified properly.

Trees are also notoriouly bad in splitting 2D **XOR** in a unit square, as averaging across both x and y gives left≈right, and a greedy tree would just split at some random point, defined by data density variations, and not really by class borders.

> Great #todo : model that in a playground, just to get a feel of what it can and cannot do.

ESL p288 claims that bagged estimate is an approximate posterior Bayesian mean, which is something I don't get yet.

**Bayesian interpretaton:** Suppose you're trying to estimate ζ, given a training set Z, and we have a family of models {M}. Then we can marginalize posterior P(ζ|Z) over the family of models {M_i}: P(ζ|Z) = ∑P(ζ| M,Z)∙P(M|Z), summing across M in {M}. It means that posterior mean E(ζ) will also be a weighted average across models, weighted by probability of each of these models given the data: E(ζ|Z) = ∑E(ζ | M)P(M|Z), summing across M in {M}. In practice, **Committee methods** give equal weight to each observed model. (_I'm guessing it means they assume that with enough models samples, P(observing a model) will be taken care of just by design of this model-sampling procedure?_)

Alternatively, try to estimate P(M) somehow; say, if a model has a set of parameters θ, one could Bayes-flip P(M|Z) into P(Z|M), further marginalize P(Z|M) by these parameters as ∫P(Z|θ)P(θ|M)dθ, and compute it numerically. _Not sure what it means and how it helps..._

**Frequentist take**: let's do linear regression on results of all models, and find a mix of them that would minimize the L2 loss. If outputs of every model are f_i, together they form a matrix F, and we know how to do population regression to a true target Y: w = (FFᵀ)⁻¹FY. (**Population regression** just means that here instead of inner products, we imagine taking actual expectations E() over the true distribution that generages the data). As linear regression is a minimization of L2= var(Y-wF), then minimzed var(Y-wF) < var(Y-f_i) ∀i. In practice, population regression is of course impossible, so we can do the next best thing: linear regression on training data.

> But then they say something that I don't understand; how with different models having different complexity somehow everything would break. Why again? ESL p290