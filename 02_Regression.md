# Linear Regression
**Linear model**: h(x) = estimation for y = $∑θ_i x_i$ = xᵀθ, or y ~ h = Xθ in matrix form. Here we assume that xᵀ is a row-vector length p+1, with x_0 = 1 (intercept), followed by n variables. The equation above defines a hyperplane in ℝ^p.

## L2 loss
Loss: J(θ) = ∑(h-y)² across all data points i. Is called **Squared error**, or **squared distance**, or **Mean Squared Error** (MSE), or **Least Squares**. Sensitive to outliers, permissive to small deviations around zero (because of its convex shape). Nice practical illustration from [Google crash course](https://developers.google.com/machine-learning/crash-course/): 2 outliers of 2 are worse than 4 outliers of 1, as 8>4. 

We can minimize this loss by differentiating by θ (partial derivative for each of the coordinates). For one point: ∂J(θ)/∂θ_j = by definition of J: ∂/∂θ_j (h(θ,x)-y)² = simple chain rule: 2(h-y) ∙ ∂h/∂θ_j = by formula for h: 2(h-y)∙x_j . Minimum: dJ/dθ = 0, ⇒ (derivative above written in matrix notation): Xᵀ(Xθ-y) = 0. Here X is a matrix in which each _row_ is an input vector, and y is a column-vector of target outputs. If XᵀX is nonsignular, we can open the brackets, send y to the right, muliply from the left on inverse, get: θ = (XᵀX)⁻¹Xᵀy.

> To sum up **notation choices** (roughly matching ESL): one point is a row-vector, so all points make a column (a column of row- vectors). The values of y are also a column. As data-point is a row, parameters θ are a column, so that dim(y) = N×1 = dim(Xθ). A bit confusing part when writing it all is that X (matrix) of N×k is multiplied by θ from the right (Xθ), but individual points x have to be written as xᵀθ to show that they are row-vectors (by default x would have been a column-vector). As Unicode makes diacritics a hassle, I try to use θ for parameter estimates, and β for true values. x_i is typically a column of X. Subscripts are shortened (xi instead of x_i) if it looks ok. Cdot (∙) may mean both normal and inner product; ⟨x,y⟩ always means inner product. 

Interpretation: if y ~ h = Xθ, then h is in the column-space of X, which forms a subspace of R^N (N data points, p+1 dimensions, assuming x0≡1). To find θ, we project y to col-space using a projection matrix, which obviously minimizes residual (unexplained) variance, by making it orthogonal to col-space.

Another interpretation: 
* for a univariate x, we have a classic projection of vector y (all observations) on x (all input values): y ~ h = xθ = x ∙ xᵀy / xᵀx (by definition of projection). 
* If x is a vector, with several coordinates, but these coordinates are independent (say, in case of orthogonal experiment design design), so columns of X are orthogonal, we can just project to each dimension separately, and the result will be the sum of these projections. That's because once you dot-product y=∑θ_i x_i with x_j, all terms but one would die. Which means that essentially we can pretend that multiple linear regression is just a bunch of univariate regressions. It also matches the general formula, as in this case XᵀX is diagonal. 
* If however columns of X are correlated (not orthogonal), we have to do repetitive elimination ( aka **Gram-Schmidt**) that leads to a full form projection matrix. See below.

Or we can perform a gradient descent: θ := θ + α(h-y)x, with some learning rate α. As this loss is a convex quadratic function, it has only one minimum, and so it always converges. 

## Gram-Smidt
A computationally better alternative to working with inverse matrices. The idea: beak y into x1 and everything else, as if the basis was orthonormal, changing it on the fly. (In practice, x0≡1 goes before x1, but it doesn't change the idea)

Algorithm (without normalization):
1. Start with xi set to x0
2. Replace xi  with its projection to the complement of all previously considered coordinates: qi = xi - ∑proj(xi→qj) = xi - ∑qj∙⟨xi,qj⟩/⟨qj,qj⟩ where j=0 to i-1. Remember all scalar products from this formula as cji = ⟨xi,qj⟩.
3. Find the best projection of y on qi: γi = ⟨y,qi⟩
4. Shift current x from xi to x_i+1. Go to step 2.

This produces a split of X into X = QR where Q with dimensions (N × p+1) is column-orthogonal, and R with dim of (p+1 × p+1) that is upper triangular, with elements cji=⟨xi,qj⟩/⟨qj,qj⟩ (in case or not-normalized basis, 1s on the diagonal). And we get a set of estimates {γ_i} that represent y in the basis Q.

Now as X = QR, our Xθ = y becomes QRθ = y, but we also just found Qγ = y, so Rθ = γ. It gives θ = R⁻¹γ. 

> What I still not quite understand here is why R remains, meaning that we still have to calculate an inverse for it. Could not we somehow manage to tuck it into the calculation as well, to get θ directly? And if not, how do we calculate the inverse in practice? (How to make it computationally effective)? Park for now.

Refs: ESL p54, [some lecture](http://homepages.ulb.ac.be/~majansen/teaching/STAT-F-408/slides01multipleregression_4.pdf)

In a general case, coordinates of X may be dependent, or close to it. Then, XᵀX from a general formula will be singular, and won't have an inverse, meaning that θ is not uniquely defined. If we use orthogonalization, and two columns of X are highly correlated (say, x1~x2), then x1 will get a normal score, but on the next step we'll have to face a vector that is almost pure noise: q = x2-proj(x1→x2) ~ x2-x2 = 0. Then both qᵀy and qᵀq will be very small, and θ_2 = qᵀy/qᵀq will be very unstable. Estimate for variance: var(θ_2) = σ²/qᵀq. Practical consequences for variable selection (dangers of collinearity), conjuctive dim reduction, ridge regression (why not just zero this θ if it's so bad?) etc.

If Y is multivariate as well, but all errors in Y are independent, we just have several independent linear regressions. If errors in Y are correlated, the very definition of RSS is changed (to encorporate the covariance matrix), but we don't go deep into it here. Refs: ESL p56

## Bias-Variance Tradeoff
#variance
Total prediction error (L2 loss) consists of two parts: **bias** and **variance**. 

Derivation: Say, we regress y by x on a dataset (x,y) generated by a distribution P(x,y).
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
So we ended up with 3 terms:
* Variance (of the classifier): (h-Eh)²
* Bias: (Eh-Ey)²
* Irreducable error, aka noise, aka variance of the data: (Ey-y)²

It seems to be somewhat similar to precision / recall situation. If you try to minimize bias (make your classifier cling to the data), you fall at mercy of your training set, and so increase variance. Each particular classifier, understood as an instance produced by some particular set of training data, will cling to this testing data, so all classifiers will be slightly different, if you train on different data. If however you believe that the underlying solution has some constraints to it, you can restrain your classifier, even at a cost of accepting higher bias (Eh-Ey), so that it would not change that widely for different subsets of your testing data. Regularization and the use of simplified, constrained models, do that. Then for each given training set you'll get higher bias, but lower variance (of your classifier, across all possible training sets), as all models will be more similar to each other (and hopefully also closer to the "ground truth"). A parsimony principle: an assumption that simpler models are better.

Whether this effect will be manifest IRL depends on whether it is exacerbated by overfitting. When a constrained model tries to cling to training data, it may also diverge in parts of the landscape that have no or few observed training values, driving variance up. (Imagine a textbook picture of a polynomial overfitting here). In this case, low bias ⇒ high variance ⇒ great fit on the training set ⇒  horrible fit on parts of the testing set. This is because variance can be further split into **variance due to sampling** and **variance due to optimization** [[Neal2019blog]], and it is the strength of the 2nd component of variance that matters.

There are two typical graphical illustrations for it. One shows how training and validation error rates change as you increase the complexity of the model. Both training and testing losses first go down, but at some point overfitting kicks in and turns the test curve up. Another canonical illustration looks like a parabola (total error) lying on two meeting hyperbolas (left horn for bias, right horn for variance): as you change model complexity, first error goes down because the bias goes down, but then increases again as variance starts to rapidly increase.

> But look, it would only work if the ground trugh is actually simple. Why does it actually work? Why would the ground truth be simple, in practice? Is it some expected statistical property that the world actually follows, similar to how the central limit theorem shows that the world follows certain statistical patterns? Or is it because a typical practical problem has certain properties, by virtue of being practical (some sort of selection bias)? What's the philosophy behind that?

Refs: ESL p24, p37; [lecture by K Weinberger](https://www.youtube.com/watch?v=zUJbRO0Wavo); [[Neal2019blog]].

### Variance of parameter estimations
How well can we guess θ? Assuming that y_i have normal uncorrelated noise with variance σ², we get var(θ) = (XᵀX)⁻¹σ², where σ² can be estimated from σ² ~ ∑ (y-h)² / (N-p-1). (ESL p47) In this case, θ are destributed multivariate-Gaussian, and estimations for σ² are chi-squared with N-p-1 degrees of freedom. Which also means that we can use t-test for particular θ_i.

For **categorical variables** (in X), the situation is a bit different, as each of them is represented by several **dummy variables**  (one for each of the levels), and these dummy variables need to be included or excluded all together. F-statistics (with p1-p0, N-p1-1 degrees of freedom). This is also known as **one-hot encoding**.

### Gauss-Markov theorem
**Theorem:** The least squares estimate for β has the smallest variance of all unbiased linear estimators for true parameters β.

In other words, consider that we actually have a linear system plus some independent errors for each data point: y = Xβ + ε. We're saying that the estimation θ obtained by the least squared process ( θ = (XᵀX)⁻¹Xᵀy ) is unbiased, in the sense that E(θ) = E(β), and crucially, Var(θ) is minimal among all possible unbiased linear estimators ( ξ = Cᵀy ) for β. 

It can be proven through direct matrix substitution into the general formula for least squares. The fact that least squares is unbiased is easy: as Xθ is always linearly fit to y, y-Xθ = 0 for any fixed set of y, so E(y-Xθ) = lim(y-Xθ) for N→∞ also = 0 (it never deviates from it). Or one can directly torture E(ξ) after expressing y through true β, and assuming that C=(XᵀX)⁻¹Xᵀ + some non-zero matrix D, as wiki does it. The key part, goes similarly; uses the fact that Var(Cy) = C∙Var(y)∙Cᵀ, and arrives at Var(ξ) = Var(θ) + DDᵀσ² (?).

> Park for now. I don't like it. Supposely it's somehow related to the triangular inequality, which suggests that there should be some reasonable geometric intuition to it, but for now I cannot find it.

In practice, it means that there are estimators with lower variance, but they are necessarily biased. Which leads to a practical program of introducing some bias in order to reduce variance. For example, if we decide to set (aka "shrink") any particular θ_i to 0, we inevitably get a biased estimate.

Refs: ESL p51, [wiki](https://en.wikipedia.org/wiki/Gauss%E2%80%93Markov_theorem)

### Dimensionality curse
Now let's consider a special case of bias-variance trade-off in a case of very high dimensions. Say, we have a linear model with error: y = Xᵀθ + ε (aka **additive error model**), where ε is normal noise with variance σ². Try to infer θ from a training set of Y. As θ = (XᵀX)⁻¹Xᵀy, we get it with errors of (XᵀX)⁻¹Xᵀε. Which means that whe we are estimating y as h = Xθ, during training we get errors for these estimations: X(XᵀX)⁻¹Xᵀε . 

J for one point xᵀ (x0 = one row in X) is given by a bias-variance formula: J = E( (h-Eh)² + (Eh-y)² ) = xᵀ(XᵀX)⁻¹xσ² + Bias + σ². Here Bias≡0 because linear fit is unbiased, and σ² is irreducable error.

If training is random, (XᵀX)→N∙Cov(X), so we have:
J ~ E( xᵀCov(X)⁻¹xσ²/N ) + σ²= trace(Cov(X)⁻¹Cov(x))σ²/N + σ² = pσ²/N + σ², where p is the number of dimentions. So J grows as a linear function of the number of dimentions p.

> This trace thing here is pretty annoying, but it works. If we E() the left part, we get a E() of sum_ij (x_i x_j C⁻¹ij), which is p^2 terms. But if you open this trace(C∙C⁻¹), you get the same expression (j coming from trace-sum, and j from internal multiplication). So E() indeed gives the trace(), and then as trace() only runs through p terms, and all of these terms happen to be =1 by def of CC⁻¹, we get what we get.

Which means that the higher the number of dimensions, the worse the error (variance). Now, some methods are worse than others: linear regression is actually faring OK, while some (like KNN regressor) get cursed pretty fast (ESL p26), but the gist is the same for all. If you simulate this, you have diff deterioration curves for different data assumptions. IRL, we try to be in-between these two extremes (linear regression and 1NN) using some info (or assumptions) about the structure of the data. 

# Philosophy of prediction
Returning to bias-variance tradeoff, we consider x and y as random variables, with a joint distribution P(x,y). We're trying to build a predictor f(x) that approximates y. The squared error loss: L = (y-f)² = ∬ (y-f)² P(x,y) dx dy. We can split P(x,y) into P(x)∙P(y|x), and integrate by y first (inside), then by x outside. Then to minimize total L, we can bind f(x) to matching y point-wise: f(x) = E(y|X=x). Which would essentially give us the **nearest neighbor** method (see the very beginning of "Classification" chapter). An alternative to point-wise matching would be a global smooth model, like linear regression. It makes regression and 1NN two opposite extremes of what can be done with the data. Other stuff (like **additive models** where f = ∑ f_j(x)) are kinda in-between.

In spirit, all models are about setting local and global constraints on the behavior of predictor (such as **behavior in the neighborhood**, that is const for KNN, linear for local linear etc.), and the **granularity** of these  neighborhoods. Some methods (kernels, trees) make this granularity parameter explicit, some don't. And high dimensionality is a problem for all methods, regardless of how they work. 

ESL defines 3 broad classes of model smoothing / constraining:
* Roughness penalty
* Kernel methods (aka local regression)
* Basis functions and dictionaries

**Roughness penalty**: Use basic RSS (Residual Sum of Squares) with an explicit penalty: L = RSS(f) + λJ(f), where J grows as f() becomes too rough. For example, defining J as λ∫(f'')²dx leads to **cubic smoothing splines** as an optimal solution. Roughness penalty can be interpreted in a Bayesian way, as a log-prior.

**Kernel methods**: explicitly specify local neighborhoods, and a type of function to fit it locally. The neighborhood is defined by the kernel function K(x0,x) that acts as weights for x around x0. Gaussian Kernel: K = 1/λ exp( - |x-x0|² / 2λ). The simplest way to use kernels, is to calculate a weighted average (aka Nadaraya–Watson kernel regression): f(x0) = ∑ K(x0,x_i)∙y_i / ∑ K(x0,x_i).

Or we can set some sort of smooth f(x), and use kernels for RSS calculations: RSS(x0) = ∑ K(x,x0)∙(y-f(x))² , where ∑ runs through all (x_i, y_i). If we assume f(x) = θ0 + θx, we get **local linear regression**. KNNs (see [[03_Classification]]) can also be considered a subtype of a kernal method, just with a weird step-wise kernel.

**Basis functions**: includes linear and polynomial regressions, but the idea is that you have a basis of functions on R^n, and project into it: f = ∑_i θ_i h_i(x). Examples: **polynomial splines**; **radial basis functions** [[RBF]] with K(μ, x) defined around several centroids μ that can itself be optimized. Gausssian kernels are still popular. Feed-forward **Deep Learning** actually also belongs here, it just htat basis functions are defined by network design (the space of functions that can be achieved with this particular network depth, activation functions etc.).

**Are there alternatives to L2?** Sure, **L1 norm** = abs(distance), which effectively pushes f(x) towards median(y) rather than the mean(y): sum of distances to 2 points is min when you're exactly between them. Hard to work with, as derivatives are discontinuous.

**Max Likelihood**: In some way, can be considered  an alternative to L2 (MSE) loss. For a data sample y_i, the log-prob of observing it is equal to: L(θ) = ∑ log P(y_i | θ), summed by i (all points). We try to find θ that maximizes ∏P, and thus ∑logP. It is connected to L2 though, as least squares maximizes likelihood for conditional probability P(y|x,θ)=𝒩(h,σ²) where h = Xβ. _ESL has a formula 2.35 for it, but without derivation. I think they may have found a derivative by θ, set it to zero, then integrate the right side by x, but I'm not sure._

# Constrained models
## Best-subset regression
Manually find a combination (subset) of k variables that offers best fit for this k. There's an efficient algorithm for that, called **Leaps and Bounds procedure**. Main ways to decide which one to use: empirical **cross-validation**, and AIC = **Akaike Information Criterion**. 

## Stepwise Regression
A (bad) compromise between performance and speed: automated stepwise selection of features based on the quality of fit, and a complexity penalty (regularization). Two main approaches: **Forward selection** and **Backward selection**. Or a combinationof both (*not sure how?*)

The maxim about stepwise regression is that it is useless for hypothesis testing, questionable for prediction, and OK for variable selection (see [[07_Features]]). 
* It can't be used for hypotheses, as p-values after this procedure are meaningless, and arguably shouldn't even be reported, as their direct probabilistic intepretation is impossible, and adjustment is too complicated (p-values aren't independent, so you cannot do FDR), making adjustment impossible in practice.
* It can be used for prediction, but usually full models (or, assumably, probably regularized models) are more powerful anyways.
* It's an OK method to select variables tho.

Some people (like Thompson cited below) are really vehemently against them tho. That's because they are usually too optimistic (the very concept of degrees of freedom becomes problematic after multiple testing), yet are strictly less powerful than proper regularization, and also typically don't replicate, as the sequence of descent depends on the data, so performing stepwise regression on different subsets of data is much more likely to yield different results, compared to a more sophisticated model.

Refs:
* [[Guyon2003variable]], [wiki](https://en.wikipedia.org/wiki/Stepwise_regression), ESL p59
* Steyerberg, E. W., Eijkemans, M. J., Harrell Jr, F. E., & Habbema, J. D. F. (2001). Prognostic modeling with logistic regression analysis: in search of a sensible strategy in small data sets. Medical Decision Making, 21(1), 45-56.
* Thompson, B. (2001). Significance, effect sizes, stepwise methods, and other issues: Strong arguments move the field. The Journal of Experimental Education, 70(1), 80-93. 

There's also a thing called **Forward-Stagewise Regression** that is based on correlations between remaining features and the current residual, and it is just plan horrible.

## Ridge regression
Also known as **Shrinkage Methods**, closely related **Tikhonov regularization**. Modern DL name: **L2 regularization** (see DL chapter).

Motivation: Consider Ax = b, and x doesn't exist. In a most typical practical case, it gives a superposition of an over-determined (still unsolvable) problem Aξ = b for the component of x that is in the row-space ( ξ = proj(x→RowSpace(A)) ), and so is affected by the actual value of the matrix A, and an under-determined problem A(x-ξ) = 0 for the components x-ξ that are in the null-space of A (orthogonal to the RowSpace(A), and thus projected to 0). The part x-ξ can be chosen arbitrary without any effect on the value of Ax. So we are free to pick this part in some "nice" fashion, to satisfy some priors, for example, or minimize solution complexity. 

Compared to variable selection, this approach is smooth and stable: while with model (variable) selection loss may oscillate pretty wildly as you add more features, with shrinkage, it usually changes monotonously.

This is especially important in case of **Multicollinearity**, when you're trying to predict y from many variables X, in a way Xθ = y (observations of variables x_i for different training points become columns of X, while regression coefficients form θ), but some of x_i are strongly correlated. In this case trying to painfully minimize y-Xθ would be counter-productive, as we'd fit noise in y with noise in x_i. Imagine an extreme case: all p columns of X are the same (rank=1), but are observed with noise, and noise is independent (so formally rank = p). What we actually need is only one (doesn't matter which one) θ_i>0, and all others ≡0. But what will happen, is that we'll fit noise in y with noise in X.

**Ridge regression**: we add a penalty on θ in the form of λ∑θ². Note that this sum runs from 1 to p, as θ0 (intercept) is not penalized. Included in the formula for h, but is not penalized. Alternatively, one can just insist that λ∑θ²<t; because the rest of the loss is defined by {X,y}, for any given task there's an exact match between λ and t. As the values of θ depend on the scale of x_i, standardization of x (de-biasing + normalization) becomes critical.

The name "**ridge**" comes from a visual example of what is describe above. Imagine that only part of the solution is well defined, and the rest is close ot null-space of X. Then the "true solution" is a "generalized cylinder" made by the true solution (in those coordinates that make sense), arbitrarily extended across the "irrelevant coordinates". Small changes in training data (right side of the Xθ=y equation) would sway the solution along this "ridge". By adding regularization we change a "ridge" into a "peak" (lines turn into parabolas), which stabilizes the solution against perturbations in both X and y.

**How to pick  λ?** One method: **Ridge trace**: plot found coefficients as a function of λ, and eyeball value at which they stop oscillating, and switches to converging (not in the sense of becoming const, but in the sense of switching from curving up and down to monotone almost-linear change). A better approach, of course, is **cross-validation**. Also, for any given λ, one can estimate the effective degrees of freedom (as a trace of certain matrix, see ESL p68), so for these plots it's common to use df(λ) for x axis, not λ itself.

Interestingly, if all x_i (columns, variables) are orthonormal, ridge regression has no reasons to penalize any one θ in particular, and just all of them get scaled by 1/(1+λ).

> I am not quite sure why this is the case, even though it feels like something that could be explained visually. It seems that the point where an ellipse and a circle touch doesn't necessarily lie on a straight line between the centers of said circle and ellipse. #todo : figure it out

**Tikhonov regularization**: a generalization of Ridge. In this case we minimize |Ax-b|² + |Γx|² where Γ is some matrix. If Г=identity matrix I multiplied by a coefficient λ, we have a sum of squared x_i, and so Ridge regression. 

> ESL p67 also seems to suggest that ridge regression is somehow related to PCA, or can be understood in terms of first moving to the PCA space, then doing something, then projecting back, but the math is sketchy, and no good comments, so I'll park it for now.

#todo : Deconstruct this post that seems to have a more concise and systematic summary of all math involved in parallels between shrinkage and PCA, and in the same system as ESL: https://stats.stackexchange.com/questions/220243/the-proof-of-shrinking-coefficients-using-ridge-regression-through-spectral-dec

Refs:
* [Stackexchange](https://stats.stackexchange.com/questions/118712/why-does-ridge-estimate-become-better-than-ols-by-adding-a-constant-to-the-diago/119708#119708), on visual interpretations
* ESL p61

## Lasso and Elastic Net
**L1 regularization**, aka **basis pursuit**: require ∑|θ_i| < t, or add λ∑|θ_i| penalty to the loss (again summing from 1 to p, meaning no punishment for the intercept θ0). Unlike for L2, it has no closed form solution (because abs() is harder to optimize).

While Ridge tends to scale θ_i values down with a coefficient, lasso tends to slide (shift) them towards zero by a certain value, until they bury in 0 and become 0. For an orthonormal X, this can be proven theoretically (ESL p71).

Visual representation: romboid and a quadratic (elliptic) optimum nearby. A circle (ridge) projected on an elliptic field always has a global minimum. A corner made of two lines (lasso) would either reach a mininum on a line (non-zero optimal θ), or at the corner (zero θ). Graphically, it's easy to see that θ sets to 0 the moment the unpenalized minimum leaves the band made of 2 other sides of the diamond (draw it to see it).

Generalizaton: one can try to use a penalty of λ∑|θ_i|^q, where q is some value in R. Foor q=2 we get ridge, for q=1 lasso, for q in (0,1) - something in-between.

**Elastic net penalty**: An mix of both Ridge and Lasso that can be used to approximate these generalized romboid shapes. It's way easier to calculate, and also shares the ability to set coefficients to exactly 0 (because of pointy points )

## Least Angle Regression
Similar to stepwise regression, but with penalized coefficients, as in shrinkage. The approach:
1. Standardize all variables
2. Set up the residual; initialize it as r=y-intercept
3. Add a variable x_i with a highest correlation to the bag. Set θ_i := 0.
4. Move θ (linearly; not as in gradient descent) towards the least-squares value ⟨x_i , r⟩. Calculate the total correlation of variables in the bag (Xθ) with the residual.
5. Calculate correlations of all runners-up (other x_j currently not in the mix) with the residual r. The moment some x_j becomes as correlated with r as the current Xθ, add x_j to the mix.
6. Repeat 4-5 until least squared is reached.

As a result of this procedure, we get a piecewise-linear change of θ with "iteration time". The results are pretty similar to that for lasso, except for histeresis around θ=0 point.

> Not sure what are the benefits of this approach though.

## Principal Components Regression
1. Standardaze columns of X.
2. Calculate **PCA** of X: Z=XV, where columns of Z are orthonormal.
3. Regress y on Z: Zξ~y. As z_i are orthonormal, it's like running p univariate regressions.
4. Recalculate θ from ξ: θ = Vξ, but only using first m biggest eigenvectors.

Similar to Shrinkage, in the sence that Shrinkage also ends up shrinking components with small eigenvalues more. With the principal components regression, we just zero them altogether. Unlike ridge, it can amplify "unworthy" coefficients in the original X space though, what would have been shrunk by both Ridge and Lasso.

Refs: ESL p79

## Partial Least Squares
1. Standardize columns of X
2. Start cycle i=1
3. Calculate φij = ⟨x_j , y⟩ for each i. Synthesize z_i = ∑ φij xj (by j)
4. Regress (project) y by z_i, get ξ_i.
5. Switch from y to residual (⊥ z_i)
6. Orthogonalize all x_j to z_i (project to ⊥ z_i)
7. Repeat 3-6 m times (m<p)
8. Project ξ back to θ

The idea behind is to let y pull signal from X, regardless of how this signal is encoded (mixed?) in columns of X. ESL claims though that "variance aspect tends to dominate", so in practice this method doesn't behave too differently than Ridge regression. Seems to also be similar in spirit to "Canonical Correlation Analysis" (see [[04_Features]] section). Same as PCR above, may inflate weaker dimensions, making it unstable. Which all together is probably why it is not that widely used? 

Refs: ESL p81, [wiki](https://en.wikipedia.org/wiki/Partial_least_squares_regression)