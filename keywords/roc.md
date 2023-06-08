# ROC and AUC

Parent: [[03_Classification]]
Related: [[confusion]], [[youdens-j]]

#classification


The certified best approach to model assessment and calibration : **AUC** = **Area under the ROC**, where ROC is the **Receiver Operating Characteristic** curve. 

Take a threshold-like parameter of the model, and slide it through all possible values from ideally permissive to fully prohibitive. Then we connect the dots and build the **ROC** in TPR/FPR coordinates (aka Sensitivity vs 1-Specificity). For max permissive we get maximal recall, or TPR=1, but also all negatives falsely reported as positive: TNR=0, or FPR=1 (that's how false-positive rate is defined, as 1-TNR). For max prohibitive, we get TPR=0, but also TNR=1, and so FPR=0. And then we look at the area under this curve to classify the performance of the model. For assigning classes with random probability, we get a y=x "curve", and so AUC=1/2; for perfect model knowledge we get AUC=1.

Interestingly, AUC = P(score of a true example > score of a wrong example). _I wonder if proving this is hard?_ #todo At least for two extreme cases this statement can be "felt" inuitively: for perfect classification (where score is sorted, with all class 0 points having lower scores that all class 1 points) AUC=1, as for very low thresholds only FPR changes (all TP are classified correctly, but not all TN), and then at some point it reverses (all TN are classified incorrectly, but not all TP), so AUC makes a sharp bend through the top left corner of the square. And for random scoring (where, for a large N, class 0 and class 1 are about uniformly distributed), AUC draws a diagonal, so the area is 1/2, corresponding to a case pf P(inversion = 0.5.