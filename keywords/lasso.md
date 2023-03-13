# Lasso and Elastic Net

#regularization

Parents: [[02_Regression]], [[regularization]]
See also: [[ridge_regression]]

**L1 regularization**, aka **basis pursuit**, aka **Lasso**: require ∑|θ_i| < t, or add λ∑|θ_i| penalty to the loss (summing for i = 1 to p, meaning no punishment for the intercept $θ_0$). Unlike for L2, it has no closed form solution (because abs() is harder to optimize). The word Lasso is actually a weird abbreviation from Least Absolute Shrinkage and Selection Operator.

Overall, while [[ridge_regression]] tends to scale θ_i values down with a coefficient, lasso tends to slide (shift) them towards zero by a certain value, until they bury in 0 and become 0. For an orthonormal X, this can be proven theoretically (ESL p71). The most critical benefit of L1 regularization is that is performs **covariate selection**: when some x_i are strongly correlated, L2 would tend to distribute θ equally between them, while L1 would kill one, and glorify the other.

Visual representation: romboid and a quadratic (elliptic) optimum nearby. A circle (ridge) projected on an elliptic field always has a global minimum. A corner made of two lines (lasso) would either reach a mininum on a line (non-zero optimal θ), or at the corner (zero θ). Graphically, one can see that θ sets to 0 the moment the unpenalized minimum leaves the band that continues the diamond (draw it to see it).

There's a generalizaton of both L1 and L2: one can try to use a penalty of λ∑|θ_i|^q, where q is some value in R. For q=2 we get ridge, for q=1 lasso, and for q in (0,1) - something in-between. Which is nice, but computationally expensive.

**Elastic net penalty**: An mix (weighted sum) of both Ridge and Lasso that can be used to approximate these generalized romboid shapes. It's way easier to calculate, but also shares the ability to set some junk coefficients to exactly 0 (because of pointy points )