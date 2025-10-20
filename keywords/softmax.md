# Softmax layer

#dl #classification

Parents: [[dl]], [[classification]]
See also: [[pooling]], [[loss]], [[hierarchical_softmax]]

Takes in an arbitrary vector, and transforms it so that all values are positive, between 0 and 1, and the sum is = 1 (and so outputs can be interpreted as probabilities). Formula: first exp(each input), then divide by the sum of all of these results. Preferred last layer in classification networks (in which case it's used with cross-entropy loss function). If placed in the middle it is equivalent to an exponential activation layer with batch normalization.

$\displaystyle σ(z)_i = \frac{e^{z_i}}{\sum_j e^{z_j}}$

For a case of a binary classifier, softmax becomes a sigmoid of difference of activations:

$\displaystyle σ\begin{pmatrix}a \\ b\end{pmatrix} = \begin{pmatrix}s \\ 1-s\end{pmatrix}$ where $\displaystyle s = \frac{e^a}{e^a + e^b} = \frac{1}{1+e^{b-a}}$ .

We can think of softmax as a smooth and differentiable version of argmax. Why exp() is a good choice here; why wouldn't we use some other function?
* They make everything positive, which is the first step towards outputting probabilities (normalization is the 2nd step).
* Exponents have pleasant derivatives (important for backprop)
* It is mathematically sound (in the same way in which **logistic regression** is mathematically sound), as it relies on the idea of entropy maximization and log-likelihood minimization. Assuing uniform distribution of labels, softmax **maximizes mutual information between X and Y** (Qin 2020). That's also the reason why softmax should be used with **log-loss** or **cross-entropy** loss.

When training classification problems with softmax layer at the end, it's better to balance labels: use all possible training points for the label that is trained, but only a random subsample for negative labels.

# Refs

https://deepai.org/machine-learning-glossary-and-terms/softmax-layer

https://datascience.stackexchange.com/questions/23159/in-softmax-classifier-why-use-exp-function-to-do-normalization

Qin, Z., & Kim, D. (2019). Rethinking Softmax with Cross-Entropy: Neural Network Classifier as Mutual Information Estimator. arXiv preprint arXiv:1911.10688.
https://arxiv.org/pdf/1911.10688.pdf