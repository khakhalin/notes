# Information and entropy

#todo
Stone, J. V. (2018). Information Theory: A Tutorial Introduction. arXiv preprint arXiv:1802.05968.
https://arxiv.org/abs/1802.05968

## Entropy

For an event x with probability p(x), the amount of information obtained if we learned that the event happened is -lg(p(x)). Nice property, because if two independent events happen together, probabilities multiply, and so informations add. lg() is a base-2 logarithm.

Entropy of a random variable = expectation of entropy of a message (expectations of entropy if you observe a value of this variable), so = -∑p(x)∙lg(p(x)). Note how it makes some variables more informative (surprising, unexpected) than others: if it's highly skewed (in an extreme case: almost always one value), entropy is small (for a constant, it's 0). That's intuitive, and mathematically that's because x log(x) → 0 when x→0. (The way it is proven, BTW, is by replacing x with "divide by 1/x", then taking a derivative from both numerator and denominator that is called "L'Hopital's Rule")

References:
* Jason Brownlee about [entropy](https://machinelearningmastery.com/what-is-information-entropy/), [cross-entropy](https://machinelearningmastery.com/cross-entropy-for-machine-learning/) (confusing)

## Cross-entropy

Definition: H(p,q) = -∑P(x)∙lg(Q(x)), or integral for continuous.

Is used as loss for classification tasks (see chapters: [[03_Classification]] and [[06_DL]]).

The Information Theory interpretation: the expected message length when transmitting data from a (wrong) distribution P, using a code optimized for Q. So, Q is assumed, but P is happening. ([ref](https://en.wikipedia.org/wiki/Cross_entropy#Motivation))

Not to be confused with joint entropy, even tho some people use similar or same notation for it H(p,q). Joint entropy is just an entropy of a vector variable composed of two variables that are "joined"; nothing fancy.

## KL divergence

Kullback–Leibler Divergence between 2 probability distributions P and Q: 
D(P||Q) = -∑p∙lg(q/p) . 
(And yes, they really use this funny symbol || everywhere.)

Note that:
* division by p, so p cannot be 0 anywhere. Although p∙log(1/p)→0 for p→0.
* Asymmetric. P is the "target distribution", and Q is the one that you compare to it.

Sometimes is called _relative entropy_ of P in respect to Q. Like, how much more you woudl have learned if you updated from Q to P (Bayesian). Or how many extra bits you would need to transmit a message from P using a code optimized for Q, as KL = D(P || Q) = H(P, Q) - H(P).

From this point of view, entropy for a variable X following distribution P is lower than the max entropy for a uniform distribution exactly by its KL divergence with uniform distribution:
H(X) = log(N_events) - D( P || uniform_across_events ) .
For uniform P(x) for every x we have max entropy, and by updating from it to P, we demonstrate that we learned something, and so entropy decreases ([wiki](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence)).

> Interesting that if we think of it in terms of an update, it is P that is a posterior, or an update, while if we use it as a loss function, P is the "ground truth", and Q is compared to it. So which one is known first, and which one second, is kind of opposite in these two scenarios. What is common, is that P is a better known, and thus a "more true" distribution.

KL divergence is also related to mutual information: I(X;Y) = D_KL( P(X,Y) || P(X) P(Y) ) . That is, how much extra information you have, if you know how events from X and Y match each other, compared to if you only know them separately).

## JS divergence

Most commonly used symmetric alternative to LK ([wiki](https://en.wikipedia.org/wiki/Jensen%E2%80%93Shannon_divergence)). 
JSD(P||Q) = ( LK(P||M) + LK(Q||M) )/2
where M = (P+Q)/2
Always between 0 and 1.