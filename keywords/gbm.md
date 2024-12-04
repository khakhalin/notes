# Gradient Boosting and GBMs

Parents: [[05_Ensembles]]
Related: [[boosting]], [[lightgbm]]

#ensemble #todo #halfthere


To process:
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

# Refs

* [How to explain gradient boosting](https://explained.ai/gradient-boosting/index.html), Parr, Howard. Very good description!
* [Custom loss functions for Gradient Boosting](https://towardsdatascience.com/custom-loss-functions-for-gradient-boosting-f79c1b40466d), Prince Grover, 2018
* Friedman, J. H. (2001). Greedy function approximation: a gradient boosting machine. Annals of statistics, 1189-1232. - Mathy paper with 10k citations explaining everything about gradient boosting. https://statweb.stanford.edu/~jhf/ftp/trebst.pdf
* A bunch of confusing explanations that I either didn't like, or that explain boosting in general rather than gradient boosting specifically:
    * [GB from scratch](https://medium.com/mlreview/gradient-boosting-from-scratch-1e317ae4587d)
    * [Gentle Introduction to the Gradient Boosting Algorithm](https://machinelearningmastery.com/gentle-introduction-gradient-boosting-algorithm-machine-learning/)
    * [Understanding Gradient Boosting Machines](https://towardsdatascience.com/understanding-gradient-boosting-machines-9be756fe76ab)