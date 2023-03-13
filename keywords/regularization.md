# Regularization

Parents: [[06_DL]], [[02_Regression]]

Regression and classic ML:
* [[ridge_regression]] - aka Tikhonov regularization, or L2 regularization
* [[lasso]] - L1 regularization

Deep Learning:
* [[dropout]] - can be treated as a type of regularization
* [[weight_decay]] - doesn't it feel surprisingly neuromorphic?

Overall, regularization is more important, and particularly efficient, when data is scarse. When we have a lot of data, overfitting is less of a problem, and regularization is often not helpful. 

> Is there any hard empyrical math to back that? How to tell where the threshold lies, in terms of data size vs n parameters?

# On the difference between L1 and L2

**L2 regularization**: use a modified loss = Loss(Data|Model) + λ∑ω² where ω are model weights, and λ is a hyperparameter known as **Regularization rate**. High values of λ push {ω} towards normal distribution, while low λ make the distribution of {ω} closer to uniform. Discourages sparseness of {ω}, as having many small weights becomes better than having a few solid ones. So, in a way, L2 regularization is pro-democracy, and anti-parsimony. As λ is increased, weights are pressed down asymptotically.

Alternative: **L1 regularization**, aka **Lasso** (least absolute shrinking). Loss = Loss(Data|Model) +  λ∑abs(ω) . Aggressive shrinkage of small ω; encourages parsimony; discourages "leakage of features". As λ is increased, weights, one by one, suddenly drop dead from something to zero.

**Elastic net**: some sort of combination of both, with two different λ. Flexible, nice.

# Refs

On dropout, but some general statements about regularization as well:
https://machinelearningmastery.com/dropout-for-regularizing-deep-neural-networks/