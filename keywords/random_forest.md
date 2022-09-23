# Random forest

#ensemble

Parent: [[05_Ensembles]]
Related: [[bagging]], [[boosting]]

The idea: construct many alterantive full trees, by 
1) **data bagging** - generating partial data with subsetting, aka "sampling with replacement"), and also 
2) **feature bagging** - by only considering a random subset of features at every split (aka Random subspace method), to force a creation of many very ddifferent trees. Typically, we use √p features out of p total, or something like max(5, p/3) (according to Wikipedia). 

This mixed approach is an improvement upon pure bagging, as it makes trees less correlated, and way more diverse. Trees are always built to maximal depth, which makes every individual tree quite overfit, but it's OK, as there are a lot of them. All trees have an equal weight in the final classification.

Random forests are also great for ranging features in terms of their usefulness (aka **Feature Importance**), as one can look into average performance of trees that use a certain feature (or use it at one of the early splits perhaps?), and compare them to the average performance of all trees in a forest. Scikit-Learn, for example, computes these scores automatically.

# Varieties and extensions

**Extra Trees** or **Extremely Randomized Trees**: same as RF, but for each tree the first split is made at random (random feature, random point), then the rest of a tree is allowed to be optimized. Sometimes performs better than a standard RF, sometimes not.

**Unsupervised RF**: according to Wikipedia (3 references), one can generate fake data with roughly same statistics as the real data (aka reference distribution), then use RF to find real data in a cloud of fake data. Can help to find clusters, statistical anomalies etc. Can also be used to calculate **random forest dissimilarity** between two sets of data (??? #todo)

# Refs

* https://en.wikipedia.org/wiki/Random_forest