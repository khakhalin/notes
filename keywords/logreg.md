# Logistic Regression

#classification

Parent: [[03_Classification]], [[02_Regression]]
See also: [[softmax]], Discriminant Analysis, [[svm]]

By Bayes theorem, our best guess about P(g|x) woudl be P(g|x) = f(x)p/∑f(x)p , where p is prior probability, and sum goes through all classes. Not only it looks a more like a probability, but actually leads to the math behind logistic regression.

**Logistic function**: $\displaystyle \sigma(x) = \frac{1}{1+e^{-x}} = \frac{e^x}{1+e^x}$. The inverse function is called **logit**: logit(p) $=\ln \frac{p}{1-p}$. **Logistic regeresion assumes that log-odds** are linear: log p/(1-p) = Xθ, which means that p/(1-p)=exp(Xθ), leading to p = σ(Xθ). This shape is nice and doesn't suffer from class masking.

For a multi-class case, the log-odds for each class g_i are fit linearly: log P(x∈gi)/P(x∈gk) = xᵀθi. Here g_k is some reference class (often the last one on the list); it doesn't really matter which one exactly; what matters is that, if you check it, with this formula we always get ∑P(gi)=1.

To find a solution, use max-likelihood (maximize conditional P(g|x) ), which is equivalent to **minimizing log-loss**. For two classes, log-loss = -∑(y log(P(1|x)) + (1-y)log(1-P(1|x))) , with the sum taken over all training points x_i and their labels y_i∈{0,1}. As with p→0, we have −log(p)→inf, we can expect numerical problems when the data is too clean and some areas only contain points of one type, as weights can explode (it's hard to fit infinity!), making  **regularization** extremely important. 

The expression for loss can be simplified: 
L(θ) = -∑( y log(p) + (1-y)log(1-p) ) =
-∑( y (log(p) - log(1-p)) + log(1-p)) =\
-∑( y log(p/(1-p)) + log(1-p) ) = …

...Now log(p/(1-p)) is just plain Xθ by definition above. For log(1-p), substitute p=sigmoid(xᵀθ) = 1/(1+exp(-xᵀθ)), then calculate 1-p, mutiply both numerator and denominator by exp(xᵀθ), take a log, use log(1/a) = -log(a), resulting in log(1-p) = log(1+exp(xᵀθ)). Put both in the formula above, get:

L(θ) = … = -∑( yxᵀθ - log(1+exp(1+xᵀθ)) ). 

Differentiate this by θ, set to 0. Get ∂L/∂θ = −∑ (xy − x∙exp(xᵀθ)/(1+exp(xᵀθ))) = −∑x(y-p) = 0. Here each x (each point) and $\vec 0$ on the right side are both vectors length ndim+1 (one extra dim for the intecept $θ_0$). 

> ESL p120-121 gives a solution for the updating (descent) procedure that I skip for now. Also there's a weighted self-consistency formula that ties θ, x, y, and p together, and can apparently be used to achieve some numerical shortcuts.

## Regularized Log Regression

**L1 penalty** (lasso) is good for this: take the previous L=-∑ y log(p) + (1-y) log(1-p), and add to it λ∑|θj| , where the sum runs by dimention (variable). Flip signs if you like maximizing stuff, as ESL does. Apparently, if you do the math, you get the following link between everything: xjᵀ(y-p) = λ∙sign(θj) for each dimension j.
