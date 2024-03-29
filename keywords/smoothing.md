# Smoothing

#features #dsp

Parents: [[02_Regression]], [[04_Features]]
See also: [[kernels]], [[loess]]

There are **3 broad approaches to model smoothing**:
* Roughness penalty
* Explicitly local methods, like local regression; see kernel methods ([[kernels]])
* Constrained basis functions and dictionaries

# Regression with roughness penalty

Use basic RSS (Residual Sum of Squares) with an explicit penalty: L = RSS(f) + λJ(f), where J grows as f() becomes too rough. For example, defining J as λ∫(f'')²dx leads to **cubic smoothing splines** as an optimal solution (see below). Roughness penalty can be interpreted in a Bayesian way, as a log-prior.

# Smooth basis functions

This type includes linear and polynomial regressions, but the idea is that you pick a basis of functions in R^n, and project into it: $f = \sum_i θ_i h_i(x)$. Examples: **polynomial splines**; **radial basis functions** [[RBF]] with K(μ, x) defined around several centroids μ that can itself be optimized. Gaussian kernels are still popular. Feed-forward **Deep Learning** also conceptually belongs here; it's just that its basis functions are defined by the network design (the space of all functions that can be achieved for a given network depth and activation functions).

# Splines

Consider **Piecewise polynomials**. The simplest example ever: naive downsampling (dropping most points), which is equivalent to selecting some bins, and then picking only one point from every bin to represent the entire bin. A slightly better one: binning, and replacing all points with an average for this bin. Better yet: piecewise linear fit. Even better: piecewise linear fit that is continuous at knots (bin borders). This continuity implies some limitations (two lines should meet at the edge of each bin, as in a+bx = c+dx), so we'll have fewer parameters to fit. 

For **Splines**, we want not only f(x), but also the first derivative f'(x) to be continuous at the edges of segments. Usually cubic splines, but not necessarily.
* There's apparently an efficient way to calculate them, called a **B-spline basis**, but let's park it for now. (ESL p144, referencing Esl chapter 5 appendix) 

**Natural cubic spline**: is forced to become linear on all knots (with f''=0), with no curvature. Three benefits: 1) allows some extrapolation outside the range, as it fits edge data with a reasonable straight line instead of a crazy cubic overshoot, 2) two fewer degrees of freedom per spline to deal with, 3) arguably, they look nice. Formulas may be solved explicitly, it is just annoying. [ref](https://towardsdatascience.com/numerical-interpolation-natural-cubic-spline-52c1157b98ac)

# Smoothing splines

Instead of picking knot points (for splines), we can also use regularization. Let's define loss as ∑(y-f(x))² + λ∫(f''(x))²dt, where λ is a **smoothing parameter**. Then apply it to a natural spline sequence with knots in unique values of x_i (that apparently happens to be an optimal function for this type of task). Apparently, there also exists an optimal basis of natural splines F (B-splines? _Or are B-splines different?_), so f = ∑Fi(x)θi, and after combining it with the equation fo loss, we get a generalized ridge regression:

L(θ, λ) = (y-Fθ)ᵀ(y-Fθ) + λθᵀΩθ, where Ω is a matrix with $Ω_{jk} = \int F'' _ j(t)F''_ k(t)dt$. It gives the solution: θ = (FᵀF + λΩ)⁻¹Fᵀy. There exist computationally effective way to calculate that all. 

An approximation for y can now be produced from here, by doing h = Fθ = F(FᵀF + λΩ)⁻¹Fᵀy = Sy, where S is a **smoother matrix**, or smoother operator. Linear in respect to y. This matrix S = F(FᵀF + λΩ)⁻¹Fᵀ is similar to a standard projection operator B(BᵀB)⁻¹Bᵀ; it's also symmetric positive semidefinite, but while H² = H, S² ≤ S (because of the shrinkage). As for a (reasonable?) projection operator tr(H) = the number of coordinates remaining in place = dimention of the projection space, we can use df$_ λ$= tr(S) an estimate for the **effective degrees of freedom**.

Another way to write S is to split it in a so-called **Reinsch form**: S = (I+λK)⁻¹, where K is not depended on λ, and is called a **penalty matrix**. (_I'm not immediately sure why it is possible._) The igenvectors can be found (ESL p155), and they don't depend on λ. The **Bias-Variance Tradeoff** is quite prominent in this case (ESL p159)

> #todo : ESL p161 to p181 skipped for now: Nonparametric Logistic Regression, Multididimensional splines, Reproducing Kernel Hilbert Spaces (RKHS), 

# Wavelets

> Also parked for now: Wavelet smoothing (around ESL p170)

# Kernel Smoothing Method

> ESL 192