# Random forest

#ensemble

Parent: [[05_Ensembles]]
Related: [[bagging]], [[boosting]]

The idea: construct many full trees, by bagging (partial data), but also by providing to every tree a random subset of features (aka **feature bagging**; typically √p features out of p total, or something like max(5, p/3), ref: [wiki](https://en.wikipedia.org/wiki/Random_forest#From_bagging_to_random_forests)). This is an improvement upon bagging, as it makes trees less correlated, more diverse. In this approach, trees are always built to maximal depth, which makes them quite overfitted, but it's OK, as there are many of them.

> Not sure if the sequence in which different values are used for splits is also randomized for each tree, or allowed to be optimal. I'd expect that both approaches could be possible, depending on dimensionality? Is it true?

All trees usually have an equal vote in the final classification.

Random forests are great for ranging features in terms of their usefulness, as one can see at the average performance of trees that include a certain feature, and compare it to the average performance of all trees in a forest (aka **Feature Importance**). Scikit-Learn, for example, computes these scores automatically.

A similar approach: **Extra Trees** of **Extremely Randomized Trees**, where for each tree the first split is made at random (random feature, random point), then the rest of a tree is allowed to be optimized. Sometimes performs better than a standard RF, sometimes not.