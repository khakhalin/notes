# Stepwise Regression

Parent: [[regression]]
See also: [[ridge_regression]], [[aic]]

#regularization


A (bad) compromise between performance and speed: automated stepwise selection of features based on the quality of fit, and a complexity penalty (regularization). Two main approaches: **Forward selection** and **Backward selection**. Basically, you regress y against all features x_i one by one, and pick the best regression (based on r2 or p-value). Then you eliminate the effect of this variable, get the residual, and repeat the process (either until a certain r2 is explained, or a certain number of variables is reached, or p-values stop being significant)

The maxim about stepwise regression is that it is useless for hypothesis testing, questionable for prediction, but OK for variable selection (see [[07_Features]]). 
* It can't be used for hypotheses, as p-values after this procedure are meaningless, and arguably shouldn't even be reported, as their probabilistic intepretation is impossible, and adjustment is too complicated (p-values aren't independent, so you cannot do FDR).
* It can be used for prediction, but full models (properly regularized models) are more powerful.
* It may be an OK method to select variables tho.

Some people (like Thompson, cited below) are vehemently against this metnhod. That's because they are usually too optimistic (the very concept of degrees of freedom becomes problematic after multiple testing), yet are strictly less powerful than proper regularization, and don't replicate, as the sequence of variable selection depends on the data, so performing stepwise regression on different subsets of data is much more likely to yield different results, compared to a more sophisticated model.

# Least Angle Regression

Similar to stepwise regression, but with penalized coefficients, as in shrinkage. The approach:
1. Standardize all variables
2. Set up the residual; initialize it as res = y-intercept
3. Find the variable x_i with a highest correlation with y, and add it to the mix. Set its θ_i to 0.
4. Loop:
    5. Move θ for all {i} currently in the mix towards their least-squares values ⟨x_i , r⟩ (linearly; not proportionally to the error as in gradient descent) by a small step.
    6. Update the residual, calculate the total correlation of all variables already in the mix (current Xθ) with the residual. As we gradually move towards the least-square solution, this correlation will monotonously decrease.
    7. Calculate correlations of individual runner-up (those variables  x_j that are currently not in the mix) with the residual r. If one of the x_j become as correlated with r as the current mix Xθ, add this x_j to the mix (with its θ_j set to 0).
    8. Repeat this process until the exact least squared is reached.

As a result of this procedure, we get a piecewise-linear change of θ with "iteration time". We can then go back in time, and pick various levels of simplified approximations for the least squares solution. The results are pretty similar to that for [[lasso]], except for some histeresis effects around θ=0 point.

> Not sure what are the benefits of this approach though, compared to [[lasso]]

# Refs

https://en.wikipedia.org/wiki/Stepwise_regression

[[Guyon2003variable]], [wiki](https://en.wikipedia.org/wiki/Stepwise_regression), ESL p59

Steyerberg, E. W., Eijkemans, M. J., Harrell Jr, F. E., & Habbema, J. D. F. (2001). Prognostic modeling with logistic regression analysis: in search of a sensible strategy in small data sets. Medical Decision Making, 21(1), 45-56.

Thompson, B. (2001). Significance, effect sizes, stepwise methods, and other issues: Strong arguments move the field. The Journal of Experimental Education, 70(1), 80-93. 

There's also a thing called **Forward-Stagewise Regression** that is based on correlations between remaining features and the current residual, and it seems to be just plan horrible.