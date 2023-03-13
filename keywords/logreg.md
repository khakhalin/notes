# Logistic Regression

Parent: [[03_Classification]], [[02_Regression]]
See also: [[linear_regression]], [[linear_separation]], [[softmax]], [[lda]], [[svm]], [[loss]], [[ranking]]

#classification #regression


Intuition: we want something like linear regression (as simple as it gets), but for classification. So we want a function of Xθ. But what function?
* We could just do Xθ (pure [[linear_separation]]). The problem is that we want outputs to be probabilities of point x belonging to a class, and probabilities are within 0 and 1. And then there's also a problem of class masking. We want something nonlinear: f(p) = Xθ
* What nonlinearity? One of the most logical choices is to use ln(p), as then a point 2 times further from the decision line would have p² probability of belonging to a class But ln(p) is also not bound.
* So let's use the "next simplest formula", **logit**: logit(p) $=\ln \frac{p}{1-p}=Xθ$. It behaves close to ln(p) for small p; almost linearly around p=0.5, and symmetrically for p close to 1. Logistic regression for two classes assumes that **log-odds are linear in the space of X**

The inverse of logit is a **Logistic function**: $\displaystyle \sigma(x) = \frac{1}{1+e^{-x}} = \frac{e^x}{1+e^x}$. As logit is linear, we have: $p/(1-p)=\exp(Xθ)$, leading to $p = σ(Xθ)$, if you solve for p. This shape is nice and doesn't suffer from class masking.

To find a solution, use max-likelihood, by maximizing conditional probabilities of observing the data, given the model. This is equivalent to **minimizing log-loss** (see [[cross-entropy]]). For two classes, the overall probability of observing a set of labels y_i ∈ {0,1} for points x_i, given model predictions $p_i = p(x_i → 1)$ is equal to $P = P_0 P_1$, where $P_1$ is the joint probability of observing all those data points that actually belong to class (y=1) and so $P_1 = \prod p_i$, while $P_0$ is the joint probability to observe all the zeros $P_0 = \prod (1-p_i)$. To combine both products into one product, let's use y_i as an indicator vector, producing $P = \prod (y_i p_i + (1-y_i)(1-p_i))$. Now instead of maximizing P, let's minimize its negative log, calling it "log-loss": $L = -∑_i(y_i \log p_i + (1-y_i)\log(1-p_i))$ , with the sum taken over all training points x_i and their labels y_i. 

(All of this is assuming that all observed points are **independent**! If they aren't independent, then we can't break joing probability into a product, and so log regression shouldn't work!)

Note that with p→0, we have −log(p)→inf, so we can expect numerical problems when the data is too clean and some areas only contain points of one type, as weights for these points will explode (it's hard to fit infinity!). In practice, its very important to **regularize** log-regression (see below).

The expression for loss L can be simplified: 
$L(θ) = -∑( y \log(p) + (1-y)\log(1-p) ) =$ ...group terms with y in them...
$-∑( y (\log(p) - \log(1-p)) + \log(1-p)) =$ … combine logs with y into a log of a fraction ...
$-∑( y \log(p/(1-p)) + \log(1-p) ) =$ … 
now log(p/(1-p)) is just plain Xθ by definition above.
For log(1-p), we need to do some math. First substitute $p=σ(xᵀθ)= 1/(1+\exp(-xᵀθ))$. Then calculate 1-p. Multiply both numerator and denominator by exp(xᵀθ), take a log. Use log(1/a) = -log(a), resulting in log(1-p) = log(1+exp(xᵀθ)). Put both in the formula above, get:

$L(θ) = … = -∑( yxᵀθ - \log(1+\exp(1+xᵀθ)) )$. 

Differentiate this by θ, set the derivative to 0. Get $∂L/∂θ = −∑ (xy − x\exp(xᵀθ)/(1+\exp(xᵀθ)))$ 
$= −∑x(y-p) = 0$. Here each x (each point), as well as the $\vec 0$ on the right side, are both vectors length ndim+1 (one extra dim for the intercept $θ_0$). Now we can optimize for θ using gradient descent. 

And that's why log-regression (that is, assuming that p follows a sigmoid curve) is so good: it leads to a really simple formula that is almost identical to that for linear regression, except that instead of X(Y-Xθ) = 0, we have X(Y-σ(Xθ)) = 0. And it often works well. But that said, it's less "divinely inspired" than some other techniques; in case of logistic regression, tradition is a big part of why it is so popular.

> ESL p120-121 gives a solution for the updating (descent) procedure that I skip for now. Also there's a weighted self-consistency formula that ties θ, x, y, and p together, and can apparently be used to achieve some numerical shortcuts.

# Regularized Log Regression

**L1 penalty** (lasso) is good for this: take the previous L=-∑ y log(p) + (1-y) log(1-p), and add to it λ∑|θj| , where the sum runs by dimension (by coordinate). Apparently, if you now do the math, you get the following link between everything: $xθ_j^\top (y-p) = λ\cdot\text{sign}(θ_j)$ for each dimension j.

> Not sure what it means. Check elsewhere or derive! #todo

# Multi-class log regression

For a multi-class case, the log-odds for each class g_i are fit linearly: log P(x∈gi)/P(x∈gk) = xᵀθi. Here g_k is some reference class (often the last one on the list); it doesn't really matter which one exactly; what matters is that, if you check it, with this formula we always get $∑_iP(g_i)=1$.

> Wait, why? Check and explain. #todo

# Parts that I summarized from somewhere that I no longer understand

By Bayes theorem, our best guess about $P(g|x)$, the probability of point x belonging to a class g, would be $\displaystyle P(g|x) = \frac{f(x)p_g}{\sum_h f(x)p_h}$ , where p_g is prior probability of belonging to this class, and the sum goes through all possible classes. Not only it looks a more like a probability, but actually leads to the math behind logistic regression.

# To-Read

Ng, A. Y., & Jordan, M. I. (2002). On discriminative vs. generative classifiers: A comparison of logistic regression and naive bayes. In Advances in neural information processing systems (pp. 841-848).
https://ai.stanford.edu/~ang/papers/nips01-discriminativegenerative.pdf

# Refs

How loss is derived from the max likelihood principle:
https://www.expunctis.com/2019/01/27/Loss-functions.html

Nice lecture notes by Cosma Shalizi:
https://www.stat.cmu.edu/~cshalizi/uADA/12/lectures/ch12.pdf

Logistic Regression Blues. Will M. Gervais. 9/28/2020
https://rpubs.com/wgervais/667244