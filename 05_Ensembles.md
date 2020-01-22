Ensembles are about combining (or gradually refining) lots of poor predictions (aka **weak learners**) into a very good prediction.

# Bagging

Bootstrapping can be used to improve estimations. The idea: bootstrap the data many times; build an estimator on each of these subsets of data, then average predictions of these estimators.

For linear estimators (like linear regression) this operation doesn't yield anything interesting, as in this case E(h(x)) = h(E(x)) = h(full data). But for non-linear h(), like **regression trees**, for example, intersting things may happen.

How to combine opinions of several classifiers into one output? One option is to encode output labels as one-hot encoding, and produce a vector of probabilities; then pick max coordinate (the most popular one). Another is to make classifiers output vectors of probabilities for different levels, without actual argmax-ing, and then average these outputs. The benefit here is that the result of averaging also works as a probability (∑=1), and also it happens to have lower variance.

> So from this part, it seems that the key power of bagging is that it takes a very discrete, run-down method that is by design jumpy, and also overfits like hell, and turns it into a smoothed landscape. Is this shifting from discrete to pseudo-continuous the main reason for why bagging works? Or is some hidden indirect serendipitous regularizaton also involved?

> And I still dont' undertand how individual trees can output anything like probabilities, but it will clear once I read about trees.

Refs:
* ESL p282

# Boosting

A popular, archetypical approach: **AdaBoost**. Uses most simple decision trees possible: trees with single split, aka **decision stumps**. But with every next tree, we increase weights of those that weren't classified correctly by the previous classifier (starting with equal weights for the first tree). Predictions are made by the majority rule, weighted by the accuracy of individual trees.

> How exactly individual observations are weighted, for all trees after the 1st one? Does it happen in loss calculation? What is the formula?

# Gradient Boosting

**Gradient Boosting Machine**, or **GBM**. Work in problems where the output is numerical, and each decision tree outputs two values for its split. Later outputs of all trees are summed together, producing the output of the ensemble.

New trees are added in a greedy manner, based on the purity score. Trees, as weak learners, are strongly constrained in terms of their complexity (we want them simple).

Instead of assigning higher weights to misclassified elements, we introduce a differentiable loss function, and assign higher weights to elements that have most effect on this loss function, in terms of its gradient in respect to .… UNCLEAR!

As a greedy algorithm, GB is prone to overfitting, but can be **regularized**. Stochastic GB only considers part of the data for each decision tree. 

Can (and often should) be used with custom loss functions: for example, weighing false-positives and false-negatives (undershoots and overshoots) very differently (see Grover 2018 below).

References:
* [Custom loss functions for Gradient Boosting](https://towardsdatascience.com/custom-loss-functions-for-gradient-boosting-f79c1b40466d), Prince Grover, 2018
* [Gentle Introduction to the Gradient Boosting Algorithm](https://machinelearningmastery.com/gentle-introduction-gradient-boosting-algorithm-machine-learning/), Jason Brownlee, 2016. Confusing.
* [Understanding Gradient Boosting Machines](https://towardsdatascience.com/understanding-gradient-boosting-machines-9be756fe76ab),  Harshdeep Singh, 2018. Confusing.