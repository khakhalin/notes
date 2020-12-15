# Regression

#regression

### Subtopics

* [[linear_regression]] - the core concept :)
    * [[gram-schmidt]] - close form elimination
    * [[optimizers]] - related
* [[bias-variance]] - Bias-Variance Trade-off

**Constrained models**
    * Reliable approaches:
        * [[ridge_regression]] - class. Aka **L2 regularization**, aka **shrinkage**, aka Tikhonov
        * [[lasso]] - aka **L1 regularization**. Also covers **Elastic Net** (a combo of L1 and L2)
    * Trickier approaches:
        * [[aic]] - Akaike Information Criterion and Best-Subset Regression
        * [[stepwise_regression]] - a bit dangerious, but fast. Also covers least angle regression
        * [[pca_regression]] - pca regression and partial least squares
    * See also:
        * [[ransac]] - bootstrapping-like regression estimator based on inliners voting

# Notation

To sum up **notation choices** (roughly matching ESL): one data point is a row-vector of coordinates, so all points in a dataset make up a tall slim matrix (a column of row- vectors). The values of y are also a column. Next, as one data-point is a row, parameters θ have to be a column, so that dim(y) = N×1 = dim(Xθ). When writing it all, a part that is a bit confusing is that X (matrix) of N×k is multiplied by θ from the right (Xθ), but individual points x may have to be written as xᵀθ to show that they are row-vectors (by default x would have been a column-vector).

Also, as Unicode makes diacritics a hassle, I try to use θ for parameter estimates, and β for true values. x_j is typically a column (one coordinate) of X, to match a corresponding θ_j. Subscripts are shortened (xi instead of x_i) where it looks ok. Cdot (∙) may mean both normal "single-number" product and inner product, while ⟨x,y⟩ always means inner product. 

# Philosophy of prediction

Let's consider x and y as random variables, with a joint distribution P(x,y). Now try to build a predictor f(x) that approximates y. The squared error loss is: L = (y-f)² = ∬ (y-f)² P(x,y) dx dy. We can split P(x,y) into P(x)∙P(y|x), and integrate first by y  (inner integral), then by x (outer integral). Then to minimize total L, we can bind f(x) to match y point-wise: f(x) = E(y|X=x), as it would minimize inner integral. Which would essentially give us the **nearest neighbor** method (see the very beginning of [[03_Classification]]). An alternative to point-wise matching would be a global smooth model, like linear regression. It makes regression and 1NN two opposite extremes of what can be done with the data. Other stuff (like **additive models** where f = ∑ f_j(x)) are kinda in-between.

In spirit, all models are about setting local and global constraints on the behavior of the predictor (such as **behavior in the neighborhood**, which is "const" for KNN, "linear" for local linear embedding etc.), and the **granularity** of these  neighborhoods. Some methods (kernels, trees) make this granularity parameter explicit, some don't. And high dimensionality is a problem for all methods, regardless of how they work. (See [[curse_dim]])

There are **3 broad approaches to model smoothing**:
* Roughness penalty
* Explicitly local methods (kernel methods, aka local regression)
* Constrained basis functions and dictionaries

### Roughness penalty

Use basic RSS (Residual Sum of Squares) with an explicit penalty: L = RSS(f) + λJ(f), where J grows as f() becomes too rough. For example, defining J as λ∫(f'')²dx leads to **cubic smoothing splines** as an optimal solution. Roughness penalty can be interpreted in a Bayesian way, as a log-prior.

### Kernel methods

Explicitly specify local neighborhoods, and a type of a function to fit it locally. The neighborhood is defined by the kernel function K(x0,x) that acts as weights for x around x0. For example, Gaussian Kernel: K = 1/λ exp( - |x-x0|² / 2λ). [[normal]]]

The simplest way to use kernels K is to calculate a weighted average of y using these kernels (aka **Nadaraya–Watson kernel regression**): $f(x_0) = ∑ (y_i K(x_0,x_i)) / ∑ K(x_0,x_i)$. #kernel

Or we can choose some smooth f(x), and then optimize it, while using kernels in loss ([[l2]]) calculations: RSS(x) = $\sum_i K(x, x_i)\cdot(y_i-f(x_i))^2$ , where ∑ runs through all data points (x_i, y_i). If we assume $f(x_i) = θ_0 + θ(x_i-x)$, with it's own θ for every x, we get **local linear regression** (aka [[loess]]). KNNs (see [[03_Classification]]) can also be considered a subtype of a kernal method, just with a weird discontinuous stepwise kernel.

### Basis functions

This type includes linear and polynomial regressions, but the idea is that you pick a basis of functions in R^n, and project into it: $f = \sum_i θ_i h_i(x)$. Examples: **polynomial splines**; **radial basis functions** [[RBF]] with K(μ, x) defined around several centroids μ that can itself be optimized. Gaussian kernels are still popular. Feed-forward **Deep Learning** actually also belongs here, it just that its basis functions are defined by the network design (the space of functions that can be achieved with this particular network depth, activation functions etc).

## Variance of parameter estimations

How well can we guess θ? Assuming that y_i have normal uncorrelated noise with variance σ², we get var(θ) = (XᵀX)⁻¹σ², where σ² can be estimated from σ² ~ ∑ (y-h)² / (N-p-1). (ESL p47). In this case, θ are destributed multivariate-Gaussian, and estimations for σ² are chi-squared with N-p-1 degrees of freedom. Which also means that we can use t-test for particular θ_i.

For **categorical variables** (in X), the situation is a bit different, as each of them is represented by several **dummy variables**  (one for each of the levels), and these dummy variables need to be included or excluded all together. F-statistics (with p1-p0, N-p1-1 degrees of freedom). This is also known as **one-hot encoding**.

## Gauss-Markov theorem

**Theorem:** The least squares estimate for β has the smallest variance of all unbiased linear estimators for true parameters β.

In other words, consider that we actually have a linear system plus some independent errors for each data point: y = Xβ + ε. We're saying that the estimation θ obtained by the least squared process ( θ = (XᵀX)⁻¹Xᵀy ) is unbiased, in the sense that E(θ) = E(β), and crucially, Var(θ) is minimal among all possible unbiased linear estimators ( ξ = Cᵀy ) for β. 

It can be proven through direct matrix substitution into the general formula for least squares. The fact that least squares is unbiased is easy: as Xθ is always linearly fit to y, ∑(y-Xθ) = 0 for any fixed set of y, so E(y-Xθ) = lim(y-Xθ)(with N→∞) = 0 (it never deviates from it). Or one can directly torture E(ξ) after expressing y through true β, and assuming that C=(XᵀX)⁻¹Xᵀ + some non-zero matrix D, as wiki does it. The key part goes similarly: it uses the fact that Var(Cy) = C∙Var(y)∙Cᵀ, and arrives at Var(ξ) = Var(θ) + DDᵀσ² ...

> Park for now. I don't like it. Supposely it's somehow related to the triangule inequality, which suggests that there should be some reasonable geometric intuition to it, but for now I cannot find it. It feels like it should be elementary explainable, or at least intuitive, soo look in other books. #todo

In practice, it means that there are estimators with lower variance, but they are necessarily biased. Which leads to a practical, actionable program: we can introduce some deliberate bias in order to reduce variance. For example, if we decide to set (aka "shrink") any particular θ_i to 0, we inevitably get a biased estimate, but it can help with variance.

Refs: ESL p51, [wiki](https://en.wikipedia.org/wiki/Gauss%E2%80%93Markov_theorem)