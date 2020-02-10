Dictionary of Terms
===
Not exhaustive: if a term is explained in one of the thematic chapters, it is not duplicated here. This list is only for "orphan" terms that are yet too short or off-topic to get a personal entry elsewhere.

# AB

**ACID** (db): standard requirements for relational databases that maximize their reliability: Atomic (each transaction either fails as a whole, or succeeds as a whole; you cannot half-update a table), Consistent (invariants are checked; database always remains valid), Isolated (if transactions are executed concurrently, the result is the same as if they were ran sequentially), Durable (completed transactions are recorded in non-volatile memory: important for power outages).

**Bagging** (ml): the simplest way to create an ensemble of classifiers using an algorithm: split data into bags (with replacement), train a different model on each of them. Final prediction = mean of all predictions. [ref](https://analyticsindiamag.com/primer-ensemble-learning-bagging-boosting/). See [[05_Ensembles]].

**BIC** (stats): Bayes Information Criterion, an approach to model selection. To be described (for now, [wiki](https://en.wikipedia.org/wiki/Bayesian_information_criterion)).

**Boosting** (ml): an alternative to _bagging_ for ensemble generation: Iteratively select samples that previous learners failed to learn or disagreed upon, and use them to train new learners; majority vote at the end. [ref](https://analyticsindiamag.com/primer-ensemble-learning-bagging-boosting/). See [[05_Ensembles]].

# C

**CART** (ml): Classification And Regression Tree; often used for gradient boosting. An umbrella term to cover decision trees (classifiers), but also trees that have real numbers assigned to their leaves as outputs. See [[05_Ensembles]].

**Clique** (math): an all-to-all subgraph within a graph. Two confusing names: **Maximal clique** is just a clique that is not a part of a larger clique (you cannot include one more vertex or ege from a graph, and still have a clique). **Maximum clique** is the largest clique in a graph. Apparently finding cliques, as well as enumerating cliques, is an NP problem with no good solution. [wiki](https://en.wikipedia.org/wiki/Clique_problem), [current best algorithm (hard)](https://www.sciencedirect.com/science/article/abs/pii/S0305054810001504)

**Collaborative filtering** (pipe): a name for what Netflix users do when they watch movies and generate data of "co-occurrence". Probably a pre-DL synonim for Federated Learning?

# D

**DAG** (math): Directed Acyclic Graph

**Databricks Workspace** (db): workspace (environment) bhy Databricks, to interact with programs (jobs), notebooks, libraries, data. (?)

**Dataiku**: some sort of visual programming language platform for data science?

**Design Patterns** (prog): A famous programming paradigm, and a name of a book by Peter Norwig, about typical ways to engineer interactions between classes and objects in object-oriented programming. About 23 or so archetypical solutions and interfaces that Java/C++ programmers learn by heart. [wiki](https://en.wikipedia.org/wiki/Software_design_pattern)

**Docker** (db): Cloud computing service that provides a pratform with OS-level virtualization: an isolated user space (called container) that looks like a dedicated computer to programs running on it. Containers can talk to each other via defined channels. But unlike virtual machines, there's only one OS, so is less resource-hungry.

**Document store** (db): instead of rows, returns XML documents or something similar. [wiki](https://en.wikipedia.org/wiki/Document-oriented_database)

**Drop-in replacement** (prog): Replacing part of the code without rewriting anything else. Aka "bug for bug compatibility" (drop-in will only work if all idiosynctratic bugs match exactly).

**Dynamic programming** (prog): Recursion (or some other form of divide-and-conquer) + memoization (to never calculate the same sub-problem twice).

# EF

**ECS** (dev): Amazon Elastic Container Server. Is used to manage (run, stop) Docker containers on a cluster.

**EDA** (): Exploratory Data Analysis.

**Feature store** (data) A practical concept for data project implementation: a collection of curated features that are automatically produced (updated) from new data, and can be tapped into by various projects. Paying it forward with feature engineering.

# G

**GAM** (stats): Generalized Additive Models: f = sum of smooth functions. Often GLM + some smoothing splines.

**GBM** (ml): Gradient Boosting Machine. See [[05_Ensembles]]

**GCP** (db): Google Cloud Platform.

**GOFAI** (ai): Good Old Fashioned Artificial Intelligence (aka Symbolic Intelligence). Dominant until 1980. Plugging inputs into a logic scheme (at most - fuzzy logic).

**Golang** aka **Go** (prog): statically typed compiled language by Google, similar to C, but with memory safety, garbage collection, concurrency support, and structured type system.	

# HIJ

**Hadoop** (db): collection of open-source software utils for cluster support, maintained by Apache. Apparently, an alternative to data warehouses and MPP (why?).

**HCI**: Human-Computer Interaction. An academic area of research.

**Heap** (prog): a tree-like structure where children < parents. An optimal realization of a [[priority_queue]] (similar to a stack, but with built-in sorting by priority).

**Indicator matrix** (data): A matrix Y, in which each row one-hot encodes the class g of a data point x. See [[03_Classification]].

**JVM** (dev): Java Virtual Machine.

# KL

**Kafka** (db): open-source stream-processing plantform developed by Linkedin, written in Scala, and donated to Apache. High-throughput low-latency platform to handle realtime data feeds.

**Kubernetes** (db): Open-source system for deploying applications on a cloud. Originally developed by Google. Operates **pods** that each contains several **containers**, to be placed together, within the same localhost. Pods can communicate with each other via pod IP.

**LDA** 
1. (text): Latent Dirichlet Allocation (see [[10_Text]])
2. (ml): Linear Discriminant Analysis (see [[04_Features]])

**loess** (stats): Locally Estimated Scatterplot Smoothing, aka Savitzky-Golay filter. See [[smoothing]]

# M

**MAE** (ml): Mean Average Error, aka L1 loss.

**MapReduce** (dev): programming model for parallel computing; a type of split-apply-combine strategy. Essentially, seems to define two functions, one function on splittable tasks (something like `for each a in List`) that analyzes its `a` and sends answers out; and another one that catches answers and combines them all into one answer.

**MCMC** (ml) Markov Chain Monte-Carlo. See [[13_MC]].

**MPP** (dev): Massively Parallel Processing: a data architecture for big data. Examples: Amazon Redshift, Microsoft Azure, Snowflake, Google BigQuery. "Share nothing" architecture: sets of data don't overlap. Easier to deploy; support SQL. Alternative to Hadoop.

**MST** (algos): Minimum Spanning Tree. A cycle-less subgraph of a weighted graph with min ∑w_ij. See [[graph_algos]].

# N

**NMF** (text): Non-negative Matrix Factorization, V=WH + U, where W and H are non-negative, and also typically V is alot×alot, but W is alot×little, and H is little×alot (similar to other dimensionality reduction techniques). Often used with text analysis.

**noSQL** (db): a general word for non-relational [[database]]s: not tabular-based, good for realtime big data. May work with documents, key-values, wide-columns, graphs. Have lower guarantees on consistency (stale reads: updates don't update immediately), which can lead to data loss. [wiki](https://en.wikipedia.org/wiki/NoSQL)

# OPQ

**Petri net** (math): a way to model discrete dynamic systems via a directed bipartite graph with 2 types of nodes: places and transitions. Tokens (agents?) can accumulate at places (states, circles), until a transition (bar) fires (consumes input tokens, and creates output tokens). Because of that, tokens inherently interact with each other. May be deterministic or not. [wiki](https://en.wikipedia.org/wiki/Petri_net)

**Pickling** (development): dumping binary data into a database, to be loaded later, instead of processing it in some meaningful (human readable) way. In Python, may be used for serializing Python object structure. A better alternative: JSON, which is human-readable, while pickles aren't.

**Plotly Dash** (dev): A dashboard (interactive graphs and such) for Python and R. A bunchof JS, rendered in browser, that interacts with Python script that generated it via certain callbacks. These callbacks are put within the script in a certain way, and you can set (or know?) the functions the JS dashboard will call in Python when events happen in the dashboard, so you can define them, and thus process them.

**POC** (dev): Proof of Concept.

**Propensity score matching** (stats): way to compare output variables in case of unavoidable confounding factors. Based on careful matching of confounding factors between individuals in groups (many methods here), then analyzing this constrained set. The trick is that instead of matching in high-D, it uses an all-data linear model to conflate all confounding values into one "risk factor" (aka propensity score), and match based on it. This is obviously both a strong, and a weak point. Polular in epidemiology. [ref](https://en.wikipedia.org/wiki/Propensity_score_matching)

**PubSub** or **Pub/Sub** (db): asynchronous messaging (event ingestion and delivery) service used by Google cloud. [wiki](https://cloud.google.com/pubsub/docs/tasks-vs-pubsub)

**PySpark** (db): a Pythonic language (wrapper) to run on a JVM and interact with Spark. Coz it's a wrapper, it doesn't like dynamic typing that Python tolerates, but other than that is mostly similar.

**QR Factorizatoin** (math): A = QR where Q is orthonormal, and R is upper triangular. Achieved via Gram-Schmidt algorithm (aka elimination). See [[la_01]].

# R

**Redshift** (dev): MPP postgreSQL-based data warehouse from Amazon (AWS).

**REST** (dev): Representational State Transfer. Predefined set of stateless operations (stateless means that all session info is tracked by the client, not the server). HTTP is the archetypical RESTful system.

# S

**Scala** (dev): functional programming language, improved Java. Compiles to Java bytecode. Looks vaguely like a mix of Java and Pascal, but inspired by Python: more features, shorter code, but without compromising the integrity. No semicolons; different ways to define variables, functions; declare types; can define control structures. Less verbose, runs faster, supports parallelism better.

**Singular Value Decomposition** (math): A = UDV, where U and V are unitary (orthonormal, may be complex), and D is diagonal with d_ii ∈ R, d_ii>0. Dimensions: mn = mm∙mn∙nn. More general than spectral decomposition, as applies to all matrices on C, unlike QDQᵀ that only works for symmetic, or a slightly more general PDP⁻¹ for diagonalizable (sum dim eigenspaces = dim A).

**SMOTE** (stats): Synthetic Minority Oversampling Technique. A simple approach to oversampling (supplementing) under-represented categories by creating fake points (in pD of variables describing each case), that are linearlly shifted from under-represented points to one of its closest neighbors. [wiki](https://en.wikipedia.org/wiki/Oversampling_and_undersampling_in_data_analysis) 

**Snowflake** (dev): cloud SQL data warehouse; mostly interesting as a contender for older / larger players. SQL, Java Script in queries; possibility of in-database ML. Emphasis on virtualization for deep cloning, allowing virtual queries on big data. Refs: [1](https://towardsdatascience.com/why-you-need-to-know-snowflake-as-a-data-scientist-d4e5a87c2f3d), [2](https://towardsdatascience.com/machine-learning-in-snowflake-fdcff3bdc1a7).

**Spark** (dev): cluster computing framework (engine to work with big data). Written in **Scala**; donated to Apache. Runs on the JVM, nterfaces with programming languages, has its own SQL, graph processing, and what not.

# T

**Tehnical debt** (dev) picking an easy (quick to implement) but inherently limiting solution, even though statistically every limiting decision now may have to be reingeneered (scaled up) later.

**Topic modeling** (text): unsupervised classification of pieces of text into different topics, similar to k-means clustering for numbers. Typically uses LDA or NMF. ([ref](https://medium.com/ml2vec/topic-modeling-is-an-unsupervised-learning-approach-to-clustering-documents-to-discover-topics-fdfbf30e27df))

# UVW

**Wide column store** (db): Imagine a table, except that different rows may have different columns (number, names, types). 

# XYZ

**Zero-inflated model** (stats): a model that is intentially biased towards observations of zeros; for example a bimodal switch between active and inactive states, with inactive resulting in always-0, and active generating a Poisson process. [ref](https://en.wikipedia.org/wiki/Zero-inflated_model)