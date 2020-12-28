# KL and JS divergences

Parents: [[information]]
Related: [[cross-entropy]]

#entropy


**Kullback–Leibler Divergence (KL)** between 2 probability distributions P and Q: 
$D(P||Q) = -∑p\cdot\log(q/p)$ 
(And yes, they really use this funny symbol || everywhere.)

Note that:

* We divide by p, so numerically p cannot be 0 anywhere. Even though p∙log(1/p)→0 for p→0.
* **Asymmetric**. P is the "target distribution" (baseline), and Q is the one you are trying to compare to P.

> Does it mean that in practice they add ε>0 to p?

Sometimes is called _relative entropy_ of P in respect to Q. Like, how much more you would have learned if you updated from Q to P (Bayesian). Or how many extra bits you would need to transmit a message from P using a code optimized for Q, as KL = D(P || Q) = H(P, Q) − H(P).

$H(P) = Σ p \log(p) = Σ p \log (p q / q) = Σ p (\log(q) + \log(p/q)) = Σp \log(q) - Σ p \log (q/p) = H(P,Q) − D(P||Q)$

As a corollary, the entropy for a variable X following distribution P is lower than the max possible entropy (that for a uniform distribution) exactly by its KL divergence with uniform distribution:
H(X) = log(N_events) - D( P || uniform_across_events ) .
For uniform P(x), we have max entropy, so by updating from it to P, we demonstrate that we learned something, so entropy decreases.

> Interesting that if we think of it in terms of an update, it is P that is a posterior, or an update, while if we use it as a loss function, P is the "ground truth", and Q is compared to it. So which one is known first, and which one second, is kind of opposite in these two scenarios. What is common, is that P is a better known, and thus a "more true" distribution.

KL divergence is also related to mutual information: I(X;Y) = D_KL( P(X,Y) || P(X) P(Y) ) . That is, how much extra information you have, if you know how events from X and Y match with each other, compared to if you only knew them separately).

# JS divergence

**Jensen-Shannon divergence**: the most commonly used symmetric alternative to LK.
JSD(P||Q) = ( LK(P||M) + LK(Q||M) )/2
where M = (P+Q)/2
Always between 0 and 1.


# Refs

https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence

https://en.wikipedia.org/wiki/Jensen%E2%80%93Shannon_divergence