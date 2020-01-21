# Gini impurity

#classification

Has nothing to do with Gini coefficient from economics, except that it was introduced by the same mathematician Corrado Gini ([ref](https://jamesmccaffrey.wordpress.com/2018/09/06/calculating-gini-impurity-example/)). Good for quantifying the quality of classification by a decision tree (not necessarily with 2 categories). Lies betweein 0 (limit case for one category overpowering all) and 1 (limit case for inifinitely many equally represented categories).

Quantifies the probability of mislabeleing an element if we pick it at random, and then label it at random, but with probabilities of different labels matching observed probabilities from a distribution.

Say we have N labels, and the frequencies of these labels are p_i. Then, assuming that these probabilities are true probabilities, if we pick an element at random, we will pick an element of true label i with probability p_i. If then we label it at random, we will label it incorrectly with a probability of (1-p_i). 

The total probability of picking an element at random and mislabeling it is thus equal to I = ∑pi(1-pi) = ∑pi - ∑pi² = 1-∑(pi)² .

# References
* [wiki](https://en.wikipedia.org/wiki/Decision_tree_learning#Gini_impurity)
* [Example calculation](https://jamesmccaffrey.wordpress.com/2018/09/06/calculating-gini-impurity-example/)