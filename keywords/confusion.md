# Confusion Matrix and model assessment

#classification

Parents: [[03_Classification]]
Related: [[youdens-j]], [[roc]], [[validation]]

**Model accuracy**: The most primitive measure = number of true statements / total number of statements. Obvious dependency on class balance (famous example, a statement of "today is not Christmas" has an accuracy of 99.7%). But may work for toy examples with carefully balanced classes (like [[mnist]] for example).

**Precision and recall**: Let's concentrate on detection only (positive predictions only). We want something that is proportinal to true positives (TP), but what to put in the denominator? Two options: 

* **Precision**: TP/(all positive answers) = TP/(TP+FP) : how well can we believe positive predictions. Is related to false discovery rate (P=1-FDR). From the stats POV, it's about false-positives, or Type I. The metaphor for the name: imprecise estimates are hard to believe.
* **Recall**: TP/(all positive cases) = TP/(TP+FN) : how well does the model actually detect cases, and how sure can we be that most positive cases were detected, that not much was left behind. From the stats POV, it's about false-negatives, or Type II. THe metaphor for the name: you show actual positives; can the model recognize ("recall") them?

Can we balance them somehow, to make sure the model is reasonably good on both accounts? Most popular approach: **F1 score**, or the harmonic mean of precision and recall: 

$\displaystyle F = 2\frac{precision \cdot recall}{precision+recall} = (\text{mean}(precision^{-1},recall^{-1}))^{-1}$

Note that while recall is strictly a measure of the model (class imbalance doesn't affect it), precision is not (it goes down as the share of TP decreases, as most positives become FP). Which means that F1 score also is not, strictly speaking, a measure of the quality of the model, but rather, a score for the performance of a given model on a given dataset.

* An even better approach: [[ROC]] (and related measure of AUC)
* Another option: [[youdens-j]] (also includes )

# Confusion matrix

**Confusion matrix** is a good diagnostic tool for multiclass classification. Take all true classes, and tally into which classes they are misclassified. Identify the issues. _Is it better to do it on the validation set?_ It's better to zero the diagonal, as most cases will be classified correctly, but we're interested in the errors!

And the thing _is_ really confusing. Note for example that FPR and TPR are not at all the opposites of each other; they have different everything! TPR = 1-FNR = TP/(TP + FN), while FPR = 1-TNR = FP/(FP+TN). Don't get confused here. Note that the denominator is always the numerator + its opposite, and if you use some common sense, you can always "reconstruct" what the negative is, as with incorrect classification both the label and the truthfulness of this label are negated.