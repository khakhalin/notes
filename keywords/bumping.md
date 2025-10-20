# Bumping

#ensemble

Parent: [[ensembles]]
Related: [[random_forest]], [[bagging]]

A way to find better models by randomly moving in model space, avoiding local minima. Train a bunch of models on different subsamples of X, then test each on full X, and pick the best. As in this case optimization is kinda shifted towards the end, it's probably better to use quite undersampled training sets. Say, for a **XOR** case, if you test enough small subsamples, at least one of them will probably split the data decently, even though training on a full dataset may be hopeless for many types of models.

It's important to keep all models in  the comparison group similar in their complexity (for trees, same number of terminal nodes).