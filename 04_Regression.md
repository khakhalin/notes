# Regression

## Linear Regression

## Bias-Variance Tradeoff

An estimate for the number of training examples needed ot learn problems of certain dimensionality.

Generalization error consists of two parts: bias and variance.

**Derivation for a linear system:** (ref: [Some lecture](https://www.youtube.com/watch?v=zUJbRO0Wavo) by Kilian Weinberger)

Say, regression x->y on a dataset (x,y) from distribution P(x,y).
For a given x, you always have P(y|x), so that P(x,y) = P(y|x)P(x).
Regression predicts expected E y(x) which is weighted by y (integrated by P(y|x) ).
ML is about guessing Ey(x) using some algo. 
Say, h(x) is the guess (aka classifier), produced by algo A, based on a training set D.
The expected error for this estimation h is E (h-y)^2 = $\iint$ of P(x,y)(h(x)-y)^2 by x and y.
h itself is a random variable, as it's a function of D, which is a random subset of (x,y).
So we can consider expected Eh, by integrading over all possible training sets D.
(We can estimate it by training lots of times and averaging. For regression, it's regression. For other loss functions estimations may be more complicated, but still possible. In practice, if you are only interested in E(h), you could also just apply A to the full dataset, obtaioning the best possible h possible.)
Now, what's the expected error for the algorithm (across all possible classifiers it could possibly produce) is E integrated overe all h (so over all Ds), so we have a triple-integral (by D, x, y).
$\int_D \int_x \int_y (h(x)-y)^2 P(D) P(x,y)\,dD\,dx\,dy$
Add and subtract mean h (Eh) inside the square, get h-Eh and Eh-y, open squares.
(h-Eh)^2 becomes variance, (Eh-y)^2 eventually becomes bias.
2(h-Eh)(Eh-y) dies out because for each fixed x you get a fixed (Eh-y), and int_D (h-Eh) = 0 by def of Eh.
Except we need to massage (Eh-y)^2 some more. Let's subtract and add Ey, then open squares.
This time we're left with (Eh-Ey)^2 and (Ey-y)^2, while 2(Eh-Ey)(Ey-y) will similarly die out at int_y step. 
(Just make sure to integrate by y at the deepest level, to get 0 before int_x has a chance to see it).
So we ended up having 3 terms:
* Variance of the classifier
* Bias (squared)
* Variance of the data, aka noise, or irreducable error
So sort of precision / accuracy situation.

> **My current understanding** The point is that if you try to minimize bias (make your classifier stick closer to the data), you fall at mercy of your training set, and so increase variance. Each particular classifier, understood as an instance produced by some particular set of training data, will cling to this testing data. So all classifiers will be slightly different, if you train on different data. If you believe that the underlying solution has some structure, you may want to restrain your classifier, so that it would not change that widely for different subsets of your testing data (regularization). It means that for each given training set you'll get higher bias, but you'll reduce variance (of your classifier, across all possible training set), and so hopefully would stay closer to the "ground truth". Essentially, a parsimony principle: simpler models are better.

> So it's directly related to overfitting. Low bias = high variance = very good fit on the training set, but horrible fit on the testing set (or rather, validation set).

> But look, it would only work if the ground trugh is actually simple. Why does it work? Why is the ground truth actually simple? Is it some expected statistical property that the world actually follows?.. Or is it because a typical practical problem has certain properties? Seems like that; see below.

## Ridge regression

Seems to be an old name for **L2 regularization** (see DL chapter). Another name: **Tikhonov regularization**. 

Motivation: Consider Ax = b, and x doesn't exist. In a most typical practical case, it gives a superposition of an overdetermined problem Ax' = b for the component of x that is in the row-space, and so is affected by the matrix (x' = projection of x into row-space), and an underdetermined problem A(x-x') = 0 for the components x-x' that are in the null-space of A, and so can be chosen arbitrary without any effect on Ax. Everything that in the null-space cannot bring Ax closer to b, so we are free to pick them in some "nice" fashion, for example, to satisfy some priors, or minimize complexity.

With Tikhonov regularization, we minimize not squared Euqledian norm |Ax-b|^2 but |Ax-b|^2 + |Γx|^2 where Γ is some matrix; often identity matrix I multiplied by a coefficient, in which case we just have a sum of squared x_i (L2 regularization) multiplied by a coeff (usually denoted k). 

This is especially important in case of **Multicollinearity**, when you're trying to predict y from many variables a_i, in a way Ax = y (observations of variables a_i for different training points become columns of A, while regression coefficients form x), but some of a_i are strongly correlated. In this case trying to painfully minimize y-Ax would be counter-productive, as we'd fit noise in y with noise in a_i. Imagine an extreme case: all columns of A are the same (rank=1), but are observed with noise, and noise is independent (so formally rank = N). What we actually need is only one (doesn't matter which one) x_i>0, and all others 0. But what will happen, is that we'll fit noise in y with noise in A.

The name "ridge" comes from a visual example of what is describe above. Imagine that only part of the solution is well defined, and the rest is close ot null-space of A. Then the "true solution" is a "generalized cylinder" made by the true solution (in those coordinates that make sense), arbitrarily extended across the "irrelevant coordinates". Small changes in training data (right side of the Ax=y equation) would sway the solution along this "ridge". By adding regularization we change a "ridge" into a "peak" (lines turn into parabolas), which stabilizes the solution to perturbations in both A and y. ([Source: stackexchange](https://stats.stackexchange.com/questions/118712/why-does-ridge-estimate-become-better-than-ols-by-adding-a-constant-to-the-diago/119708#119708))

How to pick the coefficient k? One method: **Ridge trace**: plot found coefficients as a function of k, and eyeball value at which they stop oscillating, and start converging (not in the sense of becoming const, but in the sense of of monotone almost-linear change).

## Logistic Regression

Logistic function: $\sigma(x) = \frac{1}{1+e^{-x}} = \frac{e^x}{1+e^x}$.

The inverse function is called logit(p) $=\ln \frac{p}{1-p}$

Assumes that log-odds $\log \frac{p}{1-p}=a+bx$ , so is a linear function of x (that is generally a vector). It means that p/(1-p)=exp(ax+bx), which leads to p = σ(ax+b).

