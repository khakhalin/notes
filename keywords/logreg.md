# Logistic Regression

Parent: [[03_Classification]], [[02_Regression]]
See also: [[linear_regression]], [[linear_separation]], [[softmax]], [[lda]], [[svm]], [[loss]], [[ranking]]

#classification #regression


Intuition: we want something like linear regression (aka as simple as it gets), but for classification, returning some sort of probabilities. So we want some function of Xθ. But what function would it be?

* We could just do Xθ (pure [[linear_separation]]). But they won't naturally behave as probabilities (won't fit between within 0 and 1). And then there's also a problem of class masking. We want something nonlinear: $f(p) = Xθ$
* What nonlinearity to use? It feels that something like a $\log(p)=Xθ$ could be a good choice, as in this case a point 2 times further from the decision line would have had a squared "score" $\exp(2d)=\exp(d)^2$ of not belonging to a class. But this "score" still wouldn't be a true probability, as exp(Xθ) is unbound.
* So let's use the "next simplest formula", **logit**: logit(p) $=\ln \frac{p}{1-p}=Xθ$. It behaves similar to ln(p) for small p; almost linearly around p=0.5, and symmetrically around this point. Logistic regression for two classes assumes that "log-odds are linear in the space of X" (a memorizable ).

The inverse of logit is a **Logistic function**: $\displaystyle \sigma(x) = \frac{1}{1+e^{-x}} = \frac{e^x}{1+e^x}$. As logit is linear in X, we have: $p/(1-p)=\exp(Xθ)$, leading to $p = σ(Xθ)$, if you solve for p. This shape is nice and doesn't suffer from class masking.

The other way to rationalize this formula is that you have it for a conditional probability P(from class G | observation x) in case of two Gaussians in 1D. If points from G0 are distributed according to $\exp(-x^2)$, and points in class G1 come form $\exp(-(x-a)^2)$, then in-between 0 and a, the conditional probability of getting a point from G0 changes as P = P0/(P0 + P1) = $\displaystyle \frac{e^{-x^2}}{e^{-x^2} + e^{-(x-a)^2}}$  = $\displaystyle \frac{1}{1 + e^{-(x-a/2)2a}}$ , which is a logistic function, centered half-way between the centers of two Gaussians. Our high-dimensional task above, therefore, is about finding a projection to 1D axis defined by θ, to achieve best separation of points X by a hyperplane perpendicular to that axis.

To find a solution, let's use max-likelihood, and maximize the conditional probability of observing the data, given the model. This is equivalent to **minimizing log-loss** (see [[cross-entropy]]). Let's consider a case of two classes, $G_0$ and $G_1$. For each observation $x_i$ we'll calculate the probability $p_i$ that this point belongs to class $G_1$. Then we'll compare these probabilities to the actual set of labels $y_i$ ∈ {0,1} for these points, and calculate how probable it is, to get this set of labels given these probabilities. The joint probability of getting these labels $P = P_0 P_1$, where $P_1$ is the joint probability of observing all points that were observed as ones (had a label y=1), and $P_0$ is the probability of observing all the zeros. Now, $P_1 = \prod p_i$, and $P_0 = \prod (1-p_i)$, but to combine both products into one product, let's use $y_i$ as an indicator vectorin this way: $P = \prod (y_i p_i + (1-y_i)(1-p_i))$. Now instead of maximizing P, let's minimize its negative log, which is called a "log-loss": $L = -∑_i(y_i \log p_i + (1-y_i)\log(1-p_i))$. 

* All of this only works, of course, if all observed points are independent! If they aren't independent, then we can't break the joint probability into a product of point-based probabilities, and log regression shouldn't work)
* Note also that with p→0, we have −log(p)→inf, so we can expect numerical problems when the data is too clean and trivially separable (has no overlap between point domains), as weights for these points may explode. In practice, log-regression is typically **regularized** (see below).

