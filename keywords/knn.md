# 1NN and KNN

Parent: [[classification]]
See also: [[validation]], [[hyperparameters]], Bias-Variance trade-off, kernel methods

#classification


Simplest archetypical approach: **One Nearest Neighbor**. Just pick the closest training case. This is an example of a **lazy** approach: instead of generating estimations upfront (that would be called **eager**), we only generate them at retrieval. A better. and more practical, approach: **K nearest neighbors** (aka KNN).

While in principle KNN is lazy, for analysis purposes we can calculate predictions on a grid, and thus identify borders between areas "assigned" to different categories. For k=1 (simple Nearest Neighbor) we get a Voronoi tesselation between all training points (disconnected and jaggedy, as each point gets its own area). Which presents a case of extreme overfitting: all training data is correctly classified, but it looks scary. With higher k, we get increasingly smoother areas, which makes k a **hyperparameter**. But the **effective number of parameters** for KNN is higher (about N/k), as data points themselves serve as parameters. 

KNN can be used for numerical predictions as well (beyond simple classification); just use the average of y-values for nearest k x-points (in fact, that's how ESL introduces it).

Now, how to find optimal k? We'll need a training and a testing set, then try different values of k, to find the point when the test error (or some other measure) is lowest for test data. See [[validation]].

**Can we pick the best k using a quadratic loss function?** Probably not, as the training loss would go to 0 for k=1, so a hyperparameter search would always recommend overfitting. 

> 🔥 OK, there's something I don't understand here. If we introduce training and testing sets, why cannot we use quadratic loss? For a smooth enough underlying function, k=1 (overfitting) would almost certainly lead to higher L2 loss than a more decent "averaging" k. Right? Please check. #todo

Instead, we should use **Max Likelihood**! Say, you have data X and a qualitative output G denoting different classes $g_k$. We can calculate the probability of assigning points from X to each of the classes $g_i$:  $P(G=g_i|X=x) = P_i(θ,X)$, and try to maximize $∏P$ for a given data. Which is the same as maximising $L = ∑_i\log P(θ , g_i , x_i)$ in the space of θ.

KNN is an basic archetype for a whole family of "local" methods.
* For example, instead of matching each point x to the "nearest" training point, which is equivalent to linking them with yes-or-now functions $δi$, we can use fuzzy weights that are larger when you are close to each data point: K(distance). This would give us a type of a **kernel method**. 
* If we want some dimensions of X matter differently than others, we can use non-round kernels. 
* **Local regression** is also similar in spirit (sorta a combination of linear regression and the idea of contextual locality).
* DL networks with RELU also imitate something like that, by having different subnetworks effectively "enabled" or "turned off" (driven to 0 by negative activations) for different inputs.
* Arguably, from this point of view, any classification method uses KNN as the ultimate archetype, and just defines the **decision boundaries** differently (more powerfully than a naive KNN).