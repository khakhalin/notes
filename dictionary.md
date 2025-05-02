# Dictionary of Terms

Not exhaustive: if a term is explained in one of the thematic chapters, it is not duplicated here. This list is mostly for "orphan" terms that are yet too short or off-topic to get a personal entry elsewhere.

See also other topical dictionaries:
* [[oop]] - has its own set of terms
* [[product_supply]] - a list of idiosynratic terms that PS ppl really like


# AB

**ACID** (db): Atomic, Consistent, Isolated, Durable. An opposite of BASE. See: [[database]], [[db_consistency]]

**ASIC** (hard): Application-Specific Integrated Circuit, such as Google-NVideia TPUs for example ([wiki](https://en.wikipedia.org/wiki/Tensor_processing_unit))

**Athena** a simple (not very efficient, but fast and [[sql]]-based) way to query from Amazon S2. ([ref](https://www.techtarget.com/searchaws/definition/Amazon-Athena)). Fully in the cloud, paying for the data scanned (not cheap, if used for anything analytic-like by mistake). No indexing, but partitioning.

**Bagging** (ml): the simplest way to create an ensemble of classifiers using an algorithm: split data into bags (with replacement), train a different model on each of them. Final prediction = mean of all predictions. [ref](https://analyticsindiamag.com/primer-ensemble-learning-bagging-boosting/). See [[05_Ensembles]].

**BASE** (db): basically Available, Soft state, Eventually consistent (a silly backronym of an antonym to ACID). See [[db_consistency]], [[database]]

**BIC** (stats): Bayes Information Criterion, an approach to model selection. To be described (for now, [wiki](https://en.wikipedia.org/wiki/Bayesian_information_criterion)).

**Boosting** (ml): an alternative to _bagging_ for ensemble generation: Iteratively select samples that previous learners failed to learn or disagreed upon, and use them to train new learners; majority vote at the end. [ref](https://analyticsindiamag.com/primer-ensemble-learning-bagging-boosting/). See [[05_Ensembles]].

**BST** (prog): Binary Search Tree. See [[bst]].

# C

**CART** (ml): Classification And Regression Tree; often used for gradient boosting. An umbrella term to cover decision trees (classifiers), but also trees that have real numbers assigned to their leaves as outputs. See [[05_Ensembles]].

**Clique** (math): an all-to-all subgraph within a graph. Two confusing names: **Maximal clique** is just a clique that is not a part of a larger clique (you cannot include one more vertex or ege from a graph, and still have a clique). **Maximum clique** is the largest clique in a graph. Apparently finding cliques, as well as enumerating cliques, is an NP problem with no good solution. [wiki](https://en.wikipedia.org/wiki/Clique_problem), [current best algorithm (hard)](https://www.sciencedirect.com/science/article/abs/pii/S0305054810001504)

**Collaborative filtering** (pipe): a name for what Netflix users do when they watch movies and generate data of "co-occurrence". Similar to Federated Learning. [[collab_filtering]]

**CPC** (marketing): Cost Per Click = Ad spending / N clicks

**CPI** (marketing): Cost Per Install = Ad spend / N installs

**CRM** (business): Customer Relationship Management system. A customer DB + a system to aid conversations (aka "Contact Management": history of conversations etc.) + pipeline tracking (aka Lead management) + analytics and forecasting + tools for within-organization info sharing. ([ref](https://www.salesforce.com/eu/learning-centre/crm/crm-systems/))

# D

**DAG** (math): Directed Acyclic Graph

**DAU** (marketing): Daily Active Users

**Databricks Workspace** (db): workspace (environment) by Databricks, to interact with programs (jobs), notebooks, libraries, data. (?)

**Dataiku**: some sort of visual programming language platform for data science?

**Document store** (db): instead of rows, returns XML documents or something similar. [wiki](https://en.wikipedia.org/wiki/Document-oriented_database)

**Drop-in replacement** (prog): Replacing part of the code without rewriting anything else. Aka "bug for bug compatibility" (drop-in will only work if all idiosynctratic bugs match exactly).

**Dremio** (db): Position themselves as "Data Lakehouse" (a wordplay on [[datalake]] and [[dwh]]?) They claim that it's a datalake solution, but unlike typical approaches that allow only limited querying (e.g. Amazon S3 with Athena), this one can directly support analytical work and dashboarding. But aparently quite expensive. [1](https://docs.dremio.com/current/), [2](https://dashboardfox.com/blog/what-is-dremio-and-how-can-it-be-used-in-business-intelligence/)

**DSS** (db): Decision Support System. Executive dashboards, medical diagnosis systems etc.

**Dynamic programming** (prog): Recursion (or some other form of divide-and-conquer) + memoization (to never calculate the same sub-problem twice).

**DynamoDB** (db) a [[nosql]] key-value db from Amazon.

# EF

**ECS** (dev): Amazon Elastic Container Server. Is used to manage (run, stop) Docker containers on a cluster.

**EDA** (stats): Exploratory Data Analysis.

**ELA** (image): Error Level Analysis - analysis of JPG artifacts that can indicated that the imate is photoshoped (first saved in a lossy format, then combined, then saved again), as the stats of errors would be different at different parts of the image. [wiki](https://en.wikipedia.org/wiki/Error_level_analysis)
	
**ERP** (business): Enterprize Resource Planning. Stuff like inventory, shipments, production, raw materials, capacity, etc. ([wiki](https://en.wikipedia.org/wiki/Enterprise_resource_planning))

**ETL** (db): Extract Transform Load ([wiki](https://en.wikipedia.org/wiki/Extract,_transform,_load)). Either a process, or a tool. See [[data_wh]]

**Feature store** (data) A practical concept for data project implementation: a collection of curated features that are automatically produced (updated) for new data, and can be tapped into by various projects. In a way, paying it forward with feature engineering.

# G

**GAM** (stats): Generalized Additive Models: f = sum of smooth functions. Often GLM + some smoothing splines.

**GBM** (ml): Gradient Boosting Machine. See [[05_Ensembles]]

**GCD** (math): Greatest Common Divisor (see Euclidean algorithm: [[gcd]])

**GCP** (db): Google Cloud Platform.

**GOFAI** (ai): Good Old Fashioned Artificial Intelligence (aka Symbolic Intelligence). Dominant until 1980. Plugging inputs into a logic scheme (at most - fuzzy logic).

**Golang** aka **Go** (prog): statically typed compiled language by Google, similar to C, but with memory safety, garbage collection, concurrency support, and structured type system.	

# HIJ

**Hadoop** (db): a collection of open-source software utils for cluster support, maintained by Apache. Apparently, an alternative to data warehouses and MPP ( #todo why?).

**HCI**: Human-Computer Interaction. An academic area of research.

**Heap** (prog): a tree-like structure where children < parents. An optimal realization of a [[priority_queue]] (similar to a stack, but with built-in sorting by priority).

**Hessian** (math): a matrix of all 2nd-order partial derivatives, for a scalar function, over all of its arguments. H_ij = ∂²f/∂x_j ∂x_i. H is a transposed Jacobian of a gradient (Hᵀ = J∇f). Appears in optimization as a Taylor approximation around the optimum.

**Indicator matrix** (data): A matrix Y, in which each row one-hot encodes the class g of a data point x. See [[03_Classification]].

**Jacobian** (math): a matrix of all 1st order partial derivates of a vector function over its arguments. J_ij = ∂f_i / ∂x_j.

**JVM** (dev): Java Virtual Machine.

# KL

**Kimball data warehouse model**: see [[data_wh]]

**LDA** 
1. (text): Latent Dirichlet Allocation (see [[10_Text]])
2. (ml): Linear Discriminant Analysis (see [[04_Features]])

**loess** (stats): Locally Estimated Scatterplot Smoothing, aka Savitzky-Golay filter. See [[smoothing]]

**log-sum-exp** (math): a handy smooth differentiable approximation for a max function: $\text{lse}(x)=\log \sum_i e^{x_i}$ . Immediately related to softplus (see [[activation_functions]]), which is lse(0, t). [ref](https://nhigham.com/2021/01/05/what-is-the-log-sum-exp-function/)

# M

**MAE** (ml): Mean Average Error, aka L1 loss.

**Map Reduce** (dev): programming model for parallel computing; a type of split-apply-combine strategy. Essentially, seems to define two functions, one function on splittable tasks (something like `for each element in List`) that analyzes its `element` and sends answers out; and another one that catches answers and combines them all into one answer.

**Marshaling** (programming): an alternative to pickling, where instead of dumping memory representation and hoping that it can be read, you force some sort of a reversed-engineered structure on it.

**MCMC** (ml) Markov Chain Monte-Carlo. See [[modeling]].

**MLE** (stats): Max-Likelihod Estimation. See [[02_Regression]].

**MPP** (dev): Massively Parallel Processing: a data architecture for big data. Examples: Amazon Redshift, Microsoft Azure, Snowflake, Google BigQuery. "Share nothing" architecture: sets of data don't overlap. Easier to deploy; supports SQL. Alternative to Hadoop ( #todo why?).

**MST** (algos): Minimum Spanning Tree. A cycle-less subgraph of a weighted graph with min ∑w_ij. See [[graph_algos]].

# N

**nat** (Information): it's like a bit, but for natural, rather than base 2 log. Way too nerdy for my taste.

**NMF** (text): Non-negative Matrix Factorization, V=WH + U, where W and H are non-negative, and also typically V is alot×alot, but W is alot×little, and H is little×alot (similar to other dimensionality reduction techniques). Often used with text analysis.

**Non-functional requirements** : true global requirements on the product, as opposed to functional requirements (particular functions a system should be able to do). Examples: safety, secuirity, transparency, testability, scalability, usability, various types of compliance, open source etc.

**noSQL** (db): non-relational databases: not tabular-based, realtime big data. See: [[nosql]]

# OPQ

**OLAP** (db): Online analytical processing. An approach to business analytics that involves quering with slicing, consolidation (aggregation) and drill-down (going to the details). OLAP systems are mostly optimized for reading, and apparently are not too scalable (compared to [[data_wh]]) [wiki](https://en.wikipedia.org/wiki/Online_analytical_processing)

**OLTP** (db): Online Transaction Processing. A kind of an antonym for OLAP, as OLTP (online transactions) implies lots of writing (not only reading), and higher volume. [wiki](https://en.wikipedia.org/wiki/Online_transaction_processing)

**ORM**: Object-Relational Mapping. The design of mapping between objects in a program (in the [[oop]] sense) with their methods and properties, and the external data sources (ultimately, [[database]]s with columns) that these objects represent and give access to. Objects are inherently hierarchical, and tend to encode one partiular way of accessing the data, while databases are at the bottom of it atomic (individual records, "tuples") with [[sql]] and friends only faking  a hierarchical access to them. A reliable mapping here is not always easy to achieve. Example library for [[python]]: SQLAlchemy.

**Petri net** (math): a way to model discrete dynamic systems via a directed bipartite graph with 2 types of nodes: places and transitions. Tokens (agents?) can accumulate at places (states, circles), until a transition (bar) fires (consumes input tokens, and creates output tokens). Because of that, tokens inherently interact with each other. May be deterministic or not. [wiki](https://en.wikipedia.org/wiki/Petri_net)

**Pickling** (development): dumping binary data into a database, to be loaded later, instead of processing it in some meaningful (human readable) way. In Python, may be used for serializing Python object structure. Better alternatives: [[parquet]] (for table data), [[json]] (for everything else)

**Plotly Dash** (dev): A dashboard (interactive graphs and such) for Python and R. A bunch of JS rendered in browser, interacting with Python scripts that generate it via certain callbacks. These callbacks are put within the script in a certain way, and you can set (or know?) the functions the JS dashboard will call in Python when events happen in the dashboard, so you can define them, and thus process them.

**POC** (dev): Proof of Concept.

**PostgreSQL** (sys): An open-source SQL server that is a bit slower tham mySQL, but more reliable, and fully ACID compliant (mySQL is not necesarily). Fault-tolerant because of "write-ahead logging". ([ref](https://www.guru99.com/introduction-postgresql.html), [wiki](https://en.wikipedia.org/wiki/PostgreSQL))

**Propensity score matching** (stats): way to compare output variables in case of unavoidable confounding factors. Based on careful matching of confounding factors between individuals in groups (many methods here), then analyzing this constrained set. The trick is that instead of matching in high-D, it uses an all-data linear model to conflate all confounding values into one "risk factor" (aka propensity score), and match based on it. This is obviously both a strong, and a weak point. Polular in epidemiology. [ref](https://en.wikipedia.org/wiki/Propensity_score_matching)

**PubSub** or **Pub/Sub** (db): asynchronous messaging (event ingestion and delivery) service used by Google cloud. [wiki](https://cloud.google.com/pubsub/docs/tasks-vs-pubsub)

**PySpark** (db): a Pythonic language (wrapper) to run on a JVM and interact with Spark. Coz it's a wrapper, it doesn't like dynamic typing that Python tolerates, but other than that is mostly similar.

**QR Factorizatoin** (math): A = QR where Q is orthonormal, and R is upper triangular. Achieved via Gram-Schmidt algorithm (aka elimination). See [[linalg]].

# R

**RCT** (stats): Randomized Controlled Trial(s) - basically, what most scientists mean by an "experiment". Diff treatment groups, assigned randomly. Gold standard (everything else is epistemologically inferior)

**REST** (dev): see [[restful]]

**RSS** (stats): Residual Sum of Squares. See [[l2]], [[02_Regression]], [[bias-variance]]

# S

**Scala** (dev): functional programming language, improved Java. Compiles to Java bytecode. Looks vaguely like a mix of Java and Pascal, but inspired by Python: more features, shorter code, but without compromising the integrity. No semicolons; different ways to define variables, functions; declare types; can define control structures. Less verbose, runs faster, supports parallelism better.

**SGD** (ml): Stochastic Gradient Descent. When gradient is calculated for every point, instead of the entire dataset (as for classic, aka batch gradient descent). See: [[Ruder2016descent]]

**Singular Value Decomposition** (math): See: [[svd]]

**SMOTE** (stats): Synthetic Minority Oversampling Technique. A simple approach to oversampling (supplementing) under-represented categories by creating fake points (in pD of variables describing each case), that are linearly shifted from under-represented points to one of its closest neighbors. [wiki](https://en.wikipedia.org/wiki/Oversampling_and_undersampling_in_data_analysis) 

**Spark** (dev): cluster computing framework (engine to work with big data). Written in Scala; donated to Apache. Runs on the JVM, nterfaces with programming languages, has its own SQL, graph processing, and what not.

**SPF** (prog): Shortest Path First, better known as Dijkstra's algorithm: [[dijkstra]]

# T

**Tehnical debt** (dev) picking an easy (quick to implement) but inherently limiting solution, even though statistically every limiting decision now may have to be reingeneered (scaled up) later.

**Topic modeling** (text): unsupervised classification of pieces of text into different topics, similar to k-means clustering for numbers. Typically uses LDA or NMF. ([ref](https://medium.com/ml2vec/topic-modeling-is-an-unsupervised-learning-approach-to-clustering-documents-to-discover-topics-fdfbf30e27df))

# UVW

**upsert** (db) - a stupid term for "update a record if it already exists, and if not, insert a new record"

**VAE** (dl): Variational Auto-Encoder. See [[08_Unsupervised]]

**VPS** (system): Virtual Private Server. Virtual Machine -style half-way between shared service and private server?

**Wide column store** (db): Imagine a table, except that different rows may have different columns (different number, different names, types). Related to [[nosql]], [[json]]

# XYZ

**XGBoost** (dev): an open-source library that does gradient boosting. Same type of an object as Keras, for example: available for many languages, as libraries; can run locally, or in a scaled-up distributed way; takes care of some fine-turning. Popular with Kagglers, apparently. ([wiki](https://en.wikipedia.org/wiki/XGBoost))

**Zero-inflated model** (stats): a model that is intentially biased towards observations of zeros; for example a bimodal switch between active and inactive states, with inactive resulting in always-0, and active generating a Poisson process. [ref](https://en.wikipedia.org/wiki/Zero-inflated_model)
