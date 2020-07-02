# Regularization

Parents: [[06_DL]], [[02_Regression]]
* See Ridge regression and Tikhonov regularization in [[02_Regression]] (maybe we shoudl take them out)
See also:    
* [[dropout]] - can be treated as a type of regularization
* [[weight_decay]] - same

**L2 regularization**: use a modified loss = Loss(Data|Model) + λ∑ω² where ω are model weights, and λ is a hyperparameter known as **Regularization rate**. High values of λ push {ω} towards normal distribution, while low λ make the distributin of {ω} closer to uniform. Discourages sparseness of {ω}, as having many small weights becomes better than having a few solid ones. So, in a way, L2 regularization is pro-democracy, and anti-parsimony. As λ is increased, weights are pressed down asymptotically.

Alternative: **L1 regularization**, aka **Lasso** (least absolute shrinking). Loss = Loss(Data|Model) +  λ∑abs(ω) . Aggressive shrinkage of small ω; encourages parsimony; discourages "leakage of features". As λ is increased, weights, one by one, suddenly drop dead from something to zero.

**Elastic net**: some sort of combination of both, with two different λ. Flexible, nice.