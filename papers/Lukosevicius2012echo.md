# Practical guide to echo-state networks

Lukoševičius, M. (2012). A practical guide to applying echo state networks. In_Neural networks: Tricks of the trade_(pp. 659-686). Springer, Berlin, Heidelberg.
http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.720.616&rep=rep1&type=pdf
500 citations.

Parents: [[echo]]

#echo


Starts with a long laud to reservoir computing, then a list of practical advice, by category.

Advocates for normalized root-mean-square error (NRMS) = RMS/sd(y) = $\text{mean} \sqrt{\frac{1}{T} \sum(\hat y - y)^2}/σ(y)$ as the best loss for RNNs and echo networks in particualar (easy to interpret; 1 is the baseline of $\hat y = const$). A neat summary of formulas.

### Reservoir

Reservoir as a kernel trick: it expands data, making different aspects of it linearly separable. Temporal memory is another useful metaphor. But these two aspects are in a contradiction, so there's a trade-off here.

Large reservoirs (in 2012, up to 1e4) are better, provided that there's a regularization for the signal fit. The lower bound is the number of points it should remember, as the longest chain would have N nodes. _Wait, is it even true? It sounds logical, but cannot one remember longer sequences with bistable cells, technically?_ Forgetting is not abrupt, but rather gradual, so smaller reservoirs can surprise you.

**Sparcity** of weight matrix W is not required, but is better. Also, if sparse matrix multiplication is available, it opens a door for larger, more computationally efficient reservoirs. Non-zero weights are typically either uniform from -1 to 1, or normal, or binary (in -1 and 1 set). Binary weights can make some neurons "identical", which lowers the capacity. But overall, in his opinion, neither of these parameters is really crucial.

What is crucial is the **spectral radius** (maximal absolute eigenvalue of W matrix: $ρ = \max_i λ_i$). Typically, they generate a random matrix, then scale it (divide it) by ρ (to get radius of 1), then multiply by some target value ~1. In theory, we need the signal to die out, to satisfy the **echo property**: that after you feed the signal to the reservoir long enough, it resets (defines) its state. The original state before "loading" the signal becomes irrelevant.

At the same time, actual activity of echos is not necessarily pre-defined by the eventual ρ. It's possible to get chaotic (generative, not-signal-driven) behaviors even for ρ<1, and the other way around, a robust signal drives the effective ρ of the system down, away from a linear case of ρ(W), as it saturates activation functions, thus limiting signal propagation. So for any given signal, optiomal ρ may very well be much greater 1.

> Didn't either [[Carroll2019structure]] or [[Carroll2020chaos]] say that they want ρ<1? Why this disagreement?

Plotting activation signals from the reservoir is the most human-efficient way to understand what's going on.

### Input weights and data normalization

Scaling and shifting of input data is useful. Sometimes people also squish inputs with tanh(), but I'm not sure what his take on it is. If data is mutidimensional, it may also be useful to run a PCA first, and feed first several PCA components, instead of the raw signals.

### Leaking rate (α)

The rule of thumb: make α match the typical speed of the signal, or you won't be able to fit the signal. But on the other hand, with low α, the echos even in a relatively small reservoir may be quite long.

### Readouts and "Training"

Use [[ridge_regression]]: $W_\text{out} = YX^⊤\cdot \text{inv}(XX^⊤ + βI)$. An alternative that gives a very similar effect, noisify x at training (they call it "noise immunization"). On small data, direct pseudoinverse (Moore-Penrose?) works best ([[pseudoinverse]]).

For classification, calculate total activation of every element, multiply by W out, do softmax. Which implies that at training, it's 1-hot encoding.

**Ensemble method** (training a lot of very small ESNs, then averaging their outputs) often works great.

The beginning of the data (used to load the network, until it forgets its internal state) needs to be discarded, which is of course a shame. There are some tricks to save this data though.

**Online training** is also possible, using gradient descent. They claim that instead of descending on every point, it's better to use **Recursive Least Squares**, where error is not instantaneous, but lingers for a while, decaying exponentially. It is more computationally demanding, and apparently not always numerically stable, but works faster. Some links on the topic are given. Footnote: https://en.wikipedia.org/wiki/Recursive_least_squares_filter

### Output Feedbacks

For pattern generation (prediction in the future) we typically feed model output back as an input. Training still typically happens as a feedforward prediction task (aka **teacher forcing**), and actual feedback isn't used. Training a one-step predictor. The problem of course is that even if qualitatively the prediction is good, small deviations in signal shape can make generation mode unusable.

Alternative - always train in generation mode, and hope that the model will stabilize itself. Some fancy-named algorithms (for example, **BackPropagation-DeCorrelation**, and **FORCE**).