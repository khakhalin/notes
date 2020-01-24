Dictionary of Terms
===
Not exhaustive: if a term is explained in one of the thematic chapters, it is not duplicated here. This list is only for "orphan" terms that are too short or off-topic to get their personal entry elsewhere.

See also: [[tool_list]] - for all named products.

# AB

**Bagging** (ml): the simplest way to create an ensemble of classifiers using an algorithm: split data into bags (with replacement), train a different model on each of them. Final prediction = mean of all predictions. [ref](https://analyticsindiamag.com/primer-ensemble-learning-bagging-boosting/). See [[05_Ensembles]].

**BIC** (stats): Bayes Information Criterion, an approach to model selection. To be described (for now, [wiki](https://en.wikipedia.org/wiki/Bayesian_information_criterion)).

**Boosting** (ml): an alternative to _bagging_ for ensemble generation: Iteratively select samples that previous learners failed to learn or disagreed upon, and use them to train new learners; majority vote at the end. [ref](https://analyticsindiamag.com/primer-ensemble-learning-bagging-boosting/). See [[05_Ensembles]].

# C

**CART** (ml): Classification And Regression Tree; often used for gradient boosting. An umbrella term to cover decision trees (classifiers), but also trees that have real numbers assigned to their leaves as outputs. See [[05_Ensembles]].

**Clique** (math): an all-to-all subgraph within a graph. Two confusing names: **Maximal clique** is just a clique that is not a part of a larger clique (you cannot include one more vertex or ege from a graph, and still have a clique). **Maximum clique** is the largest clique in a graph. Apparently finding cliques, as well as enumerating cliques, is an NP problem with no good solution. [wiki](https://en.wikipedia.org/wiki/Clique_problem), [current best algorithm (hard)](https://www.sciencedirect.com/science/article/abs/pii/S0305054810001504)

**Collaborative filtering** (business): a name for what Netflix users do when they watch movies and generate data of "co-occurrence". Probably a pre-DL synonim for Federated Learning?

# D

**DAG** (math): Directed Acyclic Graph

**Design Patterns** (programming): A famous programming paradigm, and a name of a book by Peter Norwig, about typical ways to engineer interactions between classes and objects in object-oriented programming. About 23 or so archetypical solutions and interfaces that Java/C++ programmers learn by heart. [wiki](https://en.wikipedia.org/wiki/Software_design_pattern)

**Document store** (database) - instead of rows, returns XML documents or something similar. [wiki](https://en.wikipedia.org/wiki/Document-oriented_database)

**Drop-in replacement** (development): Replacing part of the code without rewriting anything else. Aka "bug for bug compatibility" (drop-in will only work if all idiosynctratic bugs match exactly).

**Dynamic programming** (programming): Recursion (or some other form of divide-and-conquer) + memoization (to never calculate the same sub-problem twice).

# EF

**Feature store** (data sci) A practical concept for data project implementation: a collection of curated features that are automatically produced (updated) from new data, and can be tapped into by various projects. Paying it forward with feature engineering.

# G

**GAM** (stats): Generalized Additive Models: f = sum of smooth functions. Often GLM + some smoothing splines.

**GBM** (ml): Gradient Boosting Machine. See [[05_Ensembles]]

**GOFAI** (ai): Good Old Fashioned Artificial Intelligence (aka Symbolic Intelligence). Dominant until 1980. Plugging inputs into a logic scheme (at most - fuzzy logic).

# HIJ

**Heap** (programming): a tree-like structure where children < parents. An optimal realizatin of a priority queue (similar to a stack, but with a build-in sorting according to priority).

**Indicator matrix** (data): A matrix Y, in which each row one-hot encodes the class g of a data point x. See [[03_Classification]].

**JVM** (dev): Java Virtual Machine.

# KLM

**loess** (stats): Locally Estimated Scatterplot Smoothing, aka Savitzky-Golay filter. See [[smoothing]]

**MapReduce** (dev): programming model for parallel computing; a type of split-apply-combine strategy. Essentially, seems to define two functions, one function on splittable tasks (something like `for each a in List`) that analyzes its `a` and sends answers out; and another one that catches answers and combines them all into one answer.

**MCMC** (ml) Markov Chain Monte-Carlo. See [[13_MC]].

# N

**noSQL** - a general word for non-relational [[database]]s, not tabular-based, good for realtime big data. May work with documents, key-values, wide-columns, graphs. Have lower guarantees on consistency (stale reads: updates don't update immediately), which can lead to data loss. [wiki](https://en.wikipedia.org/wiki/NoSQL)

# OPQR
**Petri net** (math): a way to model discrete dynamic systems via a directed bipartite graph with 2 types of nodes: places and transitions. Tokens (agents?) can accumulate at places (states, circles), until a transition (bar) fires (consumes input tokens, and creates output tokens). Because of that, tokens inherently interact with each other. May be deterministic or not. [wiki](https://en.wikipedia.org/wiki/Petri_net)

**Pickling** (development): dumping binary data into a database, to be loaded later, instead of processing it in some meaningful (human readable) way. In Python, may be used for serializing Python object structure. A better alternative: JSON, which is human-readable, while pickles aren't.

**POC** (development): Proof of Concept.

**Propensity score matching** (stats, epidemiology) comparing output variable in a case of unavoidable confounding factors; go for something like range constriction for confounders by carefully matching them (many methods here), then analyzing this filtered set. The trick of this method in particular is that instead of matching in high-D, it uses an all-data linear model to conflate all confounding values into one "risk factor" (aka propensity score), and match based on it (both a strong, and a weak point obviously). [ref](https://en.wikipedia.org/wiki/Propensity_score_matching)

**QR Factorizatoin** (math): A = QR where Q is orthonormal, and R is upper triangular. Achieved via Gram-Schmidt algorithm (aka elimination). See [[la_01]].

# S

**Singular Value Decomposition** (math): A = UDV, where U and V are unitary (orthonormal, may be complex), and D is diagonal with d_ii ∈ R, d_ii>0. Dimensions: mn = mm∙mn∙nn. More general than spectral decomposition, as applies to all matrices on C, unlike QDQᵀ that only works for symmetic, or a slightly more general PDP⁻¹ for diagonalizable (sum dim eigenspaces = dim A).

**SMOTE** (stats): Synthetic Minority Oversampling Technique. A simple approach to oversampling (supplementing) under-represented categories by creating fake points (in pD of variables describing each case), that are linearlly shifted from under-represented points to one of its closest neighbors. [wiki](https://en.wikipedia.org/wiki/Oversampling_and_undersampling_in_data_analysis) 

# T

**Tehnical debt** (development) picking an easy (quick to implement) but inherently limiting solution, even though statistically every limiting decision now may have to be reingeneered (scaled up) later.

**Zero-inflated models** (stats): models that are biased towards observations of zeros; for example a bimodal switch between active and inactive states, with inactive ==0, and active generating a Poisson process. [ref](https://en.wikipedia.org/wiki/Zero-inflated_model)

# UVW

**Wide column store** (databases): Imagine a table, except that different rows may have different columns (number, names, types). 

# XYZ