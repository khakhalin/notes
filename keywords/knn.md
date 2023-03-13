# 1NN and KNN

#classification

Parent: [[03_Classification]]
See also: [[validation]], [[hyperparameters]], Bias-Variance trade-off, kernel methods

Simplest archetypical approach: **One Nearest Neighbor**. Just pick the closest training case. This is an example of a **lazy** approach: instead of generating estimations upfront (that would be called **eager**), we only generate them at retrieval. A better. and more practical, approach: **K nearest neighbors** (aka KNN).

While KNN is lazy, for analysis purposes we can calculate predictions on a grid, and thus identify borders between areas "assigned" to different categories. For k=1 (simple Nearest Neighbor) we get a Voronoi tesselation between all training points (disconnected and jaggedy, as each point gets its own area). Which presents a case of extreme overfitting: all training data is correctly classified, but it looks scary. Higher k: smoother areas, making k a **hyperparameter**. But the **effective number of parameters** for KNN is higher (about N/k), as data points themselves serve as parameters. 

KNN can be used for numerical predictions as well (beyond simple classification); just use the average of y-values for nearest k x-points (in fact, that's how ESL introduces it).

**Can we use quadratic loss function for KNN?** No, coz it would go to 0 for k=1, so hyperparameter search would always recommend overfitting. We should use **Max Likelihood** instead: Say, you have data X and a qualitative output G with values g_k denoting several different classes. We can have P(G=g_k|X=x) = P_k(θ,X), and try to maximize ∏P for a given data. It's the same as maximising L = ∑log P(θ , g_i , x_i) in the space of θ.

How to find optimal k? We'll need a training and a testing set, then try different values of k, and find the point when the test error (or some other measure) is lowest for test data. See [[validation]].

> OK, there's something I don't understand here. If we introduce training and testing sets, why cannot we use quadratic loss? For a smooth enough underlying function, k=1 (overfitting) would almost certainly lead to higher L2 loss than a more decent "averaging" k. Right? Please check. #todo

KNN is an basic archetype for a whole family of fancy "local" methods. For example, if instead of "nearest" δi that are all or none we use fuzzy weights that are larger when you are close to each data w = K(distance), it's the same as just regressing on distance, which is a type of a **kernel method**. If we want some dimensions of X matter differently than others, we can use non-round kernels. **Local regression** is also similar in spirit (sorta a combination of linear regression and the idea of contextual locality). DL networks with RELU also imitate something like that by having different subnetworks effectively "enabled" for different inputs, depending on which neurons inactivate for each given set of inputs - at least it totally looks like that if you look at the network landscape. Arguably, from this point of view, any classification method uses KNN as the ultimate archetype, just defines **decision boundaries** differently - and that's the power (of other methods).