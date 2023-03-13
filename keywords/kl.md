# KL and JS divergences

Parents: [[information]]
Related: [[cross-entropy]]

#entropy


**Kullback–Leibler Divergence (KL)** between 2 probability distributions P and Q: 
$D(P||Q) = -∑p\cdot\log(q/p)$ 
(And yes, they really use this funny symbol || everywhere.)

Compare that to a formula for cross-entropy ([[cross-entropy]]):
$H(p,q) = -\sum p \cdot \log(q)$

Note two things about the KL formula:
* We divide by p and take a log of q, so numerically neither p nor q cannot be 0. Even though p∙log(1/p)→0 for p→0
* It's **asymmetric**: P is the "target distribution" (baseline), and Q is the one you are trying to compare to P.
* That p∙log(1/p)→0 for p→0 means that ultimately KL doesn't care about unexpected things that happen (according to P), but does care about expected things not happening

Sometimes is called **relative entropy** of P in respect to Q. Answers the question of how much more you would have learned if you updated from Q to P (Bayesian). Or how many extra bits you would need (you would have to waste) to transmit a message from P using a code optimized for Q, as KL = D(P || Q) = H(P, Q) − H(P).

Proof:
$H(P) = Σ p \log(p) = Σ p \log (p q / q) = Σ p (\log(q) + \log(p/q)) = Σp \log(q) - Σ p \log (q/p) = H(P,Q) − D(P||Q)$

As a corollary, the entropy for a variable X following distribution P is lower than the max possible entropy (that for a uniform distribution) exactly by its KL divergence from a uniform distribution:
H(X) = log(N_events) - D( P || uniform_p_across_events ) .
For a uniform P(x), we have max entropy, so by updating from it to P, we demonstrate that we learned something, and entropy decreases.

It's interesting that if we think of it in terms of an **update**, it is P that is a posterior, or an update, while if we use it as a **loss function**, then P is the "ground truth", and Q is a distribution that is compared to it. So which one is known first, and which one second, is kind of opposite in these two scenarios. What is common in both cases, is that **P is a better known distribution**, and thus a "more true" one, that other distributions are compared against.

Note also that when used as a loss function, KL is the same as cross-entropy, as they differ by H(p), and in loss function scenarios, H(p) is usually constant, and so doesn't affect optimization. (See for example [[umap]])

KL divergence is also related to **mutual information**: I(X;Y) = KL( P(X,Y) || P(X) P(Y) ) . That is, how much extra information you have, if you know how events from X and Y match with each other, compared to if you only knew them separately).

# JS divergence

**Jensen-Shannon divergence**: a popular symmetric alternative to (or an extension of?) KL:

JSD(P||Q) = ( KL(P||M) + KL(Q||M) )/2
where M = (P+Q)/2
Always between 0 and 1.


# Refs

https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence

https://en.wikipedia.org/wiki/Jensen%E2%80%93Shannon_divergence

That for {p} = const, KL(p,q) and H(p,q) make equivalent loss functions.
https://stats.stackexchange.com/questions/357963/what-is-the-difference-cross-entropy-and-kl-divergence