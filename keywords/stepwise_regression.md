# Stepwise Regression

Parent: [[02_Regression]]
See also: [[ridge_regression]], [[aic]]

#regularization

A (bad) compromise between performance and speed: automated stepwise selection of features based on the quality of fit, and a complexity penalty (regularization). Two main approaches: **Forward selection** and **Backward selection**. Or a combinationof both (*not sure how?*)

The maxim about stepwise regression is that it is useless for hypothesis testing, questionable for prediction, and OK for variable selection (see [[07_Features]]). 
* It can't be used for hypotheses, as p-values after this procedure are meaningless, and arguably shouldn't even be reported, as their direct probabilistic intepretation is impossible, and adjustment is too complicated (p-values aren't independent, so you cannot do FDR), making adjustment impossible in practice.
* It can be used for prediction, but usually full models (or, assumably, probably regularized models) are more powerful anyways.
* It's an OK method to select variables tho.

Some people (like Thompson cited below) are really vehemently against them tho. That's because they are usually too optimistic (the very concept of degrees of freedom becomes problematic after multiple testing), yet are strictly less powerful than proper regularization, and also typically don't replicate, as the sequence of descent depends on the data, so performing stepwise regression on different subsets of data is much more likely to yield different results, compared to a more sophisticated model.

Refs:
* [[Guyon2003variable]], [wiki](https://en.wikipedia.org/wiki/Stepwise_regression), ESL p59
* Steyerberg, E. W., Eijkemans, M. J., Harrell Jr, F. E., & Habbema, J. D. F. (2001). Prognostic modeling with logistic regression analysis: in search of a sensible strategy in small data sets. Medical Decision Making, 21(1), 45-56.
* Thompson, B. (2001). Significance, effect sizes, stepwise methods, and other issues: Strong arguments move the field. The Journal of Experimental Education, 70(1), 80-93. 

There's also a thing called **Forward-Stagewise Regression** that is based on correlations between remaining features and the current residual, and it is just plan horrible.

# Least Angle Regression

Similar to stepwise regression, but with penalized coefficients, as in shrinkage. The approach:
1. Standardize all variables
2. Set up the residual; initialize it as res = y-intercept
3. Find the variable x_i with a highest correlation with y, and add it to the mix. But at first set its θ_i to 0.
4. Move θ _for all {i} currently in the mix? or what?_ towards the least-squares value ⟨x_i , r⟩ (linearly; not proportionally to the error as in gradient descent). After ech movement of θ, update the residual, and calculate the total correlation of variables already in the mix (current Xθ) with the residual.
5. Calculate correlations of all runners-up (other x_j currently not in the mix) with the residual r. The moment some other x_j becomes as correlated with r as the current mix Xθ, add x_j to the mix (with its θ_j originally zeroed).
6. Repeat 4-5 until least squared is reached.

As a result of this procedure, we get a piecewise-linear change of θ with "iteration time". We can then go back in time, and pick various levels of simplified approximations for the least squares solution. The results are pretty similar to that for lasso, except for some histeresis effects around θ=0 point.

> Not sure what are the benefits of this approach though.