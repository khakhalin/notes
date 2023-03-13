# Ensemble methods

#ensemble

The concept of **ensembles** is about combining (or gradually refining) many poor predictions (aka **weak learners**) into a very good predictor.

Subtopics:
* [[bagging]] - bootstrap data repeatedly, many models, average their predictions
* [[stacking]] - many predictors (bagging + diff hyperparameters), then optimal linear combination of models
* [[bumping]] - randomly move in the latent space, to avoid local minima, then simply pick the best model
* [[boosting]] -  iteratively pick best single-var split (decision stump), boost errors, build a weighted model
* [[gbm]] - gradient boosting machines; similar to boosting, but for non-categorical data
* [[random_forest]] - bagging + features_bagging (random subset of coordinates), average all

See also:
* [[dropout]] - similar idea for DL

# Refs

"Wisdom of Crowds" concept, popularized by a 2004 book by Surowiecki:
https://en.wikipedia.org/wiki/The_Wisdom_of_Crowds

What's better, Gradient Boosted Trees, or Random Forests?
http://fastml.com/what-is-better-gradient-boosted-trees-or-random-forest/
Apparently they are so close in performance that it's hard to tell which one is better. Usually boils down to parameter choice and tuning. But maybe in some cases GBTs are just a tiny little bit better.

Comparing bagging and boosting:
https://quantdare.com/what-is-the-difference-between-bagging-and-boosting/
https://towardsdatascience.com/understanding-adaboost-2f94f22d5bfe