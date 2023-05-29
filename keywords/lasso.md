# Lasso and Elastic Net

Parents: [[02_Regression]], [[regularization]]
See also: [[ridge_regression]]

#regularization


**L1 regularization**, aka **basis pursuit**, aka **Lasso**: either require ∑|θ_i| < t (where t is some small value), or add a λ∑|θ_i| penalty to the loss (where i runs from 1 to p, not from 0 to p, as there should be no punishment on the intercept $θ_0$). Unlike L2, this one has no closed-form solution (as abs() is hard to optimize). The word Lasso is actually a weird abbreviation for Least Absolute Shrinkage and Selection Operator.

Overall, while [[ridge_regression]] tends to scale θ_i values down with a certain coefficient, lasso tends to slide (shift) them towards zero by a certain value, until they one by one become 0. For an orthonormal X, this can be proven theoretically (ESL p71). The most critical benefit of L1 regularization is that is performs **covariate selection**: when some x_i are strongly correlated, L2 would tend to distribute θ equally between them, while L1 would kill one, and glorify the other.

Visual representation: romboid around the coordinate center (correpsonding to the λ-term), and a quadratic (elliptic) optimum nearby (corresponding to the neighborhood of an overfitting solution). A circle (L2, ridge) projected on an elliptic field always has a global minimum (keep reducing the ellipse until it touches the circle of L2 in only one point). On the other hand, a corner made of two lines (L1) would either reach a mininum at the corner of it (if this corner is the last point to be caught inside the ellipse, corresponding to zero θ_i for some of the coordinates as an optimal solution), or on a line between corners (corresponding to a non-zero optimal θ). Graphically, θ is sets to 0 the moment the unpenalized minimum (the overfitting point) leaves the band that continues the diamond in one of the directions.

# Elastic Net Penalty

There's a generalizaton of both L1 and L2: one can try to use a penalty of λ∑|θ_i|^q, where q is some value in R. For q=2 we get ridge, for q=1 lasso, and for q in (0,1) - something in-between. This is conceptually nice, but computationally quite expensive.

A better alternative is a mix (weighted sum) of both Ridge and Lasso that can can well approximate these generalized romboid shapes. It is easy to calculate, and it combines the best aspects of both L1 and L2. Like L1, it can set some junk coefficients to exactly 0 (because of pointy points on axes), but like L2 it is convex (🔥 which may lead to a better convergence, right?)
