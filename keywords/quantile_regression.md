# Quantile Regression

Parents: [[stats]], [[loss]], [[02_Regression]]
See also:

 #loss #regression


Apparently, a regression that looks like L1, but not necessarily around the median. Instead of doing `abs(x-u)` for every point and then summing it, it multiplies all distances above u by $τ$, and all distances below u by $(1-τ)$ , which apparently makes u converge to $τ$-th quantile.

In synapseML [[LightGBM]], at least, instead of callint it `tau` they call it `alpha`, and it's another hyperparameters. For weirdly distributed positive-only data, the optimum for aggregate loss functions (e.g. [[mape]]) may lie at $τ \approx 0.6$ or so (above median).