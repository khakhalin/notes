# Linear Regression
**Linear model**: h(x) = estimation for y = ∑θ_i x_i = xᵀθ, or y ~ h = Xθ in matrix form. Here we assume that xᵀ is a row-vector length p+1, with x_0 = 1 (intercept), followed by n variables. Defines a hyperplane in ℝ^p.

## L2 loss
Loss: J(θ) = ∑(h-y)² across all data points i. Is called **Squared error**, or **squared distance**, or **Mean Squared Error** (MSE), or **Least Squares**. Sensitive to outliers, permissive to small deviations around zero (because of its convex shape). Nice practical illustration from [Google crash course](https://developers.google.com/machine-learning/crash-course/): 2 outliers of 2 are worse than 4 outliers of 1, as 8>4. 

We can minimize this loss by differentiating by θ (partial derivative for each of the coordinates). For one point: ∂J(θ)/∂θ_j = by definition of J: ∂/∂θ_j (h(θ,x)-y)² = simple chain rule: 2(h-y) ∙ ∂h/∂θ_j = by formula for h: 2(h-y)∙x_j . Minimum: dJ/dθ = 0, ⇒ (derivative above written in matrix notation): Xᵀ(Xθ-y) = 0. Here X is a matrix in which each _row_ is an input vector, and y is a column-vector of target outputs. If XᵀX is nonsignular, we can open the brackets, send y to the right, muliply from the left on inverse, get: θ = (XᵀX)⁻¹Xᵀy.

> To sum up **notation choices** of ESLII: one point is a row-vector, so all points make a column (a column of row- vectors). Y values are also a column. As data-point is a row, parameters θ are a column, so that dim(y) = N×1 = dim(Xθ). A bit confusing part when writing it all is that X (matrix) of N×k is multiplied by θ from the right (Xθ), but individual points x have to be written as xᵀθ to show that they are row-vectors (by default x would have been a column-vector). Some notes use a reverse notation for Y = Xθ where θ get transposed, and gets multiplied by x from the left, but let's roll with ESLII notation, as it seems to be everyone's fav book.

Interpretation: if y ~ h = Xθ, then h is in the column-space of X, which forms a subspace of R^N (N data points, p+1 dimensions, assuming x0≡1). To find θ, we project y to col-space using a projection matrix, which obviously minimizes residual (unexplained) variance, by making it orthogonal to col-space.

Another interpretation: 
* for a univariate x, we have a classic projection of vector y (all observations) on x (all input values): y ~ h = xθ = x ∙ xᵀy / xᵀx (by definition of projection). 
* If x is a vector, with several coordinates, but these coordinates are independent (say, in case of orthogonal experiment design design), so columns of X are orthogonal, we can just project to each dimension separately, and the result will be the sum of these projections. That's because once you dot-product y=∑θ_i x_i with x_j, all terms but one would die. Which means that essentially we can pretend that multiple linear regression is just a bunch of univariate regressions. It also matches the general formula, as in this case XᵀX is diagonal. 
* If however columns of X are correlated (not orthogonal), we have to do repetitive elimination ( aka **Gram-Schmidt**) that leads to a full form projection matrix. See below.

Or we can perform a gradient descent: θ := θ + α(h-y)x, with some learning rate α. As this loss is a convex quadratic function, it has only one minimum, and so it always converges. 

### Gram-Smidt
A computationally better alternative to working with inverse matrices. The idea: beak y into x1 and everything else, as if the basis was orthonormal, changing it on the fly. (In practice, x0≡1 goes before x1, but it doesn't change the idea)

Algorithm (without normalization):
1. Start with xi set to x0
2. Replace xi  with its projection to the complement of all previously considered coordinates: qi = xi - ∑proj(xi→qj) = xi - ∑qj∙⟨xi,qj⟩/⟨qj,qj⟩ where j=0 to i-1. Remember all scalar products from this formula as cji = ⟨xi,qj⟩.
3. Find the best projection of y on qi: γi = ⟨y,qi⟩
4. Shift current x from xi to x_i+1. Go to step 2.

This produces a split of X into X = QR where Q with dimensions (N × p+1) is column-orthogonal, and R with dim of (p+1 × p+1) that is upper triangular, with elements cji=⟨xi,qj⟩/⟨qj,qj⟩ (in case or not-normalized basis, 1s on the diagonal). And we get a set of estimates {γ_i} that represent y in the basis Q.

Now as X = QR, our Xθ = y becomes QRθ = y, but we also just found Qγ = y, so Rθ = γ. It gives θ = R⁻¹γ. 

> What I still not quite understand here is why R remains, meaning that we still have to calculate an inverse for it. Could not we somehow manage to tuck it into the calculation as well, to get θ directly? And if not, how do we calculate the inverse in practice? (How to make it computationally effective)? Park for now.

Refs: ESLII p54, [some lecture](http://homepages.ulb.ac.be/~majansen/teaching/STAT-F-408/slides01multipleregression_4.pdf)

In a general case, coordinates of X may be dependent, or close to it. Then, XᵀX from a general formula will be singular, and won't have an inverse, meaning that θ is not uniquely defined. If we use orthogonalization, and two columns of X are highly correlated (say, x1~x2), then x1 will get a normal score, but on the next step we'll have to face a vector that is almost pure noise: q = x2-proj(x1→x2) ~ x2-x2 = 0. Then both qᵀy and qᵀq will be very small, and θ_2 = qᵀy/qᵀq will be very unstable. Estimate for variance: var(θ_2) = σ²/qᵀq. Practical consequences for variable selection (dangers of collinearity), conjuctive dim reduction, ridge regression (why not just zero this θ if it's so bad?) etc.

If Y is multivariate as well, but all errors in Y are independent, we just have several independent linear regressions. If errors in Y are correlated, the very definition of RSS is changed (to encorporate the covariance matrix), but we don't go deep into it here. Refs: ESLII p56

## Other losses
**Are there alternatives to L2?** Sure, **L1 norm** = abs(distance), which effectively pushes f(x) towards median(y) rather than the mean(y): sum of distances to 2 points is min when you're exactly between them. Hard to work with, as derivatives are discontinuous.

**Can regression be used for classification?** Yes, but see [[03_Classification]].

**Max Likelihood**: In some way,can be considered  an alternative to L2 (MSE) loss. For a data sample y_i, the log-prob of observing it is equal to: L(θ) = ∑ log P(y_i | θ), summed by i (all points). We try to find θ that maximizes ∏P, and thus ∑logP. It is connected to L2 though, as least squares maximizes likelihood for conditional probability P(y|x,θ)=𝒩(h,σ²) where h = Xβ. _ESLII has a formula 2.35 for it, but without derivation. I think they may have found a derivative by θ, set it to zero, then integrate the right side by x, but I'm not sure._
 
## Bias-Variance Tradeoff
Total prediction error (L2 loss) consists of two parts: **bias** and **variance**. Derivation:

Say, regression x→y on a dataset (x,y) generated by a distribution P(x,y).
For a given x, we can have P(y|x), and P(x,y) = ∫ P(y|x)P(x) by x.
Regression predicts expected y(x), or Ey (x). 
By definition Ey (x) = ∫y P(y|x) , integrated across all possible x.
ML is about guessing (predicting) E y(x) well using some algorithm.
Say, h(x) is our guess for Ey (x), produced by some method A, based on a training set D.
The expected error for this estimation h is: J = E (h-y)² = ∬ P(x,y)(h(x)-y)² by both x and y.
Now, h itself is a random variable, as it's a function of D, which is a random sample from (x,y).
So we can consider expected value: Eh, by integrating over all possible training sets D.
We can estimate it by training lots of times and averaging. Or, if you are only interested in E(h), you could also just apply A to the full dataset, obtaining the best h possible.
Now, the expected error for the algorithm (across all possible classifiers it could possibly produce) is the error integrated over all possible h (and so, over all Ds), so we have a triple-integral (by D, x, y):
$\int_D \int_x \int_y (h(x)-y)^2 P(D) P(x,y)\,dD\,dx\,dy$
Add and subtract Eh (mean h) inside the square; get (h-Eh) and (Eh-y); open squares.
(h-Eh)² becomes variance.
2(h-Eh)(Eh-y) dies out because for each fixed x and D you get a fixed (Eh-y), and int_D (h-Eh) = 0 by def of Eh.
The term (Eh-y)² will eventually become bias, but it needs some massaging first. Similar to what we did before, subtract and add Ey, then open squares.
We're left with (Eh-Ey)² and (Ey-y)². The term 2(Eh-Ey)(Ey-y) dies off at int_y step. 
(Just make sure to integrate by y at the deepest level, to get 0 before int_x has a chance to see it).
So we ended up having 3 terms:
* Variance of the classifier: (h-Eh)²
* Bias: (Eh-Ey)²
* Variance of the data, aka noise, or irreducable error: (Ey-y)²

Refs: ESLII p24, p37; [lecture by K Weinberger](https://www.youtube.com/watch?v=zUJbRO0Wavo).

> **My understanding** is that it is actually close to the precision / recall situation. The point is that if you try to minimize bias (make your classifier cling to the data), you fall at mercy of your training set, and so increase variance. Each particular classifier, understood as an instance produced by some particular set of training data, will cling to this testing data, so all classifiers will be slightly different, if you train on different data. If you believe that the underlying solution has some constraints to it, you may want to restrain your classifier, even at cost of accepting higher bias (Eh-Ey), so that it would not change that widely for different subsets of your testing data. Regularization does that. Then for each given training set you'll get higher bias, but you'll reduce variance (of your classifier, across all possible training set), as all models will be more similar to each other, and hopefully also closer to the "ground truth". Essentially, a parsimony principle: an assumption that simpler models are better.

> So it's directly related to overfitting. Low bias = high variance = great fit on the training set, but horrible fit on te testing set.

A typical illustration of this: the classic with training an testing losses (ESLII p38), where both go down at first, but then overfitting turns the test curve up.

> But look, it would only work if the ground trugh is actually simple. Why does it work? Why is the ground truth actually simple? Is it some expected statistical property that the world actually follows?.. Or is it because a typical practical problem has certain properties? Seems like that; see below.

### Variance of parameter estimations
How well can we guess θ? Assuming that y_i have normal uncorrelated noise with variance σ², we get var(θ) = (XᵀX)⁻¹σ², where σ² can be estimated from σ² ~ ∑ (y-h)² / (N-p-1). (ESLII p47) In this case, θ are destributed multivariate-Gaussian, and estimations for σ² are chi-squared with N-p-1 degrees of freedom. Which also means that we can use t-test for particular θ_i.

For **categorical variables** (in X), the situation is a bit different, as each of them is represented by several **dummy variables**  (one for each of the levels), and these dummy variables need to be included or excluded all together. F-statistics (with p1-p0, N-p1-1 degrees of freedom).

### Gauss-Markov theorem
**Theorem:** The least squares estimate for β has the smallest variance of all unbiased linear estimators for true parameters β.

In other words, consider that we actually have a linear system plus some independent errors for each data point: y = Xβ + ε. We're saying that the estimation θ obtained by the least squared process ( θ = (XᵀX)⁻¹Xᵀy ) is unbiased, in the sense that E(θ) = E(β), and crucially, Var(θ) is minimal among all possible unbiased linear estimators ( ξ = Cᵀy ) for β. 

It can be proven through direct matrix substitution into the general formula for least squares. The fact that least squares is unbiased is easy: as Xθ is always linearly fit to y, y-Xθ = 0 for any fixed set of y, so E(y-Xθ) = lim(y-Xθ) for N→∞ also = 0 (it never deviates from it). Or one can directly torture E(ξ) after expressing y through true β, and assuming that C=(XᵀX)⁻¹Xᵀ + some non-zero matrix D, as wiki does it. The key part, goes similarly; uses the fact that Var(Cy) = C∙Var(y)∙Cᵀ, and arrives at Var(ξ) = Var(θ) + DDᵀσ² (?).

> Park for now. I don't like it. Supposely it's somehow related to the triangular inequality, which suggests that there should be some reasonable geometric intuition to it, but for now I cannot find it.

In practice, it means that there are estimators with lower variance, but they are necessarily biased. Which leads to a practical program of introducing some bias in order to reduce variance. For example, if we decide to set (aka "shrink") any particular θ_i to 0, we inevitably get a biased estimate.

Refs: ESLII p51, [wiki](https://en.wikipedia.org/wiki/Gauss%E2%80%93Markov_theorem)

### Dimensionality curse
Now let's consider a special case of bias-variance trade-off in a case of very high dimensions. Say, we have a linear model with error: y = Xᵀθ + ε (aka **additive error model**), where ε is normal noise with variance σ². Try to infer θ from a training set of Y. As θ = (XᵀX)⁻¹Xᵀy, we get it with errors of (XᵀX)⁻¹Xᵀε. Which means that whe we are estimating y as h = Xθ, during training we get errors for these estimations: X(XᵀX)⁻¹Xᵀε . 

J for one point xᵀ (x0 = one row in X) is given by a bias-variance formula: J = E( (h-Eh)² + (Eh-y)² ) = xᵀ(XᵀX)⁻¹xσ² + Bias + σ². Here Bias≡0 because linear fit is unbiased, and σ² is irreducable error.

If training is random, (XᵀX)→N∙Cov(X), so we have:
J ~ E( xᵀCov(X)⁻¹xσ²/N ) + σ²= trace(Cov(X)⁻¹Cov(x))σ²/N + σ² = pσ²/N + σ², where p is the number of dimentions. So J grows as a linear function of the number of dimentions p.

> This trace thing here is pretty annoying, but it works. If we E() the left part, we get a E() of sum_ij (x_i x_j C⁻¹ij), which is p^2 terms. But if you open this trace(C∙C⁻¹), you get the same expression (j coming from trace-sum, and j from internal multiplication). So E() indeed gives the trace(), and then as trace() only runs through p terms, and all of these terms happen to be =1 by def of CC⁻¹, we get what we get.

Which means that the higher the number of dimensions, the worse the error (variance). Now, some methods are worse than others: linear regression is actually faring OK, while some (like KNN regressor) get cursed pretty fast (ESLII p26), but the gist is the same for all. If you simulate this, you have diff deterioration curves for different data assumptions. IRL, we try to be in-between these two extremes (linear regression and 1NN) using some info (or assumptions) about the structure of the data. 

## Philosophy of modeling
Returning to bias-variance tradeoff, weconsider x and y as random variables, with a joint distribution P(x,y). We're trying to build a predictor f(x) that approximates y. The squared error loss: L = (y-f)² = ∬ (y-f)² P(x,y) dx dy. We can split P(x,y) into P(x)∙P(y|x), and integrate by y first (inside), then by x outside. Then to minimize total L, we can bind f(x) to matching y point-wise: f(x) = E(y|X=x). Which would essentially give us the **nearest neighbor** method (see the very beginning of "Classification" chapter). An alternative to point-wise matching would be a global smooth model, like linear regression. It makes regression and 1NN two opposite extremes of what can be done with the data. Other stuff (like **additive models** where f = ∑ f_j(x)) are kinda in-between.

In spirit, all models are about setting local and global constraints on the behavior of predictor (such as **behavior in the neighborhood**, that is const for KNN, linear for local linear etc.), and the **granularity** of these  neighborhoods. Some methods (kernels, trees) make this granularity parameter explicit, some don't. And high dimensionality is a problem for all methods, regardless of how they work. 

ESLII defines 3 broad classes of model smoothing / constraining:
* Roughness penalty
* Kernel methods (aka local regression)
* Basis functions and dictionaries

**Roughness penalty**: Use basic RSS (Residual Sum of Squares) with an explicit penalty: L = RSS(f) + λJ(f), where J grows as f() becomes too rough. For example, defining J as λ∫(f'')²dx leads to **cubic smoothing splines** as an optimal solution. Roughness penalty can be interpreted in a Bayesian way, as a log-prior.

**Kernel methods**: explicitly specify local neighborhoods, and a type of function to fit it locally. The neighborhood is defined by the kernel function K(x0,x) that acts as weights for x around x0. Gaussian Kernel: K = 1/λ exp( - |x-x0|² / 2λ). The simplest way to use kernels, is to calculate a weighted average (aka Nadaraya–Watson kernel regression): f(x0) = ∑ K(x0,x_i)∙y_i / ∑ K(x0,x_i).

Or we can set some sort of smooth f(x), and use kernels for RSS calculations: RSS(x0) = ∑ K(x,x0)∙(y-f(x))² , where ∑ runs through all (x_i, y_i). If we assume f(x) = θ0 + θx, we get **local linear regression**. KNNs (see [[03_Classification]]) can also be considered a subtype of a kernal method, just with a weird step-wise kernel.

**Basis functions**: includes linear and polynomial regressions, but the idea is that you have a basis of functions on R^n, and project into it: f = ∑_i θ_i h_i(x). Examples: **polynomial splines**; **radial basis functions** K(μ, x) defined around few centroids μ (not around every data point, as in kernel methods!). Gausssian kernels are still popular. Feed-forward **Deep Learning** also belongs here, with basis functions defined by network design.

## Best-subset regression
Manually find a combination (subset) of k variables that offers best fit for this k. There's an efficient algorithm for that, called **Leaps and Bounds procedure**. Main ways to decide which one to use: empirical **cross-validation**, and AIC = **Akaike Information Criterion**. 

## Ridge regression
Seems to be an old name for **L2 regularization** (see DL chapter). Another name: **Tikhonov regularization**. 

Motivation: Consider Ax = b, and x doesn't exist. In a most typical practical case, it gives a superposition of an overdetermined problem Ax' = b for the component of x that is in the row-space, and so is affected by the matrix (x' = projection of x into row-space), and an underdetermined problem A(x-x') = 0 for the components x-x' that are in the null-space of A, and so can be chosen arbitrary without any effect on Ax. Everything that in the null-space cannot bring Ax closer to b, so we are free to pick them in some "nice" fashion, for example, to satisfy some priors, or minimize complexity.

With Tikhonov regularization, we minimize not squared Euqledian norm |Ax-b|^2 but |Ax-b|^2 + |Γx|^2 where Γ is some matrix; often identity matrix I multiplied by a coefficient, in which case we just have a sum of squared x_i (L2 regularization) multiplied by a coeff (usually denoted k). 

This is especially important in case of **Multicollinearity**, when you're trying to predict y from many variables a_i, in a way Ax = y (observations of variables a_i for different training points become columns of A, while regression coefficients form x), but some of a_i are strongly correlated. In this case trying to painfully minimize y-Ax would be counter-productive, as we'd fit noise in y with noise in a_i. Imagine an extreme case: all columns of A are the same (rank=1), but are observed with noise, and noise is independent (so formally rank = N). What we actually need is only one (doesn't matter which one) x_i>0, and all others 0. But what will happen, is that we'll fit noise in y with noise in A.

The name "ridge" comes from a visual example of what is describe above. Imagine that only part of the solution is well defined, and the rest is close ot null-space of A. Then the "true solution" is a "generalized cylinder" made by the true solution (in those coordinates that make sense), arbitrarily extended across the "irrelevant coordinates". Small changes in training data (right side of the Ax=y equation) would sway the solution along this "ridge". By adding regularization we change a "ridge" into a "peak" (lines turn into parabolas), which stabilizes the solution to perturbations in both A and y. ([Ref: stackexchange](https://stats.stackexchange.com/questions/118712/why-does-ridge-estimate-become-better-than-ols-by-adding-a-constant-to-the-diago/119708#119708))

How to pick the coefficient k? One method: **Ridge trace**: plot found coefficients as a function of k, and eyeball value at which they stop oscillating, and switches to converging (not in the sense of becoming const, but in the sense of of monotone almost-linear change).

## Stepwise Regression
A compromise between performance and speed: automated stepwise selection of features based on the quality of fit, and a complexity penalty (regularization). Two main approaches: **Forward selection** and **Backward selection**. Or a combinationof both (*not sure how?*)

The maxim about stepwise regression is that it is useless for hypothesis testing, questionable for prediction, and OK for variable selection (see [[07_Features]]). 
* It can't be used for hypotheses, as p-values after this procedure are meaningless, and arguably shouldn't even be reported, as their direct probabilistic intepretation is impossible, and adjustment is too complicated (p-values aren't independent, so you cannot do FDR), making adjustment impossible in practice.
* It can be used for prediction, but usually full models (or, assumably, probably regularized models) are more powerful anyways.
* It's an OK method to select variables tho.

Some people (like Thompson cited below) are really vehemently against them tho. That's because they are usually too optimistic (the very concept of degrees of freedom becomes problematic after multiple testing), yet are strictly less powerful than proper regularization, and also typically don't replicate, as the sequence of descent depends on the data, so performing stepwise regression on different subsets of data is much more likely to yield different results, compared to a more sophisticated model.

Refs:
* [[Guyon2003variable]], [wiki](https://en.wikipedia.org/wiki/Stepwise_regression), ESLII p59
* Steyerberg, E. W., Eijkemans, M. J., Harrell Jr, F. E., & Habbema, J. D. F. (2001). Prognostic modeling with logistic regression analysis: in search of a sensible strategy in small data sets. Medical Decision Making, 21(1), 45-56.
* Thompson, B. (2001). Significance, effect sizes, stepwise methods, and other issues: Strong arguments move the field. The Journal of Experimental Education, 70(1), 80-93. 

There's also a thing called **Forward-Stagewise Regression** that is based on correlations between remaining features and the current residual, and it is just plan horrible.

## Logistic Regression

**Logistic function**: $\sigma(x) = \frac{1}{1+e^{-x}} = \frac{e^x}{1+e^x}$.

The inverse function is called **logit**: logit(p) $=\ln \frac{p}{1-p}$

Assumes that **log-odds** $\log \frac{p}{1-p}=a+bx$ , so is a linear function of x (that is generally a vector). It means that p/(1-p)=exp(ax+bx), which leads to p = σ(ax+b). It can be said that a logistic regression is just a linear regression of log-odds.

To find a solution, minimize **logloss**: Loss = $-\sum (y\cdot\log(\tilde y)+(1-y)\cdot\log(1-\tilde y))$, or, using p for y estimations, -∑( y∙log(p) + (1-y)∙log(1-p) ). As p→0, -log(p)→inf, so huge punishment for near-zero p. Because of that, if the data is too clean and some areas contain only points of one type, weights may explode (it's hard to fit infinity), making proper regularization extremely important.

For high-D non-linear data, works great if features cross-products are included into the set as synthetic features (see [[07_Features]]).