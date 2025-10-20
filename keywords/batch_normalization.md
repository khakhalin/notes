# Batch normalization

Parents: [[dl]]
Related: [[regularization]], [[dropout]]

#dl


Batchnorm: normalizing inputs to each consecutive layer within a minibatch (set mean to 0, and sd to 1).

It makes training faster, and also acts as a regularizer, improving generalization.

So in practice for a minibatch, first you pass all x through layer 1, then calculate m and σ and normalize it, then calculate all layers 2 etc. The model is learning **2 more parameters per layer**: γ and β, such that x' = γx + β. Typically batchnorm is used on **inputs** to each layer (after weights, but before the activation function). In continuous learning one can also keep track of a running average of these 2 values. [[backprop]] through a batchnorm layer just uses recent mean and σ in its formulas (wikipedia has them; look simple but unpleasant).

**Lore:**
* Can tolerate higher learning rates
* Less sensitive to weight init ([[init_dl]])
* Supersedes [[dropout]] (shouldn't be used with it)

# Why does it work?

The original explanation for why it works was that it solves the problem of **covariate shift**, which is a fancy name for the fact that we optimize layer k given outputs of layer k-1, but we update weights of layer k-1 given error propagated based on the output of k: that is, both the learning signal and the context of learning always change, and optimization is chasing a moving target. But apparently this idea wasn't supported by experiments (see wikipedia).

Another explanation is that it **smoothes optimization function**. _(not very clear)_

> I mean, to a neuroscientist the very idea of batchnorm looks super-reasonable, but it is hard to explain what's the exact mathematical mechanism here.

# Refs

That batchnorm is about reducing covariate shift:
https://machinelearningmastery.com/batch-normalization-for-training-of-deep-neural-networks/

wiki
https://en.wikipedia.org/wiki/Batch_normalization