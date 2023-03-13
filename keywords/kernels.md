# Kernel methods

Parents: [[02_Regression]], [[03_Classification]]
Examples: [[loess]], [[rbf]], [[smoothing]]
See also: [[svm]]

#kernel


Main idea: explicitly specify local neighborhoods, using a **kernel function** K(x0,x). For example, Gaussian Kernel: K = 1/λ exp( - |x-x0|² / 2λ). (Sometimes instead of λ people write σ², and maybe also add 1/2π as a normalization coefficient).

Then, for **regression**, fit around every point x0 using this kernel function, using its outputs as weights for x. For **classification**, use outputs of this function as new coordinates (potentially, or a larger overall dimensionality than the original space, to make drawing decision borders easier (aka **kernel tick**).

## Kernels and local regression

The simplest way to use kernels for local regression is to calculate a weighted average of y using these kernels (aka **Nadaraya–Watson kernel regression**): $f(x_0) = ∑ (y_i K(x_0,x_i)) / ∑ K(x_0,x_i)$.

Or we can choose some smooth f(x), and then optimize it, while using kernels in loss calculations: RSS(x) = $\sum_i K(x, x_i)\cdot(y_i-f(x_i))^2$ , where ∑ runs through all data points (x_i, y_i). If we assume $f(x_i) = θ_0 + θ(x_i-x)$, with its own $θ_0$ and $θ$ for every $x_i$, we get **local linear regression** (aka [[loess]]).

## Kernels for classification

See Radial Basis Functions: [[rbf]] as a classic example.''

KNNs (see [[03_Classification]]) can also be considered a subtype of a kernal method, just with a weird discontinuous stepwise kernel.

# See also

* [[Harris2020randomness]] - biological and artificial neural networks as random kernels

# Refs