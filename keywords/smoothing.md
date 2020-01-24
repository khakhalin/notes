# Smoothing
#features #dsp

# Splines
Consider **Piecewise polynomials**. The simplest example ever: downsampling, which is the same as piecewise constant, or just replacing values with their means. Better: piecewise linear fit. Even better: piecewise linear continuous at knots. Continuity implies limitations (as in a+bx = c+dx), and so a lower number of parameters to fit.  For **Splines**, not only f(x), but also first derivative f'(x) are continuous on the edges of segments. Usually cubic, but not necessarily. There's apparently an efficient way to calculate them, called a **B-spline basis**, but park for now. (ESL p144, referencing Esl chapter 5 appendix) 

**Natural cubic spline**: is forced to become linear on all knots (with f''=0), with no curvature. Three benefits: 1) same polynomial can be safely extrapolated outside the range, as it fits edge data with a reasonable straight line (instead of a crazy overshoot that cubic splines always want to do), 2) two fewer degrees of freedom per spline to deal with, 3) arguably, look quite nice. Formulas may be solved explicitly, it is just annoying. [ref](https://towardsdatascience.com/numerical-interpolation-natural-cubic-spline-52c1157b98ac)

# Smoothing splines
Instead of picking knot points (for splines), let's use regularization. Loss = ∑(y-f(x))² + λ∫(f''(x))²dt, where λ is a **smoothing parameter**. Then apply it to a natural spline sequence with knots in unique values of x_i (that apparently happens to be an optimal function for this type of task). Apparently, there also exists an optimal basis of natural splines F (B-splines? _Or are B-splines different?_), so f = ∑Fi(x)θi, and after combining it with the equation fo loss, we get a generalized ridge regression:

L(θ, λ) = (y-Fθ)ᵀ(y-Fθ) + λθᵀΩθ, where Ω is a matrix with $Ω_{jk} = \int F'' _ j(t)F''_ k(t)dt$. It gives the solution: θ = (FᵀF + λΩ)⁻¹Fᵀy. There exist computationally effective way to calculate that all. 

An approximation for y can now be produced from here, by doing h = Fθ = F(FᵀF + λΩ)⁻¹Fᵀy = Sy, where S is a **smoother matrix**, or smoother operator. Linear in respect to y. This matrix S = F(FᵀF + λΩ)⁻¹Fᵀ is similar to a standard projection operator B(BᵀB)⁻¹Bᵀ; it's also symmetric positive semidefinite, but while H² = H, S² ≤ S (because of the shrinkage). As for a (reasonable?) projection operator tr(H) = the number of coordinates remaining in place = dimention of the projection space, we can use df$_ λ$= tr(S) an estimate for the **effective degrees of freedom**.

Another way to write S is to split it in a so-called **Reinsch form**: S = (I+λK)⁻¹, where K is not depended on λ, and is called a **penalty matrix**. (_I'm not immediately sure why it is possible._) The igenvectors can be found (ESL p155), and they don't depend on λ. The **Bias-Variance Tradeoff** is quite prominent in this case (ESL p159)

> #todo : ESL p161 to p181 skipped for now: Nonparametric Logistic Regression, Multididimensional splines, Reproducing Kernel Hilbert Spaces (RKHS), 

# Loess

**Locally Estimated Scatterplot Smoothing**, aka **Savitzky-Golay filter**. Say you have y~f(x), but very noisy. Set α as **smoothing paramter**: α = K/N, where K is the number of local points to fit a polynomial, out of total number of points N. If using cubic polynomials, α obviously has to be larger than (power+1)/N = 4/N. 

And also, for the fitting purposes, to calculate the value of smoothed function in x, we weigh different points x_i ; typically using $w_i = (1-d^3)^3$, where d is something like distance from x to x_i, but scaled to $[0,1]$: $d = |x-x_i|/\max_{ij}(|x_i-x_j|)$ for xi, xj ∈ {K neighbors}. This weird formula is called **tricube**, and it's a popular pseudo-gaussian kernel apparently.

Refs: [wiki](https://en.wikipedia.org/wiki/Local_regression), [nice lecture pdf](http://pj.freefaculty.org/guides/stat/Regression-Nonlinear/Nonparametric-Loess-Splines/Nonparametric-1-lecture.pdf)

# Wavelets

> Also parked for now: Wavelet smoothing (around ESL p170)

# Kernel Smoothing Method

> ESL 192