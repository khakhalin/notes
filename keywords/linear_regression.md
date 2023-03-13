# Linear Regression

Parents: [[02_Regression]], [[linalg]]
Related: [[l2]], [[gram-schmidt]], [[loess]]


**Linear model**: y ~ h(x) = $∑θ_j x_j$ = xθ, or in matrix form: h = Xθ ~ y. We assume that θ is a column-vector of coefficients, and each row of X is a vector x length p+1, with x_0 = 1 (intercept), followed by n "normal" coordinates for this point. The equation above defines a hyperplane in ℝ^p.

# L2 formula for LR

For a linear regression h = xᵀθ, and we can minimize L2 loss by differentiating by θ (partial derivative for each of the coordinates). For one point, we have: 
$∂J(θ)/∂θ_j = ∂/∂θ_j (h(θ,x)-y)^2$ (by definition of L2 loss)
= 2(h-y) ∙ ∂h/∂θ_j (simple chain rule)
= 2(h-y)∙x_j (by definition of h).

Minimum: dJ/dθ = 0, ⇒ Xᵀ(Xθ-y) = 0 (if we write all end-formulas for partial derivatives for individual θ_j above in matrix notation). Here X is a matrix m×p (where p is dimensionality of the space); each row of X is an input point in this space; y is a column-vector (height p) of target outputs, and 0 is a p-high vector as well. If XᵀX is nonsignular, we can open the brackets, send y to the right, multiply from the left on inverse, and get: θ = (XᵀX)⁻¹Xᵀy.

Interpretation: if y ~ h = Xθ, then h is in the column-space of X, which forms a subspace of R^N (N data points, p+1 dimensions, assuming x0≡1). To find θ, we project y to col-space using a projection matrix, which obviously minimizes residual (unexplained) variance, by making it orthogonal to the col-space.

# Same in linear algebra terms

For a univariate x (and no intercept!), we have a classic projection of a vector y (a column of observations) onto x (a column of input values): y is assumed to be equal to xθ+ε, where ε are some errors ⟂ to x. So: h = xθ = x ∙ xᵀy / xᵀx (by definition of projection). 

In a multivariate case, each x_i is a vector with several coordinates. If these coordinates come from a fully **orthogonal experiment design** (all variables x_j are ⟂ to each other), we can just project to each dimension separately, and the result will be the sum of these projections. That's possible because once you dot-product y with x_j, all terms but one would die: $y \cdot x_j = \sum_i y_i x_{ji} = \sum_i \sum_k θ_k x_{ki} x_{ji}$, as all x_j form a basis. Which means that essentially we can pretend that multiple linear regression is just a bunch of univariate regressions, performed consecutively.

If columns of X are correlated (not orthogonal), we have to build a full-form projection matrix:  h = X ∙ Xᵀy/XᵀX = X ∙ (Xᵀy) ∙ (XᵀX)⁻¹ . This closed form is horrible computationally, so a better choice is to do repetitive elimination (aka [[gram-schmidt]]) that also leads to a full form projection matrix.

Or we can perform a **gradient descent**: θ ← θ + α(h-y)x, with some learning rate α. As our L2 loss is a convex quadratic function, it has only one minimum, and so it always converges. 

Note that a "classic" one-variable linear regression, as we know it from math, is actually a 2D linear regression, as we are looking not in the space of y = kx, but in the space of y = kx + b. So we have to either replace a n-long vector x with a n×2 dimensional matrix X = $[1 1 \cdots 1 ; x]'$ , or perform something like "tiny elimination" by finding b explicitly, as mean(y), then unbiasing both x and y (removing means from them), and doing 1D projection on these unbiased vectors.

# Conceptual questions

How come Y~X and X~Y have different slopes? Presumably, if we have incomplete data, sometimes with a missing X and sometimes with a missing Y, and want to impute, we would use 2 different slopes for imputing X and Y. Is it true? And if yes, then how is it possible? (I mean, it's counter-intuitive, isn't it?)

How come [[pca]] (on a noisy cloud) looks better than LR? Wouldn't the PCA-line be a better predictor Y~X at least for X≫mean? Consider this logic: the PCA line is the longer axis of the Gaussian ellipsoid that best describes the distribution. Knowing x for sure is like taking a slice of this ellipsoid, which will also be a Gaussian. But the center of this Gaussian will be on the on the PCA axis, not on the regression axis? Sure, the model is different here (x=z+ε, y=z+ε), but isn't this approach more logical, if we have a cloud of points, and then for some more points one coordinate is known, but the other one is missing? Why does everyone use linear regression then?

> My guess for now is that PCA is really a better approach in this case, and people are just lazy. Regression is good if you control the experiment, _set_ the x, and then observe the Y. If x is not observed noisily, but is set manually, then y=x+ε is warranted. But if we just observe a cloud of points, it seems more logical to assume that both x and y are observed with errors, and so PCA would be a better model. No?

Now, back to the linear regression situation. A common explanation for why we have the "(y~x) ≠ (x~y)" situation is that in "y~x" scenario, X is known perfectly, and $Y = X + ε$. That's why we only do (Δy)². But what if X is also observed with an error, just with a lower error? Like, both are imperfect, but X has a better accuracy. Would it mean that we'd have to use something roughly in-between a LR and a PCA?

> It appears so, as it would mean reaching from a point to the "fit line" with an ellipsoid (as opposed to a circle for PCA, or straight line for linear regression), creating a slanted line. Not an orthogonal drop (PCA), but not a vertical line (regression) as well. Intuitively that's how an "in-between" would look like, but how to formalize it mathematically?

Next question: consider the following Bayesian task: we have a cloud of points, and also a single erroneously observed point of with estimations X and Y, what would be our best guess about its "true" position?

# Refs

Linear algebra and statistics behind linear regression (good clear chapter):
https://www.stat.cmu.edu/~cshalizi/mreg/15/lectures/13/lecture-13.pdf