The expression for loss L can be simplified (I drop $i$ indices here, but imagine that they are there): 
$L(θ) = -∑( y \log(p) + (1-y)\log(1-p) ) =$ ...group terms with y in them...
$-∑( y (\log(p) - \log(1-p)) + \log(1-p)) =$ … combine logs with y into a log of a fraction ...
$\displaystyle-∑ \left( y \log \left( \frac{p}{1-p} \right) + \log(1-p) \right) =$ … 
now log(p/(1-p)) is supposed to be equal to Xθ by definition above (with a yet unknown θ). For log(1-p) on the other hand, we'll need some math first. Let's substitute $p$ with $σ(xᵀθ)= 1/(1+\exp(-xᵀθ))$. Then calculate $1-p$. Multiply both numerator and denominator by $exp(xᵀθ)$, take a log. Use $\log(1/a) = -\log(a)$, resulting in $\log(1-p) = \log(1+exp(xᵀθ))$. Put both in the formula above, get:

$L(θ) = … = -∑( yxᵀθ - \log(1+\exp(1+xᵀθ)) )$. 

Differentiate this by θ, set the derivative to 0. Get $∂L/∂θ = −∑ (xy − x\exp(xᵀθ)/(1+\exp(xᵀθ)))$ 
$= −∑x(y-p) = 0$. Here each $x_i$ (each point), as well as the $\vec 0$ on the right side, are both vectors length ndim+1 (one extra dim for the intercept $θ_0$), while $y_i$ are indicators, and $p_i$ are the numbers from $σ(Xθ)$, one number per point. And we can now optimize for θ using gradient descent. 

That's another reason why log-regression is so good: it leads to a really simple formula that is almost identical to that for linear regression, except that instead of $X(Y-Xθ) = 0$, we optimize  $X(Y-σ(Xθ)) = 0$. And it often works well. But at the same time, it's slightly less "divinely inspired" than some other techniques, and there's also some tradition behind its popularity, as arguably, it is not that obvious that it is the simplest solution for a classification problem.

> ESL p120-121 gives a solution for the updating (descent) procedure that I skip for now. Also there's a weighted self-consistency formula that ties θ, x, y, and p together, and can apparently be used to achieve some numerical shortcuts.

# Regularized Log Regression

**L1 penalty** (lasso) is good for this: take the previous $L=-∑ y \log(p) + (1-y) \log(1-p)$, and add to it $λ∑\text{abs}(θ_j)$ , where the sum runs along the coordinates of the θ vector. Apparently, if you now do the math, you get the following link between everything: $xθ_j^\top (y-p) = λ\cdot\text{sign}(θ_j)$ for each coordinate (dimension) j.

> Not sure what how it works. Check elsewhere or derive! #todo

# Multi-class log regression

For a multi-class case, the log-odds for each class $g_i$ are fit linearly: $\log \left( P(x∈g_i)/P(x∈g_k) \right) = xᵀθ_i$. Here $g_k$ is some reference class (often the last one on the list); it doesn't really matter which one exactly; what matters is that, if you check it, with this formula we always get $∑_iP(g_i)=1$.

> Wait, what? Check and explain. #todo

# Parts that I summarized from somewhere, but not cannot figure out how they relate

By Bayes theorem, our best guess about $P(g|x)$, or the probability of point x belonging to a class g, would be $\displaystyle P(g|x) = \frac{f(x)p_g}{\sum_h f(x)p_h}$ , where $p_g$ is prior probability of a point belonging to this class, and the sum goes through all possible classes. Apparently, this formula also leads to logistic regression. (Is it true? 🔥)


# Refs

Ng, A. Y., & Jordan, M. I. (2002). On discriminative vs. generative classifiers: A comparison of logistic regression and naive bayes. In Advances in neural information processing systems (pp. 841-848).
https://ai.stanford.edu/~ang/papers/nips01-discriminativegenerative.pdf

How loss is derived from the max likelihood principle:
https://www.expunctis.com/2019/01/27/Loss-functions.html

Nice lecture notes by Cosma Shalizi:
https://www.stat.cmu.edu/~cshalizi/uADA/12/lectures/ch12.pdf

Logistic Regression Blues. Will M. Gervais. 9/28/2020
https://rpubs.com/wgervais/667244