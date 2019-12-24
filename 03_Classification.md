# Classification

## Basic classification problem
Simplest approach: **Nearest Neighbor**. Just pick closest training case. This is an example of a **lazy learning** approach: instead of generating estimations upfront (that would be called **eager**), we only generate them at retrieval. A slightly better approach: **K nearest neighbors** (aka KNN).

Classic example: [MNIST](http://yann.lecun.com/exdb/mnist/index.html) (link to a library of all thinkable and unthinkable approaches to classification, as applied to this dataset, each given with a test error rate)

## Model assessement

**Model accuracy**: The most primitive measure = number of true statements / total number of statements. Obvious dependency on class balance (famous example, a statement of "today is not Christmas" has an accuracy of 99.7%). But may work for toy examples with very carefully balanced classes (like MNIST).

**Precision and recall**: Let's concentrate on detection only (positive predictions only). We want something that is proportinal to true positives (TP), but what to put in the denominator? Two options: 

* TP/(all positive predictions) = TP/(TP+FP) - how well can we believe positive predictions. Is called **Prediction**, and is related to false discovery rate (P=1-FDR). From the stats POV, it's about false-positives, or Type I.
* TP/(all actual positives) = TP/(TP+FN) - how well does the model actually detect cases, and how sure can we be that most positive cases were detected, that not much was left behind. **Recall**. From the stats POV, it's about false-negatives, or Type II.

Can we balance them somehow, to make sure the model is reasonably good on both accounts? Most popular approach: **F1 score**: F = 2∙(precision∙recall)/(precision+recall) = harmonic_mean(precision,recall) = inv(mean(inv(precision),inv(recall))).

An even better approach: **AUC** = **Area under the ROC (Receiver Operating Characteristic) curve**. With this approach, you take a threshold-like parameter of the model, and slide it through all possible values from ideally permissive to fully prohibitive. For max permissive we get maximal recall, or TPR=1, but also no true negatives at all: TNR=0 ⇒ FP=1 (that's how false-positives are defined, as 1-TNR). For max prohibitive, we get TPR=0, but also FPR=0, as TNR=1. For random probability, AUC=1/2, for perfect knowledge AUC=1.

Interestingly, AUC = P(score of a true example > score of a wrong example). _I wonder if proving this is hard?_

**Prediction bias**: a very simple measure of model validity: average value of all predictions - average value of all learning points. In a reasonable model, prediction bias should be close to 0. A way to assess it: build a **calibration plot** - bucket values, calculate predictions, then plot mean(predictions) against mean(values). May help to find areas where the model misbehaves.

## SVM

Widest street approach: find a line, so that if you have a band around the line, it separates the positive from the negative examples as best as possible.

How to use the line? Describe it with a vector ω ⊥ line. Now how far a point $u$ is in the line can be described as uω. Let's say that $b$ is a good projection value ($u$ lies right smack on the central line), so we have $u\cdot\omega + b \geq 0$.

To find the best line, let's insist that for samples we don't just get above 0, but get $x\omega+b$ above 1 for positive samples, and -1 of negative samples. 