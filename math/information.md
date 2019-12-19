# Information and entropy

## Entropy

For an event x with probability p(x), the amount of information obtained if we learned that the event happened is -lg(p(x)). Nice property, because if two independent events happen together, probabilities multiply, and so informations add. lg() is a base-2 logarithm.

Entropy of a random variable = expectation of entropy of a message (expectations of entropy if you observe a value of this variable), so = -∑p(x)∙lg(p(x)). Note how it makes some variables more informative (surprising, unexpected) than others: if it's highly skewed (in an extreme case: almost always one value), entropy is small (for a constant, it's 0). That's intuitive, and mathematically that's because x log(x) → 0 when x→0. (The way it is proven, BTW, is by replacing x with "divide by 1/x", then taking a derivative from both numerator and denominator that is called "L'Hopital's Rule")

References:
* Jason Brownlee about [entropy](https://machinelearningmastery.com/what-is-information-entropy/), [cross-entropy](https://machinelearningmastery.com/cross-entropy-for-machine-learning/) (confusing)

## Cross-entropy

Definition: H(p,q) = -∑P(x)∙lg(Q(x)), or integral for continuous.

Is used a loss for classification tasks (see respective chapters: classification and DL).

Not to be confused with joint entropy, even tho people use same notation H(p,q). Joint entropy is just an entropy of a vector variable composed of two variables that are "joined"; nothing fancy.

## KL divergence

Kullback–Leibler Divergence between 2 probability distributions P and Q: 
D(P||Q) = -∑p∙lg(q/p)
(they use this funny symbol || everywhere)

Note that:
* division by p, so p cannot be 0 anywhere
* Asymmetric. P is the "target distribution", and Q is the one you compare to it.

Sometimes is called _relative entropy_ of P in respect to Q. Like, how much more you learned if you updated from Q to P (Bayesian), or how many bits you need to transmit a message from P using a code optimized for Q.

From this point of view, entropy for a variable X following distribution P:
H(X) = log(N_events) - D_KL( P || uniform_across_events )
Because for uniform P(x) for every x we have max entropy, and we updated from it to P, entropy decreased. Like, we learned something ([ref: wikipedia](https://en.wikipedia.org/wiki/Kullback%E2%80%93Leibler_divergence)).

> Interesting that in terms of update, it is P that is a posterior, or an update, while if you use it as a loss, P is the "ground truth", and Q is compared to it. So which one is known first, and which one second, is kind of opposite in these two scenarios. What is common, is that P is a better known, "more true" distribution.

KL divergence is related to mutual information: I(X;Y) = D_KL( P(X,Y) || P(X) P(Y) ) . That is, how much extra information you have, if you know how events from X and Y match each other, compared to if you only know them separately).

Cross-entropy of two random variables: H(p,q) = H(p) + D_KL(P || Q)

## JS divergence

Most commonly used symmetric alternative to LK ([wiki](https://en.wikipedia.org/wiki/Jensen%E2%80%93Shannon_divergence)). 
JSD(P||Q) = ( LK(P||M) + LK(Q||M) )/2
where M = (P+Q)/2
Always between 0 and 1.

## TFIDF
https://en.wikipedia.org/wiki/Tf%E2%80%93idf
TF-IDF = Text frequency - Inverse document frequency. A measure of how often a word is used in a document, compared to all other documents. tfidf() = tf()∙idf(). 

Here tf() = text frequency, which is usually this word frequency (% of all words), or log() of it, or normalized frequency, or just raw count, or maybe even binary (yes or now)

idf() = inverse document frequency = the share of documents having this word, but taken logirithmically: idf() = log(N/(1+n)) + 1, where N is the total number of documents, and n is the number of documents containing this word.

Can be linked (and justified through) conditional entropy.