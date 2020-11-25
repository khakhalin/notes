# Random forest

#ensemble

Parent: [[05_Ensembles]]
Related: [[bagging]], [[boosting]]

The idea: construct many full trees, by bagging (generating partial data with subsetting, aka **sample with replacement**), but also by only considering a random subset of features at every split, aka **feature bagging**, aka **Random subspace method**. Typically, we use √p features out of p total, or something like max(5, p/3) (according to Wikipedia). This is an improvement upon bagging, as it makes trees less correlated, and way more diverse. In this approach, trees are always built to maximal depth, which makes every individual tree quite overfit, but it's OK, as there are many of them. All trees have an equal weight in the final classification.

Random forests are also great for ranging features in terms of their usefulness, as one can look into average performance of trees that use a certain feature (or use it at one of the early splits perhaps?), and compare them to the average performance of all trees in a forest (aka **Feature Importance**). Scikit-Learn, for example, computes these scores automatically.

# Varieties and extensions

**Extra Trees** or **Extremely Randomized Trees**: same as RF, but for each tree the first split is made at random (random feature, random point), then the rest of a tree is allowed to be optimized. Sometimes performs better than a standard RF, sometimes not.

**Unsupervised RF**: according to Wikipedia (3 references), one can generate fake data with roughly same statistics as the real data (aka reference distribution), then use RF to find real data in a cloud of fake data. Can help to find clusters, statistical anomalies etc. Can also be used to calculate **random forest dissimilarity** between two sets of data (??? #todo)

# Refs

* History of the method: https://en.wikipedia.org/wiki/Random_forest
* 