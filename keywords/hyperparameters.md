# Hyperparameters

#dl

Parents: [[06_DL]]
Related: [[ml_lore]], [[optimizers]], [[regularization]]

* A list: 
    * number of training epochs, batch size, optimizer, 
    * regularization, dropout, weight decay
    * training set size, and the training / validation split
* Potentially (but it's a gray zone), also numerical (like, width, depth, convolution window size and stride) and optional (e.g. normalize or not?) aspects of network architecture.
* Also, aspects of pre-training and fine-tuning. How many layers to freeze?

# General ideas

Main **strategies** for optimization: manual, random, grid, Bayesian (or some other form of optimization). Essentially, like minimization on top of minimization. An curious idea is that what approach is better may depend on when (within your project) you're doing it: if you're doing it late, it may be better to use some out-of-the box Bayesian method, but if you're doing it early, it may be more beneficial to understand the minimization landscape.

Iterative Sweeps may be a reasonable strategy (Svetlichnaya 2020), where you go from more obvious parameters to less obvious ones, fixing all but one at each stage, and retaining the best configuration. A logical sequence: optimizer → order of magnitude for learning rate → layer sizes and convolution depth (aka filters) → dropout → all the fancy stuff.

# Learning rate

**Goldilocks principle** - the best learning rate should "magically" put you in the minimum in a very few steps. Large learning rate leads to noisy oscillations after what looked like a convergence. It may even break everything after convergence (unstable).

# Batch size

**Mini-batch**: process >1 (usually 10-1000) points at a time. Somewhere in between fully stochastic descent (1 point at a time) and classic regression (all points every time).

What is the **optimal batch size**? The downside of small batches (in the extreme case - fully stochastic descent) is that it's slow (as by definition it's not parallel). The benefit is that it makes a trajectory weirder that may help to avoid narrow minima (overfitting). _Is it true? Did I understand it right? I'm not sure._ On the other hand, with larger batch sizes, you can get a boost in speed. The net effect of batch size $b$ per computation is a sqrt(b) increase in time (hurt), as you do b times more calculations, but everything converges sqrt(b) times faster. But it can be parallelized, so for reasonably small b, it leads to a net improvement. And another effect, for high b descent is very smooth, which may make the model trapped at sharp minima. Seems to be related to the idea of "temperature" for optimization.

> If I got it right, it would mean that we should always try to use a small, but >1 batch size. Something like 16 or 32 maybe?

# Refs

[Alex Seewald at Quora](https://www.quora.com/Is-full-batch-gradient-descent-with-unlimited-computer-power-always-better-than-mini-batch-gradient-descent) - on whether full gradient is better than stochastic

Stacey Svetlichnaya, 2020, Hyperparameters search with iterative sweeps
https://medium.com/@staceysvetlichnaya/hyperparameter-search-with-iterative-sweeps-3799df1a4d45