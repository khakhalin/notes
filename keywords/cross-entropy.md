# Cross-entropy

Parents: [[information]], [[loss]], [[classification]]
Related: [[logreg]], [[gini]], [[kl]]

#entropy


**Cross-Entropy Loss** is the most useful loss for classification tasks.

**Definition**: cross-entropy of a distribution q relative to distribution p,
$H(p,q) = E_p(\log q) = -∑ p_j \cdot \log(q_j)$, 
where p_j is a probability of an event j (class, value) in the observed set (our best available evidence about the "true distribution"), and q_i are probabilities predicted by the model (those we want to compare to the "true distribution").

**Information Theory interpretation:** the expected message length when transmitting data from a (wrong) distribution P, using a code optimized for Q. So, Q is assumed, but P is happening.

**Motivation:** essentially, it's an total log-likelihood (the log probability of observing the dataset, assuming the predictions of the model). Let the predictions of the model follow a distribution q_j, while the empirical probabilities are p_j. Then the probability of observing the dataset (assuming that the observations are independent) is P = ∏P(each observation) = $∏q_i ^ {n_i}$. If we log it, to replace products with sums, we get the total log-likelihood: $L = ∑ n_i \log(q_i)$. If we average it by dividing by the total number of test cases, and denote $p_i = n_i / N$ (actual empirical frequencies), we get $L = ∑ p_i \log(q_i) =-H(p,q)$. Now we can try to maximize this value (or rather, minimize the negative of this value), by making the model make predictions q_j as close to p_j as possible.

> Is cross-entropy always the theoretically-best loss function for classification tasks, or can custom losses be better? #todo

> What's the deal with this assumption of independence? What if our measurements are not independent; for example, when we are working with a time-series? What do people do then? #todo

For practical calculations, assume that the **ground-truth is a one-hot encoded vector** (for each point i, we have 1 only for the class j that it belongs to), while the prediction q_i is a vector of probabilities, for this point i to be assigned to each class j . (Not the final decision itself, mind it, or we'd had to resort to [[01loss]], which is sad).

Not to be confused with **joint entropy**! Even tho some people use similar or same notation for it H(p,q). Joint entropy is just an entropy of a vector variable composed of two variables that are "joined"; nothing fancy.

# Refs

Where it comes from, for log-regression:
https://www.expunctis.com/2019/01/27/Loss-functions.html

https://en.wikipedia.org/wiki/Cross_entropy#Motivation

Quite confusion explanation: https://machinelearningmastery.com/cross-entropy-for-machine-learning/