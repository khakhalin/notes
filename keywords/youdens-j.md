# Youden's J-statistics

#stats #classification

Parent: [[03_Classification]]
See also: F-score, ROC, confusion matrix

J = sensitivity + specificity − 1  ; J ∈ 0 .. 1

$J = \frac{\text{true positive answers}}{\text{all positive cases}} + \frac{\text{true negative answers}}{\text{all negative cases}} -1 =  \frac{TP}{TP+FN} + \frac{TN}{TN+FP}-1$ 

Both fractions here are **recall**-style measures (not **precision**-style), in the sense that they give a performance across all tests of a certain kind, and NOT  the proportion of good responses among all responses. But unlike precision, recall, or their harmonic mean (**F-score**), this one cases not only about positive answers and cases, but also about negative cases being reported as negative answers.

Notably, as it's a recall-style-measure, J doesn't change if the share of positive and negative cases in the testing set changes. It shows the quality of the model, not the quality of its performance on a real set.

Is related to the **Receiver Operating Characteristic** (ROC), as J value is the height of the ROC curve above the y=x curve (which corresponds to a random guess). This is by definition: ROC shows sensitivity as a function of 1-specificity, so y-x = sensitivity+specificity-1 = J.

# Refs

* https://en.wikipedia.org/wiki/Youden%27s_J_statistic