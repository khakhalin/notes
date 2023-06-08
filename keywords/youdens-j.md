# Youden's J-statistics

Parent: [[03_Classification]]
See also: [[confusion]], [[ROC]]

#stats #classification


J = sensitivity + specificity − 1  ; J ∈ 0 .. 1

$\displaystyle J = \frac{\text{true positive answers}}{\text{all positive cases}} + \frac{\text{true negative answers}}{\text{all negative cases}} -1 =  \frac{TP}{TP+FN} + \frac{TN}{TN+FP}-1$ 

Both fractions here are **recall**-style measures (not **precision**-style), in the sense that they give a performance across all tests of a certain kind, and NOT  the proportion of good responses among all responses. But unlike precision, recall, or their harmonic mean (**F-score**), this one cares not only about positive points being correctly identified as positive, but also about negative cases being correctly identified as negative.

Is related to the **Receiver Operating Characteristic** (ROC), as J value is the height of the ROC curve above the y=x line (corresponding to a random guess). This is easy to show: ROC shows sensitivity as a function of 1-specificity, which means that ROC – x = sensitivity + specificity – 1 = J.

# Refs

* https://en.wikipedia.org/wiki/Youden%27s_J_statistic