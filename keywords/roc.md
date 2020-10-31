# ROC and AUC

#classification

Parent: [[03_Classification]]
Related: [[confusion]], [[youdens-j]]

The certified best approach to model assessment and calibration : **AUC** = **Area under the ROC (Receiver Operating Characteristic) curve**. 

Take a threshold-like parameter of the model, and slide it through all possible values from ideally permissive to fully prohibitive. Then we connect the dots and build the **ROC** in TPR/FPR coordinates (aka Sensitivity vs 1-Specificity). For max permissive we get maximal recall, or TPR=1, but also no true negatives at all: TNR=0 ⇒ FPR=1 (that's how false-positive rate is defined, as 1-TNR). For max prohibitive, we get TPR=0, but also FPR=0, as TNR=1. And we look at the area under this curve. For random probability, AUC=1/2, for perfect knowledge AUC=1.

Interestingly, AUC = P(score of a true example > score of a wrong example). _I wonder if proving this is hard?_ #todo But at least for two extreme cases this statement can be "felt" inuitively: for perfect classification (where score is sorted, with all class 0 points having lower scores that all class 1 points) AUC=1, as for very low thresholds only FPR changes (all TP are classified correctly, but not all TN), and then at some point it reverses (all TN are classified incorrectly, but not all TP), so AUC makes a sharp bend through the top left corner of the square. And for random scoring (where, for a large N, class 0 and class 1 are about uniformly distributed), AUC draws a diagonal.