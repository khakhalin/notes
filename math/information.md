# Information and entropy

#todo #math #entropy #stats #bib

Subtopics:
* Entropy - described below
* [[cross-entropy]]
* [[kl]] - KL (Kullback–Leibler) and JS (Jensen-Shannon) Divergences
* [[gini]] - a classification loss important for [[gbm]]

# Entropy

Let's start from the very beginning. Intuitively, for a single **event** x happening with probability p(x), the amount of information obtained if we learned that the event happened is −lg(p(x)). We want the measure of information behave that way, as it would mean that when two independent events happen together, **probabilities multiply, but informations add**. For the information definition of entropy we usually use a base-2 logarithm: lg(). _(But  I'm not consistent with notation here, and most books aren't)._

For a **random variable**, entropy can also be defined as the "expected smallest number of bits needed to describe the state of this system". If all states are equally probable, you always need as many bits as you have for a binary representation for the number of states; you cannot cheat. For example, if you have 3 states to encode, a binary code (two letters, 0 and 1, or "a" and "b"), and messages that are at most 2-letters (bits) long, we'll have to use something like (a, b, aa) to encode the 3 states. So the average (expected) message length will be 1/3+1/3+2/3 = 4/3 bits. But if the probabilities for 3 states are (1/2, 1/4, 1/4), then we can can assign a longer word to one of the less frequent states, and the expected length will be only 1/2 + 1/4 + 2/4 = 5/4, which is smaller.  

An actual formula is: E = −∑p(x)∙lg(p(x)). As illustrated above, when the distribution of probabilities for different events is highly skewed (in an extreme case: almost always one value), entropy is small. For a constant value, it will converge to 0, as x log(x) → 0 when x→0. 

> The way it is proven BTW, is by replacing x with "divide by 1/x", then taking a derivative from both numerator and denominator using the "L'Hopital's Rule".

> $\displaystyle \lim_{x→0} x \log(x) = \lim_{z→∞} \frac{\log(1/z)}{z} = \lim_{z→∞} \frac{\frac{1}{1/z}\cdot\frac{-1}{z^2}}{z}=\lim_{z→∞}\frac{-1}{z^2}=0$.

For a **discrete uniform distribution**: H(unif) = −Σp log(p) = −Σ1/n log(1/n) = −log(1/n) = log(n)

**Entropy is maxxed for a uniform distribution.** Proof: consider entropy for a uniform distribution. Now let's redistribute p(x) a bit (as Σp=1 we cannot just change p(x) for some x; it has to be a redistribution from x1 to x2). So $p(x_1)→p+ε$, and $p(x_2)→p-ε$ (originally all of them were == to just $p$, because uniform). How did the entropy change?
$ΔE = E'-E = -\big((p+ε)\log(p+ε) - p\log(p) + (p-ε)\log(p-ε) - p\log(p)\big)$. 

Collecting the terms with p and ε, and moving everything under logs:
$-\big(p\log((p+ε)(p-ε)/p^2) + ε\log((p+ε)/(p-ε))\big)$ = 
$-\big(p\log((p^2-ε^2)/p^2)+p\log(\exp(ε/p)\cdot(p+ε)/(p-ε))\big)$ = 
$-p\log((p+ε)^2/p^2\cdot \exp(e/p))$

Under the log we have a fraction that is >1 multiplied by exp of a small but positive expression, so it's also >1. Therefore everything under the log is >1, so the log is >0, so the expression as a whole is <0. Which means that $E'<E$, and so as we went further away from the uniform distribution, entropy decreased.

Now, it may seem as if this is only true near uniform distribution, so we only proved that it is a local max, not a global one. But actually this observation is true for any ε, not only for small ε; and moreover, it doesn't depend on the value of p(x) for other x, so what we actually proved is that at least for a discrete distribution, any step away from a uniform distribution (or rather, from p(x) for two different x being ==) increases the entropy, and conversely, any step 

# Refs

* Jason Brownlee about entropy (rather confusing tbh): https://machinelearningmastery.com/what-is-information-entropy/
* http://pillowlab.princeton.edu/teaching/statneuro2018/slides/notes08_infotheory.pdf
* Not really that intuitive, but they are trying: https://math.stackexchange.com/questions/331103/intuitive-explanation-of-entropy

Stone, J. V. (2018). Information Theory: A Tutorial Introduction. arXiv preprint arXiv:1802.05968.
https://arxiv.org/abs/1802.05968