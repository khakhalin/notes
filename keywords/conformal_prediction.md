# Conformal prediction

Parent: [[stats]]
See also: confidence intervals, [[validation]], [[softmax]]


A way to somehow replace point-estimates with interval predictions, with a given alpha (a probaibility of true value lying outside of the interval?) For classification, estimation of a probability of misclassification. On top of training, testing, and validation datasets, now uses a new separate slice - **calibration dataset**.

https://en.wikipedia.org/wiki/Conformal_prediction