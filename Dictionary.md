Dictionary of Terms
===

**Bagging** (ML): the simplest way to create an ensemble of classifiers using an algorithm: split data into bags (with replacement), train on each of them. Final prediction = mean of all predictions. [ref](https://analyticsindiamag.com/primer-ensemble-learning-bagging-boosting/)

**Boosting** (ML): an alternative to _bagging_ for ensemble generation: Iteratively select samples that previous learners failed to learn or disagree upon, and use them to train new learners; majority vote at the end. [ref](https://analyticsindiamag.com/primer-ensemble-learning-bagging-boosting/)

**Drop-in replacement** (development): Replacing part of the code without rewriting anything else. Aka "bug for bug compatibility" (drop-in will only work if all idiosynctratic bugs match exactly).

**Dynamic programming** (programming): Recursion (or some other form of divide-and-conquer) + memoization (to never calculate the same sub-problem twice).

**Feature store** (data sci) A practical concept for data project implementation: a collection of curated features that are automatically produced (updated) from new data, and can be tapped into by various projects. Paying it forward with feature engineering.

# P
**Petri net** (math): a way to model discrete dynamic systems via a directed bipartite graph with 2 types of nodes: places and transitions. Tokens (agents?) can accumulate at places (states, circles), until a transition (bar) fires (consumes input tokens, and creates output tokens). Because of that, tokens inherently interact with each other. May be deterministic or not. [wiki](https://en.wikipedia.org/wiki/Petri_net)

**Pickling** (development): dumping binary data into a database, to be loaded later, instead of processing it in some meaningful (human readable) way. In Python, may be used for serializing Python object structure. A better alternative: JSON, which is human-readable, while pickles aren't.

**Propensity score matching** (stats, epidemiology) comparing output variable in a case of unavoidable confounding factors; go for something like range constriction for confounders by carefully matching them (many methods here), then analyzing this filtered set. The trick of this method in particular is that instead of matching in high-D, it uses an all-data linear model to conflate all confounding values into one "risk factor" (aka propensity score), and match based on it (both a strong, and a weak point obviously). [ref](https://en.wikipedia.org/wiki/Propensity_score_matching)

# S

**Singular Value Decomposition** (math): X = UDV, where U and V are unitary (orthonormal, may be complex), and D is diagonal with d_ii ∈ R, d_ii>0. Dimensions: mn = mm∙mn∙nn. More general than eigenvalue decomposition (applies to all matrices on C).

# T

**Tehnical debt** (development) picking an easy (quick to implement) but inherently limiting solution, even though statistically every limiting decision now may have to be reingeneered (scaled up) later.

**Zero-inflated models** (stats): models that are biased towards observations of zeros; for example a bimodal switch between active and inactive states, with inactive ==0, and active generating a Poisson process. [ref](https://en.wikipedia.org/wiki/Zero-inflated_model)