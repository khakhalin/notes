# Gini impurity

#classification

See also: [[03_Classification]]

Has nothing to do with Gini coefficient from economics, except that both were introduced by the same dude Corrado Gini ([ref](https://jamesmccaffrey.wordpress.com/2018/09/06/calculating-gini-impurity-example/)). **Gini impurity** is good for quantifying the quality of classification by a decision tree (not necessarily with 2 categories). Lies betweein 0 (a limit case for one category overpowering all) and 1 (a limit case for inifinitely many equally represented categories).

Quantifies the **probability of mislabeleing an element if we pick it at random**, and then **label it at random**, but with probabilities of different labels matching observed probabilities in the distribution.

Say we have N labels, and the frequencies of these labels are p_i. Then, assuming that these probabilities are true probabilities, if we pick an element at random, we will pick an element of true label i with probability p_i = n_i/N (n_i is the number of elements in class i, and N=∑n_i is the total number of points in the set). If we now label this element at random, we will label it incorrectly with a probability of (1-p_i). It means that the total probability of picking an element at random and mislabeling it is equal to I = ∑pi(1-pi) = ∑p - ∑p² = 1-∑p² . 

In a binary case, 1-p²-(1-p)² = 2(p-p²), where p=Ncases/Npoints (for either class 0 or 1, as the formula is obviously symmetric). When picking the **best split** for a **decision tree**, for every point (as every point can potentially become a split point), calculate gini impurity for the left part (g1), and for the right part (g2). The total impurity for the split is the weighted average of these two impurities, weighted by the number of elements in the left and right parts: g = (n1∙g1 + n2∙g2)/(n1+n2). Decision split finds the split point that achieves lowest total impurity. ([ref](https://towardsdatascience.com/the-simple-math-behind-3-decision-tree-splitting-criterions-85d4de2a75fe))

It seems that for **weighted Gini index** the formula is the same, just pi for each class is no longer just ni/N where N=∑ni, but rather the sum of weights within class, divided byt the total sum of weights: $p_i = ∑_j w_j δ_{ij} / ∑_j w_j$.

# References
* [wiki](https://en.wikipedia.org/wiki/Decision_tree_learning#Gini_impurity)
* Example calculation: https://jamesmccaffrey.wordpress.com/2018/09/06/calculating-gini-impurity-example/
* Comparison with entropy: https://towardsdatascience.com/gini-index-vs-information-entropy-7a7e4fed3fcb