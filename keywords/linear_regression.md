# Linear Regression

Parents: [[02_Regression]], [[linalg]]
Related: [[gram-schmidt]]

**Linear model**: h(x) = (estimation for y) = $∑θ_j x_j$ = xᵀθ, or in matrix form: h = Xθ ~ y. We assume that xᵀ is a row-vector length p+1, with x_0 = 1 (intercept), followed by n "normal" variables on which we regress. The equation above defines a hyperplane in ℝ^p.

Questions:
* How come PCA (on a noisy cloud) looks better than LR? Wouldn't the PCA-line be a better predictor Y~X at least for X≫mean?
* How come Y~X and X~Y have different slopes? Presumably, if we have incomplete data, sometimes with a missing X and sometimes with a missing Y, and want to impute, we would use 2 different slopes for imputing X and Y. Is it true? And if yes, then how is it possible? (I mean, it's counter-intuitive, isn't it?)
> It feels like this one can be naively modeled. Get a cloud of points, damage some values, use different strategies to recover, calculate the total loss?
* A common explanation for the above situation is that X is known perfecly, while Y is unknown, and that's why we only do (Δy)². But what if X is also not known perfectly, just with a lower error? Like, both are imperfect, but X has a better accuracy. Would it mean that we'd have to use something roughly in-between a LR and a PCA?
> Would it actually mean reaching from a point to a line with an ellipsoid, creating a slanted line? Not orthogonal, but not vertical as well? Intuitively that's how an "in-between" would look like, but is it mathematically sound?
* Bayesian approach? If we have a cloud and an erroneously observed point of both X and Y, what would be our best guess about its "true" position?

# The linear algebra behind

Intepretations for h = Xθ:

For univariate x, we have a classic projection of a vector y (a column of observations) onto x (a column of input values): y is assumed to be equal to xθ+ε, where ε are some errors ⟂ to x. So: h = xθ = x ∙ xᵀy / xᵀx (by definition of projection). 

In a multivariate case, each x_i is a vector with several coordinates. If these coordinates come from a fully **orthogonal experiment design** (all variables x_j are ⟂ to each other), we can just project to each dimension separately, and the result will be the sum of these projections. That's possible because once you dot-product y with x_j, all terms but one would die: $y \cdot x_j = \sum_i y_i x_{ji} = \sum_i \sum_k θ_k x_{ki} x_{ji}$, as all x_j form a basis. Which means that essentially we can pretend that multiple linear regression is just a bunch of univariate regressions, performed consecutively. It also matches the general formula above: h = XXᵀy/XᵀX, as in this case the matrix XᵀX is diagonal. 

If however columns of X are correlated (not orthogonal), we have to do repetitive elimination (aka [[gram-schmidt]]) that leads to a full form projection matrix. Or we can perform a **gradient descent**: θ ← θ + α(h-y)x, with some learning rate α. As our L2 loss is a convex quadratic function, it has only one minimum, and so it always converges. 