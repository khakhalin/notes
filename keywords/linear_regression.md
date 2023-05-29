# Linear Regression

Parents: [[02_Regression]], [[linalg]]
Related: [[l2]], [[gram-schmidt]], [[loess]]

#regression #linalg


**Linear model**: the value of y for each point x is approximated by $h(x) = ∑θ_j x_j = xθ$, or in a matrix form (across all n available points): $h = Xθ$. Here we assume that θ is a column-vector of coefficients, and each row of X is a vector $x$ (aka "point") length p, with $x_0 = 1$ (intercept), followed by p-1 "normal" coordinates for this point. The equation above defines a hyperplane in $ℝ^p$.

# L2 formula for LR

For a linear regression $h = Xθ$ we can try to minimize L2 loss $J = \sum (h-y)^2$  by differentiating it by θ (a vector of partial derivatives for each of the coordinates of θ), then finding the mininum by setting this vector to $\vec{0}$. 

For one coordinate j, we have: 
$∂J(θ)/∂θ_j$ = 
= $∂/∂θ_j \cdot \sum (h(θ,x)-y)^2$ (by definition of L2 loss, summing across all points)
= $2 \sum(h-y) \cdot ∂h/∂θ_j$ (differentiating a square, noting that y is a given, and doesn't depend on θ)
= $2\sum (h-y)x_j$ (by definition of h)
= $2x_j^\top(h - y)$ (in matrix notation)

Minimum: dJ/dθ = 0, ⇒ Xᵀ(Xθ–y) = 0 (if we combine all partial derivatives for individual θ_j into one matrix equation). Here X is a n×p matrix (n observations, p dimensions); each row of X is an point in this p-dimensional space; y is a column-vector (height n) of target outputs, and 0 is a p-high vector.

If XᵀX is nonsignular, we can get a closed-form solution, by opening  the brackets, send y to the right, multiply from the left on inverse, and get: θ = (XᵀX)⁻¹Xᵀy.

Interpretation: if y ~ h = Xθ, then h is in the column-space of X, which forms a subspace of R^N (N data points, p+1 dimensions, assuming x0≡1). To find θ, we project y to col-space using a projection matrix, which minimizes residual (unexplained) variance, by making it orthogonal to the col-space.

# Same in linear algebra terms

For a univariate x (and no intercept!), we have a classic projection of a vector y (a column of observations) onto x (a column of input values): y is assumed to be equal to xθ+ε, where ε are some errors ⟂ to x. So: h = xθ = x ∙ xᵀy / xᵀx (by definition of projection). 

In a multivariate case, each x_i is a vector with several coordinates. If these coordinates come from a fully **orthogonal experiment design** (all variables x_j are ⟂ to each other), we can just project to each dimension separately, and the result will be the sum of these projections. That's possible because once you dot-product y with x_j, all terms but one would die: $y \cdot x_j = \sum_i y_i x_{ji} = \sum_i \sum_k θ_k x_{ki} x_{ji}$, as all x_j form a basis. Which means that essentially we can pretend that multiple linear regression is just a bunch of univariate regressions, performed consecutively.

If columns of X are correlated (not orthogonal), we have to build a full-form projection matrix:  h = X ∙ Xᵀy/XᵀX = X ∙ (Xᵀy) ∙ (XᵀX)⁻¹ . This closed form is horrible computationally, so a better choice is to do repetitive elimination (aka [[gram-schmidt]]) that also leads to a full form projection matrix.

Or we can perform a **gradient descent**: θ ← θ + α(h-y)x, with some learning rate α. As our L2 loss is a convex quadratic function, it has only one minimum, and so it always converges. 

Note that a "classic" one-variable linear regression, as we know it from math, is actually a 2D linear regression, as we are looking not in the space of y = kx, but in the space of y = kx + b. So we have to either replace a n-long vector x with a n×2 dimensional matrix X = $[1 1 \cdots 1 ; x]'$ , or perform something like "tiny elimination" by finding b explicitly, as mean(y), then unbiasing both x and y (removing means from them), and doing 1D projection on these unbiased vectors.

# Conceptual questions

How come Y~X and X~Y have different slopes? Presumably, if we have incomplete data, sometimes with a missing X and sometimes with a missing Y, and want to impute, we would use 2 different slopes for imputing X and Y. Is it true? And if yes, then how is it possible? (I mean, it's counter-intuitive, isn't it?) Coz, let's be honest, [[pca]] axis (on a noisy  cloud) looks better than a regression line. Wouldn't the PCA-line then be a better predictor Y~X, at least for X >> mean? 

> Consider this logic: the PCA line is the longer axis of the Gaussian ellipsoid that best describes the distribution. It defines an axis of symmetry, so it looks good. However if you look at a cross-section of this elipsoid, parallel to one of the coordinates, then, oddly, the intersection with the PCA axis won't sit in the center of this cross-section! An axis of a cylinder or a cone remains the center of any of its cross-sections, but for an elongated, non-spherical ellipsoid it's not the case. Now, to add insult to injury, an off-center cross-section of an ellipsoid is also an p-1 -dimensional ellipsoid, and an off-center cross-section of a Gaussian would also be a p-1 -dimensional Gaussian, so it will still be symmetrical, but the center of symmetry will be lying in the center of this cross-section, not in the point where it intersects with the axis!

> It's kinda hard to describe in words, but you can imagining cross-secting a 3D ellipsoid, lying on one of its symmetry planes, like a piece of bread. A cross-section through a point lying on the remaining symmetry axis only has a max in this point if it cross-sects the ellipsoid perpendicularly to this axis. If it's slanted, the maximum is off. In the extreme case, if you are cross-secting parallel to the axis, which in this case is equivalent to cutting along the axis, then the max is obviously in the beginning of the coordinates! either way, all in all, for a given X, the most probably value of Y corresponding to this X likes _below_ the PCA axis (aka symmetry line) of the Gaussian ellipsoid.

Now, back to the linear regression situation. A common explanation for why we have the "(y~x) ≠ (x~y)" situation is that in "y~x" scenario, X is known perfectly, and $Y = X + ε$. That's why we only do (Δy)². While in case of a PCA model, we have a hidden variable Z, and we assume that  x=az+ε, y=bz+ε. But what if X is also observed with an error, just with a lower error? Like, both are imperfect, but X has a better accuracy. Would it mean that we'd have to use something roughly in-between a LR and a PCA?

> It appears so, as it would mean reaching from a point to the "fit line" with an ellipsoid (as opposed to a circle for PCA, or straight line for linear regression), creating a slanted line. Not an orthogonal drop (PCA), but not a vertical line (regression) as well. Intuitively that's how an "in-between" would look like, but how to formalize it mathematically?

Next question: consider the following Bayesian task: we have a cloud of points, and also a single erroneously observed point of with estimations X and Y, what would be our best guess about its "true" position?

# Refs

Linear algebra and statistics behind linear regression (good clear chapter):
https://www.stat.cmu.edu/~cshalizi/mreg/15/lectures/13/lecture-13.pdf

Interactive linear regression playgrounds:
* https://www.geogebra.org/m/xC6zq7Zv - with GeoGebra; can only more points, cannot add or delete them