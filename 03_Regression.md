# Regression

## Bias-Variance Tradeoff

An estimate for the number of training examples needed ot learn problems of certain dimensionality.

Generalization error consists of two parts: bias and variance.

**Derivation for a linear system:** (ref: [Some lecture](https://www.youtube.com/watch?v=zUJbRO0Wavo) by Kilian Weinberger)

Say, regression x->y on a dataset (x,y) from distribution P(x,y).
For a given x, you always have P(y|x), so that P(x,y) = P(y|x)P(x).
Regression predicts expected E y(x) which is y weighted (integrated) by P(y|x).
ML is about guessing Ey(x) using some algo. 
Say, h(x) is the guess (aka classifier), produced by algo A, based on a training set D.
The expected error for this estimation h is E (h-y)^2 = double-integral of P(x,y)(h(x)-y)^2 by x and y.
h itself is a random variable, as it's a function of D, which is a random subset of (x,y).
So we can consider expected Eh, by integrading over all possible training sets D.
(We can estimate it by training lots of times and averaging. For regression, it's regression. Of other loss functions estimations may be more complicated, but still possible. In practice, if you are only interested in E(h), you could also just apply A to the full dataset, obtaioning the best possible h possible.)
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

https://en.wikipedia.org/wiki/Bias%E2%80%93variance_tradeoff
**My current understanding** The point is that if you try to minimize bias (make your classifier stick closer to the data), you fall at mercy of your training set, and so increase variance (each classifier will cling to the testing data, so all classifiers will be slightly different, if you train on different data). If however you believe that the underlying solution has some structure, you may want to restrain your classifier (regularization). It means that for each given training set you'll get a bias, but you'll reduce variance (of your classifier, across all possible training set), and so hopefully would, stay closer to the "ground truth". Essentially, a parsimony principle: simpler models are better.

So related to overfitting.

But look, it would only work if the ground trugh is actually simple. Why does it work? Why is the ground truth actually simple? Is it some expected statistical property that the world actually follows?..

## Linear Regression

## Logistic Regression

Logistic function: $\sigma(x) = \frac{1}{1+e^{-x}} = \frac{e^x}{1+e^x}$.

The inverse function is called logit(p) $=\ln \frac{p}{1-p}$

Assumes that log-odds $\log \frac{p}{1-p}=a+bx$ , so is a linear function of x (that is generally a vector). It means that p/(1-p)=exp(ax+bx), which leads to p = σ(ax+b).

