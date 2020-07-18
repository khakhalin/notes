# Linear Regression

**Linear model**: h(x) = (estimation for y) = $∑θ_j x_j$ = xᵀθ, or in matrix form: h = Xθ ~ y. We assume that xᵀ is a row-vector length p+1, with x_0 = 1 (intercept), followed by n "normal" variables on which we regress. The equation above defines a hyperplane in ℝ^p.

# Subtopics

**Constrained models**

Reliable approaches:
* [[ridge_regression]] - class. Aka **L2**, aka **shrinkage**, aka Tikhonov
* [[lasso]] - aka **L1** regression. Also **Elastic Net** (a combo of L1 and L2)

Trickier approaches:
* [[aic]] - Akaike Information Criterion and Best-Subset Regression
* [[stepwise_regression]] - a bit dangerious, but fast? Also least angle regression
* [[pca_regression]] - pca regression and partial least squares

See also:
* [[ransac]] - bootstrapping-like regression estimator based on inliners voting

# L2 loss

Loss: J(θ) = ∑(h-y)² across all data points i. Is called **Squared error**, or **squared distance**, or **Mean Squared Error** (MSE), or **Least Squares**. Sensitive to outliers, permissive to small deviations around zero (because of its convex shape). Nice practical illustration from [Google crash course](https://developers.google.com/machine-learning/crash-course/): 2 outliers of 2 are worse than 4 outliers of 1, as 8>4. 

We can minimize this loss by differentiating by θ (partial derivative for each of the coordinates). For one point: 
∂J(θ)/∂θ_j = ∂/∂θ_j (h(θ,x)-y)² (by definition of J)
= 2(h-y) ∙ ∂h/∂θ_j (simple chain rule)
= 2(h-y)∙x_j (by formula-definition of h).
Minimum: dJ/dθ = 0, ⇒ Xᵀ(Xθ-y) = 0 (if we write the derivative above in matrix notation). Here X is a matrix in which each _row_ is an input vector, and y is a column-vector of target outputs. If XᵀX is nonsignular, we can open the brackets, send y to the right, muliply from the left on inverse, get: θ = (XᵀX)⁻¹Xᵀy.

> To sum up **notation choices** (roughly matching ESL): one data point is a row-vector of coordinates, so all points in a dataset make up a tall slim matrix (a column of row- vectors). The values of y are also a column. Next, as one data-point is a row, parameters θ have to be a column, so that dim(y) = N×1 = dim(Xθ). When writing it all, a part that is a bit confusing is that X (matrix) of N×k is multiplied by θ from the right (Xθ), but individual points x may have to be written as xᵀθ to show that they are row-vectors (by default x would have been a column-vector).

> Also, as Unicode makes diacritics a hassle, I try to use θ for parameter estimates, and β for true values. x_j is typically a column (one coordinate) of X, to match a corresponding θ_j. Subscripts are shortened (xi instead of x_i) where it looks ok. Cdot (∙) may mean both normal "single-number" product and inner product; ⟨x,y⟩ always means inner product. 

Interpretation: if y ~ h = Xθ, then h is in the column-space of X, which forms a subspace of R^N (N data points, p+1 dimensions, assuming x0≡1). To find θ, we project y to col-space using a projection matrix, which obviously minimizes residual (unexplained) variance, by making it orthogonal to col-space.

# Philosophy of prediction
Returning to bias-variance tradeoff, we consider x and y as random variables, with a joint distribution P(x,y). Now try to build a predictor f(x) that approximates y. The squared error loss: L = (y-f)² = ∬ (y-f)² P(x,y) dx dy. We can split P(x,y) into P(x)∙P(y|x), and integrate by y first (inside), then by x outside. Then to minimize total L, we can bind f(x) to matching y point-wise: f(x) = E(y|X=x), as it would minimize inner integral. Which would essentially give us the **nearest neighbor** method (see the very beginning of [[03_Classification]]). An alternative to point-wise matching would be a global smooth model, like linear regression. It makes regression and 1NN two opposite extremes of what can be done with the data. Other stuff (like **additive models** where f = ∑ f_j(x)) are kinda in-between.

In spirit, all models are about setting local and global constraints on the behavior of the predictor (such as **behavior in the neighborhood**, which is const for KNN, linear for local linear etc.), and the **granularity** of these  neighborhoods. Some methods (kernels, trees) make this granularity parameter explicit, some don't. And high dimensionality is a problem for all methods, regardless of how they work. 

There are **3 broad approaches to model smoothing**:
* Roughness penalty
* Local methods (kernel methods, aka local regression)
* Constrained basis functions and dictionaries

**Roughness penalty**: Use basic RSS (Residual Sum of Squares) with an explicit penalty: L = RSS(f) + λJ(f), where J grows as f() becomes too rough. For example, defining J as λ∫(f'')²dx leads to **cubic smoothing splines** as an optimal solution. Roughness penalty can be interpreted in a Bayesian way, as a log-prior.

**Kernel methods**: explicitly specify local neighborhoods, and a type of a function to fit it locally. The neighborhood is defined by the kernel function K(x0,x) that acts as weights for x around x0. Gaussian Kernel: K = 1/λ exp( - |x-x0|² / 2λ). The simplest way to use kernels K is to calculate a weighted average of y using these kernels (aka Nadaraya–Watson kernel regression): f(x0) = ∑ K(x0,x_i)∙y_i / ∑ K(x0,x_i). #kernel

Or we can choose some smooth f(x), and then optimize it using kernels in RSS calculations: RSS(x0) = ∑ K(x,x0)∙(y-f(x))² , where ∑ runs through all (x_i, y_i). If we assume f(x) = θ0 + θx, we get **local linear regression**. KNNs (see [[03_Classification]]) can also be considered a subtype of a kernal method, just with a weird discontinuous stepwise kernel.

**Basis functions**: this type includes linear and polynomial regressions, but the idea is that you pick a basis of functions in R^n, and project into it: f = ∑_i θi hi(x). Examples: **polynomial splines**; **radial basis functions** [[RBF]] with K(μ, x) defined around several centroids μ that can itself be optimized. Gaussian kernels are still popular. Feed-forward **Deep Learning** actually also belongs here, it just that its basis functions are defined by the network design (the space of functions that can be achieved with this particular network depth, activation functions etc).

**Are there alternatives to L2?** Sure, **L1 norm** = abs(distance), which effectively pushes f(x) towards median(y) rather than the mean(y): sum of distances to 2 points is min when you're exactly between them. Hard to work with, as derivatives are discontinuous.

**Max Likelihood** (aka MLE): In some way, can be considered an alternative to L2 (MSE) loss. For a data sample y_i, the log-prob of observing it is equal to: L(θ) = ∑ log P(y_i | θ), summed by i (all points). We can try to find θ that maximizes ∏P, and thus ∑logP. But it is only different from L2 if errors are assumed to be non-normal; for normally distributed errors P(y|x,θ)=𝒩(h,σ²) where h = Xβ if you go through MLE calculations, you'll arrive at same equlations as for L2 loss.

> ESL has a formula 2.35 for it, but without derivation. I think they may have found a derivative by θ, set it to zero, then integrate the right side by x, but I'm not sure.

# The linear algebra behind

Intepretations for h = Xθ:
* For a univariate x, we have a classic projection of a vector y (a column of observations) onto x (a column of input values): y ~ h = xθ = x ∙ xᵀy / xᵀx (by definition of projection). 
* If each x_i is a vector, with several coordinates, but these coordinates come from an orthogonal experiment design, we can just project to each dimension separately, and the result will be the sum of these projections. That's because once you dot-product y=∑θ_i x_i with x_j, all terms but one would die. Which means that essentially we can pretend that multiple linear regression is just a bunch of univariate regressions. It also matches the general formula, as in this case the matrix XᵀX is diagonal. 
* If however columns of X are correlated (not orthogonal), we have to do repetitive elimination ( aka **Gram-Schmidt**) that leads to a full form projection matrix. See below.

Or we can perform a **gradient descent**: θ ← θ + α(h-y)x, with some learning rate α. As our L2 loss is a convex quadratic function, it has only one minimum, and so it always converges. 

# Gram-Smidt

> #todo: this description doesn't make sense currently. It's supposed to summarize ESL p54, but I cannot understand it upon re-reading it. Rewrite!

GS is a computationally solid approach to calculating θ, that works better than inverse matrices. The idea: break y into x and everything else, as if the basis was orthonormal, then update si iteratively.

Algorithm (without normalization):
1. Set xi (the current best guess for x) to x0
2. Replace xi with its projection to the complement of all previously considered coordinates: qi = xi - ∑proj(xi→qj) = xi - ∑qj∙⟨xi,qj⟩/⟨qj,qj⟩ where j=0 to i-1. Remember all scalar products from this formula as cji = ⟨xi,qj⟩.
3. Find the best projection of y onto qi: γi = ⟨y,qi⟩
4. Shift current x from xi to x_i+1. Go to step 2.

This produces a split of X into X = QR where Q with dimensions (N × p+1) is column-orthogonal, and R with dim of (p+1 × p+1) that is upper triangular, with elements cji=⟨xi,qj⟩/⟨qj,qj⟩ (in case or not-normalized basis, 1s on the diagonal). And we get a set of estimates {γ_i} that represent y in the basis Q.

Now as X = QR, our Xθ = y becomes QRθ = y, but we also just found y = Qγ, so Rθ = γ. It gives θ = R⁻¹γ. 

> What I still not quite understand here is why R remains, meaning that we still have to calculate an inverse for it. Could not we somehow manage to tuck it into the calculation as well, to get θ directly? And if not, how do we calculate the inverse in practice? (How to make it computationally effective)? Park for now.

Refs: ESL p54, [some lecture](http://homepages.ulb.ac.be/~majansen/teaching/STAT-F-408/slides01multipleregression_4.pdf)

In a general case, coordinates of X may be dependent, or be very close to it. In this case, XᵀX from the general formula is singular, and doesn't have an inverse, meaning that θ is not uniquely defined. If we use orthogonalization, and two columns of X are highly correlated (say, x1~x2), then x1 will get a normal score, but on the next step we'll have to face a vector that is pure noise: q = x2-proj(x1→x2) ~ x2-x2 = 0. Then both qᵀy and qᵀq will be very small, and θ_2 = qᵀy/qᵀq will be unstable. Estimate for variance: var(θ_2) = σ²/qᵀq. Practical consequences for variable selection (dangers of collinearity): **conjunctive dim reduction**, **ridge regression** (why not just zero this θ if it's so bad?) etc.

If Y is multivariate as well, but all errors in Y are independent, we just have several independent linear regressions. If errors in Y are correlated, the very definition of RSS is changed (to encorporate the covariance matrix), but we don't go deep into it here. Refs: ESL p56

# Bias-Variance Tradeoff

#variance

Total prediction error (L2 loss) consists of two parts: **bias** and **variance**. 

Derivation: Say, we regress y by x for a dataset (x,y), generated by a distribution P(x,y). For a given x, we can find P(y|x), and then $P(x,y) = ∫ P(y|x)P(x) dx$. Regression predicts expected y(x), or Ey(x). By definition Ey(x) = ∫y P(y|x) , integrated across all possible x.
The main task of ML is to guess (predict) Ey(x) well. Say, h(x) is our guess for Ey (x), produced by some method A, based on a training set D. The expected error for this estimation h is: J = E (h-y)² = ∬ P(x,y)(h(x)-y)² dx dy. 

Now, h itself is a random variable, as it's a function of random sample D that was sampled from (x,y). So we can consider expected value: Eh, by integrating over all possible training sets D. We can estimate it by training lots of times and averaging. Or, if you are only interested in E(h), you could also just apply A to the full dataset, obtaining the best h possible.

Now, the expected error for the algorithm (across all possible classifiers it could possibly produce) is the error integrated over all possible h (and so, over all Ds), so we have a triple-integral (by D, x, y): $\int_D \int_x \int_y (h(x)-y)^2 P(D) P(x,y)\,dD\,dx\,dy$

Add and subtract Eh (mean h) inside the square; get (h-Eh) and (Eh-y); open squares.
(h-Eh)² becomes variance. 2(h-Eh)(Eh-y) dies out because for each fixed x and D you get a fixed (Eh-y), and int_D (h-Eh) = 0 by def of Eh. 

The term (Eh-y)² will eventually become bias, but it needs some massaging first. Similar to what we did before, subtract and add Ey, then open squares. We're left with (Eh-Ey)² and (Ey-y)². The term 2(Eh-Ey)(Ey-y) dies off at int_y step. (Just make sure to integrate by y at the deepest level, to get 0 before int_x has a chance to see it).

So we ended up with 3 terms:
* Variance (of the classifier): (h-Eh)²
* Bias: (Eh-Ey)²
* Irreducable error, aka noise, aka variance of the data: (Ey-y)²

It seems to be somewhat similar to the **precision / recall** situation. If you try to minimize bias (make your classifier cling to the data), you fall at mercy of your training set, and so increase variance. Each particular classifier, understood as an instance produced by some particular set of training data, will cling to this testing data, so all classifiers will be slightly different, if you train on different data. If however you believe that the underlying solution has some constraints to it, you can restrain your classifier, even at a cost of accepting higher bias (Eh-Ey), to ensure that it does not change too widely for different subsets of your testing data. **Regularization** and the use of simplified, constrained models, achieve that. In this case, for each given training set you'll get higher bias, but lower variance (of your classifier, across all possible training sets), as all models will be more similar to each other (and hopefully also closer to the "ground truth"). A parsimony principle: this logic is guided by an assumption that simpler, more constrained models are better.

Whether this effect will be manifest IRL depends on whether it is exacerbated by overfitting (problems of interpolation and extrapolation). When a constrained model tries to cling to the data, it may also diverge in parts of the landscape that have no or few observed training values, driving variance up. (Imagine a textbook picture of a polynomial overfitting here). In this case, low bias ⇒ high variance ⇒ great fit on the training set ⇒  horrible fit on parts of the testing set. This is because variance can be further split into **variance due to sampling** and **variance due to optimization**, and it is the strength of the 2nd component of variance that matters.

There are two typical graphical illustrations for it. One shows how training and validation error rates change as you increase the complexity of the model. Both training and testing losses first go down, but at some point overfitting kicks in and turns the test curve up. Another canonical illustration looks like a parabola (total error) lying on two meeting hyperbolas (left horn for bias, right horn for variance): as you change model complexity, first error goes down because the bias goes down, but then increases again as variance starts to rapidly increase.

> But look, it would only work if the ground trugh is actually simple. Why does it actually work? Why would the ground truth be simple, in practice? Is it some expected statistical property that the world actually follows, similar to how the central limit theorem shows that the world follows certain statistical patterns? Or is it because a typical practical problem has certain properties, by virtue of being practical (some sort of selection bias)? What's the philosophy behind that?

Refs: ESL p24, p37; [lecture by K Weinberger](https://www.youtube.com/watch?v=zUJbRO0Wavo); [[Neal2019blog]].

**Prediction bias**: a very simple measure of model validity: average value of all predictions - average value of all learning points. In a reasonable model, prediction bias should be close to 0. A way to assess it: build a **calibration plot** - bucket values, calculate predictions, then plot mean(predictions) against mean(values). May help to find areas where the model misbehaves.

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

See also:
* [[curse_dim]] - Curse Dimensionality, which can be considered a special (and very unpleasant) case of Bias-Variance trade-off