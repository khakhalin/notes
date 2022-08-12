# Words and topics to look up

#halfthere #todo

Other lists to look through:
* https://www.confetti.ai/
* https://machinelearningmastery.com/start-here/
* https://e2eml.school/blog.html
* [[job_search]] - lots of "interview questions" (of questionable quality) and lots of opinions and links on what to learn

**The missing semester of CS education**
https://missing.csail.mit.edu/ ⚠️
lots of useful practical bits and pieces: shell, debugging, data wrangling, metaprogramming and what not. Also has [lectures on youtube](https://www.youtube.com/watch?v=Z56Jmr9Z34Q&list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J&index=2&t=0s).
(the link is also saved in [[01_Tools]], so delete it from here once done)

# Queue

* Missing semester - parts on shell, Scripting, VIM, Linux, Command line
* how to start crontab jobs? (document in [[bash]])
* curl - how to use exactly (document in [[bash]])
* ssh
* LODs for [[tableau]] - read this: https://www.flerlagetwins.com/2020/02/lod-uses.html
* microservices - what they actually are
    * Intro by James Quigley: https://www.youtube.com/watch?v=1xo-0gCVhTU
* docker - what and how
    * installing system packages in Docker with minimal bloat: https://pythonspeed.com/articles/system-packages-docker/
    * [[docker]] and [[kubernetes]]: https://www.youtube.com/watch?v=u8dW8DrcSmo
    * https://www.docker.com/blog/containerized-python-development-part-1/ 
* rds
* Flask (allegedly flask + docker = 80% of what one needs for a typical deployment)
* AWS Elastic Kubernetes Service
* postgresql
* stream kinesis
* jump server
* tunnel (?) - in the ssh / linux context
* brew
* vim
* sudo
* bokeh
* [[kibana]]
* Reread this pandas advice just in case: https://github.com/chiphuyen/just-pandas-things/blob/master/just-pandas-things.ipynb
* how people use aws in practice
* Maybe: https://aws.amazon.com/getting-started/
* Move pandas here
* Move git here
* try XGBoost on real data
* LightGBM - new better replacement for XGBoost (as de-facto practical SOTA)
* Missing semester - Data Wrangling, GIT, Debugging, Meta, Security, last 2 lectures
* document creating (and editing) custom colormaps in matplotlib
* copy a few simple sequential [[keras]] examples here
* Check out this `categorical` type in Pandas: would it help to save space? ([ref](https://pandas.pydata.org/docs/user_guide/categorical.html))
* Document some non-trivial points for [[shapely]]
* start documenting [[tableau]]. Describe INCLUDE and FIXED in particular
* geopandas - create some useful patterns
* swagger
* python environments
* conda vs pip - see [[environments]]
    * wheel (?)
* Bash - find a tutorisl
* Bash - linkedin certification
* how to set up an etl machine (in theory)
* Python: how to properly do importing in large packages (does root import everything from its folder?)
* windowed functions (newer SQL): https://mode.com/sql-tutorial/sql-window-functions/
* common table expressions (newer SQL)
* Finish SQL quests: https://sqlzoo.net/wiki/SUM_and_COUNT
* This text about Pandas: https://github.com/chiphuyen/just-pandas-things/blob/master/just-pandas-things.ipynb
* How to design Twitter: https://www.youtube.com/watch?v=KmAyPUv9gOY
* Business 
    * pricing (read some theory on customer psychology)
    * pricing elasticity
    * why softmax is apparently a decent choice for decision probabilit calculations?
    * transportation choice    * 
* Python decorators (things like `def decorator`, `@wraps`etc)
    * Some reading: https://www.geeksforgeeks.org/python-functools-wraps-function/
    * Nice example: https://github.com/koaning/memo/blob/main/memo/_base.py
* Read something on 1D brownian walks maybe? Or is it too mathy?
* https://en.wikipedia.org/wiki/Partial_correlation
* hierarchical best-fit models 
    * [[arima]]
        * https://laptrinhx.com/arima-model-from-scratch-in-python-3262984784/
    * prophet
* Isn't everything essentially map-reduce? What is the crux? What are the alternatives? https://en.wikipedia.org/wiki/MapReduce
* kibana
* databricks
* anomaly detection
* [[Hooker2020hardware]] - read and fill (it's a stub now)
* [Constrained optimization](https://www.khanacademy.org/math/multivariable-calculus/applications-of-multivariable-derivatives/lagrange-multipliers-and-constrained-optimization/v/constrained-optimization-introduction) on Khan Academy
* refresh [[design_pattern]], especially those used in data science
* cointegration: https://www.youtube.com/watch?v=vvTKjm94Ars
* document those subplot-like tricks in [[matplotlib]]
* in [[sklearn]], for pca, add reconstruction (test in jupyter to make sure it's correct)
* https://www.jeremyjordan.me/ml-monitoring/
* https://en.wikipedia.org/wiki/Cointegration
* spectral properties of graphs
    * [[spectral_clustering]] - finish
    * [[modularity]] - connection to eigenvalues, do
* https://hdbscan.readthedocs.io/en/latest/how_hdbscan_works.html
* bayesian accuracy (and bias) compared to human-level? What's the deal here? What are these questions even  about?
* How react sites talk to SQL now?
* Implement the pandas part of this in Echo: https://koaning.io/posts/for-loop-memo/
* how to save sklearn models, and then load them back?
* Ha, D., & Schmidhuber, J. (2018). World models. arXiv preprint arXiv:1803.10122.
https://arxiv.org/abs/1803.10122
* https://en.wikipedia.org/wiki/Conditional_random_field
* Apparently Java doesn't have default values for parameters, but people use builder [[design_pattern]] to emulate it. How?? https://stackoverflow.com/questions/997482/does-java-support-default-parameter-values
* Sagemaker and Amazon EMR - what are they?
* how to extend vocabulary for DL word-embedding?
* File unfinished article descriptions currently dumped at [[deepneuro]], [[intrinsic_plasticity]], [[dendritic_comp]]
* Reread 3 notes for papers linked in the [[ticket]] note
* Django (alternative to Flask, slightly less popular)
* FastAPI (another alternative to Flask, seems more trendy, and gaining on Flask?)
* pseudoinverse (there's more than one!), and how numpy calculates them. [[pseudoinverse]]
    * Moore-Penrose?
    * https://stackoverflow.com/questions/13265299/the-difference-of-pseudo-inverse-between-scipy-and-numpy
* Reread data warehouses
* What methods are typically included in the idea of "knowing time series analysis"? Do I know them? If not, disassemble and put some terms into this stack.
* numpy broadcasting - what are the rules again? Does it not broadcast from a 2D to 1D array?
* model view controller - most important design pattern for web apps
* PostgreSQL - how is it different from other SQLs?
* How to backprop through the maxpooling layer? Document it in [[pooling]]
* How to perform unsupervised clustering using DL networks?
* [[Redis]]
* Neural networks breakpoints (a visualization / troubleshooting method):
    * https://discourse.numenta.org/t/the-breakpoints-of-a-neural-network/6775
    * https://www.youtube.com/watch?v=QEWe-aRBUAs
* What is lecun_normal init in [[init_dl]]?
* do "go around the tree" 
* Chartio
* Looker
* in-place mergesort (how?)
* dual-pivot quicksort
* https://en.wikipedia.org/wiki/S-expression
* Scalability lecture: https://www.youtube.com/watch?v=-W9F__D3oY4 (I'm at 24m right now)
* Scaling Etsy: https://www.youtube.com/watch?v=eenrfm50mXw
* Read about this HTM thing that Numenta is doing: https://numenta.com/blog/2019/10/24/machine-learning-guide-to-htm
* timsort
* react
* redux
* CICD (combined practices of continuous integration)
* With pycharm and Anaconda, do I now have Python installed twice? How does it work?
* Briefly describe other patterns [[design_pattern]]s
* predictive coding in the brain - definition? - Huang, Y., & Rao, R. P. (2011). Predictive coding. Wiley Interdisciplinary Reviews: Cognitive Science, 2(5), 580-593.
* Kick-start videos: https://www.youtube.com/playlist?list=PLllx_3tLoo4csmLveWHpjcRTXVMCcvvmc
* pull requests, and how this workflow works
    * https://www.netlify.com/blog/2020/03/31/how-to-scope-down-prs/
* How Python works inside:
    * GIL
    * Asyncio
    * Multithreading
    * Subinterpreters
* Neural turning machines: https://distill.pub/2016/augmented-rnns/
* How to do code reviews: https://google.github.io/eng-practices/review/reviewer/
* document lambda calculus in [[python]]
* MongoDB
* Differences between HTTP2 and 1
* Pub-sub architectures
* Microservices
* Kafka & Redis caching
Few questions on Load balancers & Reverse proxies. Also continuing discussion on Nginx and Varnish.
* do linear regression: keras, tf, naive genetic
* finish google training
    * https://developers.google.com/machine-learning/crash-course/fairness/programming-exercise
    * (programming exercise, followed by 3 real-world examples)
* https://en.wikipedia.org/wiki/Moore%E2%80%93Penrose_inverse
* https://www.expunctis.com/2019/01/27/Loss-functions.html
* [[motifs]] (not really motifs yet - listen to the lecture) #todo
* https://github.com/khangich/machine-learning-interview - disassemble
* json: https://www.w3schools.com/js/js_json_datatypes.asp
    * YAML - some standard for keeping stuff that's like XML or JSON, but human-readable. Apparently much hated, as it breaks at edge cases.
* Check out some intro to AWS, like maybe this one?: https://aws.amazon.com/about-aws/whats-new/2014/01/14/new-introduction-to-aws-instructional-videos-and-labs/
* How do transformers work again? - check attention at http://d2l.ai/index.html
* Recurrent networks at http://d2l.ai/index.html
* What's the deal with this new vision transformer from Facebook (published in 2020). Supposedly they surpassed convnets. Why? What does it mean for us?
    * https://ai.facebook.com/blog/data-efficient-image-transformers-a-promising-new-technique-for-image-classification/
    * Distillation through attention?
* AWS - do some tutorials
* counterfactuals: http://bayes.cs.ucla.edu/PRIMER/primer-ch4.pdf
* Granger causality (do I have notes about it somewhere?)
* work through confetti tutorials and exams, like https://www.confetti.ai/exams/14/start
* log regression from scratch
* Finish manifold learning list at [[04_Features]]
* spectral clustering
* refresh and document those statements about null-space, left, right, and what not
* how to run AdaBoost in scipy
* how to save the results of AdaBoost in scipy
* how do people actually save and load a trained model in tensorflow
* What is the best answer for "how to run logistic regression"?
* variational inference (and how is it different from sampling)
* wait, is softmax based on mean and sigma, or is it just a difference? Is it the same formula ultimately, or are there two different softmaxes?  https://twitter.com/chrisalbon/status/1323293801398509568
    * Some people seem to argue it should be called softargmax and there's a difference? Summarize here.
* Rethinking attention with performers: https://www.youtube.com/watch?v=xJrKIPwVwGM
* simple data pipeline: https://towardsdatascience.com/10-minutes-to-building-a-machine-learning-pipeline-with-apache-airflow-53cd09268977?gi=fb094bc58b8f
* Case studies for ML from [[Huyen2019book]]: https://github.com/chiphuyen/machine-learning-systems-design/blob/master/content/case-studies.md
* Features permutation - why it is bad? https://blog.ceshine.net/post/please-stop-permuting-features/
* given a sequence and a vocab, find shortest subsec that contains all vocab elements
* rmsprop: https://towardsdatascience.com/understanding-rmsprop-faster-neural-network-learning-62e116fcf29a
* Refresh and improve [[lstm ]] and [[gru]]: http://dprogrammer.org/rnn-lstm-gru
* swish activation, aka [[silu]] (apparently works real fast)
* random forest - reread
* normalizing flow - a way to learn arbitrary distributions ([tweet](https://twitter.com/MilesCranmer/status/1331085677581262848))
* perlin noise: https://en.wikipedia.org/wiki/Perlin_noise
* STDM: https://searchnetworking.techtarget.com/definition/STDM
* CDISC: https://en.wikipedia.org/wiki/Clinical_Data_Interchange_Standards_Consortium
* two sorted linked lists, combined into 1 sorted linked list
* finish heapsort, optimize, add to other sorts, compare methods
* list border elements of a tree counter-clock-wise
* The the math of transformers: [reddit link](https://www.reddit.com/r/MachineLearning/comments/j5jg1l/d_confused_mathematician_looking_for_clarity_on/)
* level-order traversal tree
* autoregressive loss - what is it?
* [[multi_armed_bandit]] in the context of clinical trials: what are some actual strategies?
* Design a parking lot: https://www.youtube.com/watch?v=DSGsa0pu8-k
* "code a simple FF neural network" - in np vielleicht?
* given a binary tree, output all left elements, then bottom, then right, no repetitions
* how is hashmap (dicts) implemented in Python, behind the scenes?
* quickly flip through the "Fluent Python" book, decide what to do with it
* reimplement qsort and merge sort
* Conway, the free will theorem: https://www.youtube.com/watch?v=ftIllWczf5w
* Laplace operator: https://www.youtube.com/watch?v=oEq9ROl9Umk
* super-toy model for graphs: if we can directly multiply by a Laplacian (because the graph is so small). Try it?
* https://en.wikipedia.org/wiki/Decision_tree_pruning
* Timsort - reimplement
* http://www.deeplearningbook.org/contents/regularization.html
* Generative vs discriminative models: https://stackoverflow.com/questions/879432/what-is-the-difference-between-a-generative-and-a-discriminative-algorithm
* ripser - a package, what is it?
* clusterogram: https://www.r-statistics.com/2010/06/clustergram-visualization-and-diagnostics-for-cluster-analysis-r-code/
* On batch normalization and contrastive learning (apparently with some vague neuro hints): https://untitled-ai.github.io/understanding-self-supervised-contrastive-learning.html
* Is it true that layers leading to batchnorm should have bias=false? Why? (Karpathy [said so](https://twitter.com/karpathy/status/1299921324333170689) on twitter)
* https://en.wikipedia.org/wiki/Wasserstein_metric
* Thompson sampling - bayesian exploration / exploitation solution, related to AB testing? https://en.wikipedia.org/wiki/Thompson_sampling
* Is it true that "predictive modeling by max kuhn" has pseudocode for all methods?
* Split bagging / boosting and friends into separate entries
* Decision tree classification - for some reason doesn't have a separate entry here yet! https://en.wikipedia.org/wiki/Decision_tree_learning
* Refresh and finish boosting and trees - based on [this informal poll](https://www.reddit.com/r/datascience/comments/icsul3/any_employed_data_scientists_willing_to_share_an/), boosting, trees, and SVMs are really widely used in practice (except for a niche subtype of people who only do deep learning), so (I guess) most likely they will be asked
    * [[gbm]] Gradient Boost - finish!
        * XGBoost, and how to parallelize it (one of the most popular interview topics, apparently!)
    * [[random_forest]] -  reimplement
    * [[svm]] - also reimplement? Code implementation is sometimes on interviews.
    * CART / Regression trees
* On hopfield networks: https://ml-jku.github.io/hopfield-layers/
* Self-training with noisy student (short): https://www.youtube.com/watch?v=q7PjrmGNx5A
* Geometric deep learning: https://www.youtube.com/watch?v=8kTxTX0eBRA
* Skan this: https://yangshun.github.io/tech-interview-handbook/introduction
* LCF notation for graphs: https://mathworld.wolfram.com/LCFNotation.html
* Also what's happening here?: https://www.christophermanning.org/projects/building-cubic-hamiltonian-graphs-from-lcf-notation
* linear LR warmup - what is it? (Something related to training giant models)
* Understand the very basic of this definition: https://en.wikipedia.org/wiki/Conjugate_prior
* Finish early graph lectures by Leskowec
* What do graph laplacians really mean?
* What is a Computation Graph and Who Cares?
https://end-to-end-machine-learning.teachable.com/courses/advanced-neural-network-methods/lectures/12194418
* reservoir sampling
* Bayesian linear regression
* Gibbs sampling
* Metropolis-Hastings
* hierarchical Dirichlet processes
* kernel PCA
* CMAC
* Fisher vectors
* EM algorithms (for various distributions)
* fuzzy logic
* conjugate gradients 
* what is github.io and how it works? What can it do? Is it a good way to host blogs?
* Do all graph exercises from the red book
* How to debug in Python, beyond print()? What are the best practices here?
* Maximum Likelihood (MLE) vs Maximum apriori (MAP)
* Generative vs Discriminative models
* Can batch norm be considered a type of regularization? (Actual interview question)
* flip a tree (with honest suffering)
* BayesNet - what is it?
* ICA: how does it work, and in what cases it's better than PCA
* radix sort
* Finish dangit git
* Refresh hash tables (theory and implementation)
* Refresh (maybe reimplement) red-black trees
* LRU cash (Leetcode quest)
* Difference in decision boundaries for all algorihtms (Tree vs Logistic vs Linear Reg vs SVMs vs Naive Bayes)
* Practice SQL on Hackersrank
* Systems interivew - do a minimal set (at least fill the dictionary with stubs)
* Watch some basic systems interview videos
* Bellman-Ford algorithm (pairwise distances between all nodes in a graph)
        * https://en.wikipedia.org/wiki/Johnson%27s_algorithm
* Leetcode-style question: serialize deserialize a tree
* Refresh combinatorics formulas
* torch_geometric - what is it? What's the status of it?
* https://github.com/iamtrask/Grokking-Deep-Learning - is it true that it builds pytorch-like system from scratch? Check.
* Implement Dijkstra
* Implement "Fix a binary tree"
* Does cross-entropy in DL work with softmax predictions, or probabilities? I think probabilities, but clarify.
* Softmax approximations: https://ruder.io/word-embeddings-softmax/index.html
* [[Xie2020noisy_student]] - finish this one
* Weisfeiler-Lehman Isomorphism Test (see also: [[graphsage]])
* Density-based clustering, aka HDBSCAN: https://towardsdatascience.com/a-gentle-introduction-to-hdbscan-and-density-based-clustering-5fd79329c1e8 (the idea is to project the points in a low-D space, but try to preserve local densities of points)
* How to do bayesian 01 guesser? How does process 001 if current history is 0? How to combine n-grams of diff length? Derive, and also google / check on the web.
* When to use BatchNormalization() in a network? Why don't we use it always? Or should we use it always? What's the logic here?
* MongoDB
* [[julia]] - a 2020 review paper about the language
* manual bagging and bumping on XOR
* Manual bagging for a bad diagonal case (flag with quadrants)
* Mean, variance for Normal, Uniform, Poisson, binomial, write Gaussian pdf formula
* Sampling techniques and common designs (e.g. A/B).
* Common conjugate priors (Bayesian stats)
* Theory of designing unit tests
* How to prove that both entropy and gini coeff max when all $p_i$ are the same and = 1/n?
* write backprop on a piece of paper
* https://calculatedcontent.com/2020/02/16/weightwatcher-empirical-quality-metrics-for-deep-neural-networks/
* "Good Enough" architecture: https://www.youtube.com/watch?v=PzEox3szeRc
* "Mastering chaos" - Netflix microservices: https://www.youtube.com/watch?v=CZ3wIuvmHeM	
* Refresh Linear regression and its assumptions
* Refresh Logistic regression and ROC curves
* Be able to draw confusion matrix on classification tasks
* How to prune tree-based models
* Code linear regression manually
* Code logistic regression manually
* Why autoencoders use KL and not L2 as loss? What's the logic of when KL is used? Older papers, like [[Hinton2006dim]] used cross-entropy. Why?
* https://en.wikipedia.org/wiki/Universal_approximation_theorem
* churn prediction problem
* https://en.wikipedia.org/wiki/Takens%27s_theorem
* https://en.wikipedia.org/wiki/List_of_algorithms - at least read through them haha :)
* Geometric algorithms: https://www.geeksforgeeks.org/geometric-algorithms/
* Bash
* Wald’s sequential analysis
* Bayesian extensions of the stochastic blockmodel
* https://en.wikipedia.org/wiki/Gated_recurrent_unit
* https://en.wikipedia.org/wiki/Q-learning - file to [[q-learning]]
* Visual guide to Evolution strategies: http://blog.otoro.net/2017/10/29/visual-evolution-strategies/ Must, because visual and nice!! See [[gen_alg]]
* Python decorators
* Regular expressions [[regex]], including in Python
* https://blog.reverberate.org/2020/05/29/hoares-rebuttal-bubble-sorts-comeback.html
* finish Google production thing
* How to color a graph in 4 colors?
* https://en.wikipedia.org/wiki/Rope_(data_structure)
* wrong assortativity on different circuits	 - for blog post
* check out "Competitive programming" textbook
* sieve of eratosphenes
* Code a fast 	spanning tree of a graph (using bfs or dfs)
* splay tree
* read / watch kickstarter competition advice (on their site)
* in a sequence, if any k elements sum to target - code from scratch quickly
* All graph exercises from here (they are excellent!): https://algs4.cs.princeton.edu/41graph/
    * Bridge: an edge that disconnects the graph if removed. Using DFS, find a way to check if E is a bridge in O(V+E)
    * Articulation point: same, but for a vertex (disconnects the graph if removed). Find an O(V+E) way to check if V is an articulation point.
    * Check if a graph is biconnected, aka 2 paths with diff V between any v1, v2, aka a simple cycle through any v1 v2
    * Center of a tree
    * Deletion sequence (without disconecting a graph)
    * "Rogue" game
* string sort from the red book
* Do words-based analysis
* how to solve sudoku?
* Re-implement [[red-black_tree]], this time without parents, or with compartmentalized parents (`a,b = link(from=a,to=b)` that sets both connections?)
* Re-implement [[avl_tree]]: make it neater, and fix delete.
* Do Bellman-Ford
* refresh and summarize O: https://www.bigocheatsheet.com/
* Code two types of hash-tables from memory
* Scan [this helpful list](https://www.freecodecamp.org/news/coding-interviews-for-dummies-5e048933b82b/); digest into unit-items if needed
* Rotate array in-place with juggling
* treap
* what is P, NP, and NP-complete: definition ([this youtube again?](https://www.youtube.com/watch?v=YX40hbAHx3s))
* Bloom filter (refs: [1](https://dominikschmidt.xyz/bloom-filter/))
* Make sure [this list from geekforgeeks](https://www.geeksforgeeks.org/top-10-algorithms-in-interview-questions/) is exhausted
* Make sure [this other list](https://www.geeksforgeeks.org/find-subarray-with-given-sum-in-array-of-integers/) from geekforgeeks is exhausted
* implement Dijsktra
* geometrical collision-detection algorithms
* Go through all subtopics here: https://www.geeksforgeeks.org/binary-search-tree-data-structure/
* https://towardsdatascience.com/8-useful-tree-data-structures-worth-knowing-8532c7231e8c
* https://en.wikipedia.org/wiki/Fibonacci_heap
* https://en.wikipedia.org/wiki/Pairing_heap
* https://cp-algorithms.com/data_structures/fenwick.html
* https://cp-algorithms.com/data_structures/segment_tree.html
* Revisit [this link](https://cs.stackexchange.com/questions/111353/analyze-time-series-data-to-get-min-max-over-past-period-of-time) about keeping running max & min of a time series (monotonous wedge?) - describe it, code it. [Pdf of the paper](https://arxiv.org/pdf/cs/0610046.pdf).
* Convex hull: [wrapping algorithm](https://www.geeksforgeeks.org/convex-hull-set-1-jarviss-algorithm-or-wrapping/)
* Can I code a line graph algorithm? Refs: [wolfram](https://mathworld.wolfram.com/LineGraph.html), [wiki](https://en.wikipedia.org/wiki/Line_graph), [overflow](https://stackoverflow.com/questions/13700954/graph-transformation-vertices-into-edges-and-edges-into-vertices)
* https://www.byte-by-byte.com/google-interview/ - check other topics
* Hungarian algorithm
* minimum cut (on graphs), with related topics
* Operations Research - the type of DS they do for logistics. Flip through a book?
* https://en.wikipedia.org/wiki/S-expression
* Go through [[ml_questions]], see what is missing

# Python tools

* How does garbage collection work in Python? What's the logic, and how fast is it? ([link](https://gist.github.com/osavsunenko-ring/205fa72c65d6343eaede0dc43f1c79d4))
* collections - or whatever is this common module ppl often use with prepackaged datastructures
* refresh "read from a console" and read from file in Python
* Command line intro: https://learnpythonthehardway.org/book/appendixa.html
* Lint (pylint) - what is it and why? It seems that it can catch errors, type mismatches etc.
* Keras checkpoints
* Python collections: deque, Counter, defaultdict etc.
* Python closures, why they are a thing, what's dangerous about them, and how to use them

# Systems

See also: [[system_design]]

Go through 2-3 youtube lectures in the queue that describe some of the existing solutions:
* Ruby on rails - what is it, how it works, and why so popular?
* Hadoop vs Spark video: https://hackr.io/blog/hadoop-vs-spark
* Avro
* Parquet (data storage format)
	* data warehouse technologies
* difference between columnar and row-oriented data stores
* star schema
* difference between dimension and fact tables
* OLAP ([wiki](https://en.wikipedia.org/wiki/Online_analytical_processing))

* https://www.hiredintech.com/app - a whole intro mini-course on systems design?
* https://www.byte-by-byte.com/3-ways-to-ace-your-system-design-interview/
* Go through the list of terms and write down a definition for each of them
* https://github.com/donnemartin/system-design-primer
    * https://github.com/donnemartin/system-design-primer#index-of-system-design-topics
* https://github.com/checkcheckzz/system-design-interview

Every solution depends on parameters of scale: amount of data, n users, requests per seconds, expected response time, read/write ratio.

List from Google
* Processes, threads, concurrency
* locks, mutexes, semaphores, monitors
* deadlocks, livelocks, and how to avoid them
* resource allocation
* context switching
* scheduling
* multicore concurrency

**Data solutions**
* sqlalchemy
* airflow,
* Docker
* What does it mean to "learn NoSQL"? What technical, practical skills are meant here?
    * NoSQL has a badge on LinkedIn - is it passiable?
* AWS - also has a badge on Linkedin
* Kotlin - also has a badge
* ETL (Extract, Transform and Load) pipelines
* Hadoop
* Hive
* DevOps
* Orchestration
* RDF
* SPARQL
* Triple (RDF) and graph stores
* MongeDB
* Couchbase
* Memcashed
* Redis cluster
* Zookeeper
* Kafka
* NGINX, HAProxy - balancers
* Solr, ElasticSearch
* Blobstore
* Spark
* Flink
* postgreSQL

**Concepts**
* FAIR data principles and support FAIRification of legacy data
* [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem) (Consistency, Availability, Partition tolerance) - impossible to provide all three, always a trade-off.	 P is a given, so relational DBs favor C, while noSQL favors A
* Data sharding (how to split data that doens't fit).
* Consistent hashing - way to solve data sharding
* Optimistic and pessimistic locking
* Strong vs eventual consistency
* Map reduce
* Multithreading (consistency, locks, CAS)
* Concurrency, threads, deadlocks, starvation
* https://en.wikipedia.org/wiki/Database_transaction
* https://en.wikipedia.org/wiki/Database_normalization
* https://en.wikipedia.org/wiki/Document-oriented_database
* https://en.wikipedia.org/wiki/Graph_database
* ACID transactions
* https://en.wikipedia.org/wiki/Polyglot_persistence
* https://en.wikipedia.org/wiki/Object-relational_impedance_mismatch

**Practicality - web**
* Javascript React - how does it even look like?
    * Interesting projects:
        * http://orgo.stolarsky.com/
        * https://www.christophermanning.org/projects/building-cubic-hamiltonian-graphs-from-lcf-notation
* Bloom filters and count-min sketch
* Design patterns (OOP)
* Cashing in DBs
* TCP/IP
* IPv4 vs IPv6
* DNS lookup
* HTTP vs HTTP2 vs websockets
* TCP vs UDDP
* Data centers and how they work
* Public key infrastructure, symmetric and asymmetric encryption
* CDN - content delivery network - important for streaming

**Practicality - local**
* Parts of a modern OS? Levels of cashing in a modern OS?
* Performance (bandwidth) of CPU / memory / SDD / HD / network
* How to use sequential (as opposed to random) reads and write on HD to speed up your processes
* Paxos - what is it?
* Virtual machines and containers

* Allegedly, a list of important techical skills for a SWE on a job market. First, figure out what could it mean to "learn them". How to learn them, and what does it mean, to know them?
    * Front-end: 
        * Node.js
        * React
        * Angular
    * Devops and tooling:
        * Docker
        * Kubernetes
        * service mesh problems
        * AWS/Azure
        * GCP via Coursera and hands-on qwiklabs?
    * Other: Ruby on Rails (simple, but now apparently considered dated?)

# Classic ML and related math

* Actually do gradual boosting (from a library) for some synthetic dataset; visualize
* Laplacian short video: https://www.youtube.com/watch?v=oEq9ROl9Umk
* that way to create ranking from asymmetric preferences via graphs [[ranking]]
* Laplacian operator (create a new page for basic vector calc) [1h youtube](https://www.youtube.com/watch?v=oEq9ROl9Umk), [wiki](https://en.wikipedia.org/wiki/Laplace_operator)
* dual bases: why is this term a thing? How does it help? Describe here: [[linalg]]
* How come stochastic gradient descent provides regularization? Why adding bagging to GBM, for example, can be considered a case of regularization? [[05_Ensembles]].
* Do we have 2 different descriptions from Gram-Smidt: in [[la_01]] and in [[02_Regression]]? Can we isolate and combine?
* Figure out that connection between ridge regression and and PCA that I've started in the ridge regression chapter! Seems important and reasonably doable, no? [[02_Regression]].
* finish gradient boosting
* finish SVM
* random forest
* random forest vs. adaboost?
* self-organizing (Kohonen) maps
* apply svm, boosting
* https://en.wikipedia.org/wiki/Probabilistic_roadmap
* normal equation ([link?](http://mlwiki.org/index.php/Normal_Equation#Normal_Equation_vs_Gradient_Descent))
* implement from scratch in numpy:
    * small DL
    * SVM
    * random forest
* Jax: [Baysian NN in Jax](https://colab.research.google.com/drive/1gMAXn123Pm58_NcRldjSuGYkbrXTUiN2) - collab notebook 
* that visual blog post about types of DL optimizers
* Capsule neural network - some sort of generalization of convolutional networks for hierarchical data?? Never heard of them, but they are actually quite well cited!!
* Kalman filter
* magic squares algorithm?
* HyperNEAT - a paradigm for generative network evolution
* graph cuts in computer vision
* Latent Dirichlet Allocation ([ref](https://towardsdatascience.com/end-to-end-topic-modeling-in-python-latent-dirichlet-allocation-lda-35ce4ed6b3e0))
* unit tests, pytests, mocking
* Bayesian PCA
* Demixed principal component analysis - some MDS technique on top of PCA; seems to be moderately popular with some neuro people. What's the gist of it?
* Expectation-Maximization
* Bayes Information Criterion - will be naturually described once I cover model selection?
* better understanding of Gaussian processes
* Graphical models
* Hidden markov models
* Markov random fields (precursor of modern graph models?)
* Variational inference
* Influence functions ([this pdf may help](https://arxiv.org/pdf/1810.03260.pdf))
* Resampling techniques: SMOTE, isotonic regression, Platt scaling
* Principal variables
* Mahalanobis distance
* Particle filters

# Deep Learning

* Eutoencoders in Keras: https://blog.keras.io/building-autoencoders-in-keras.html
* Why rotate pictures (augmentation) if we can just enforce rotationally-symmetric kernels. Is it possible? Is it desirable?
* How to tell the receptive field of a neuron in a deep network? Do they gradient descend on it?
* Word2vec - how does it work? And does it come pretrained or what?
* residual blocks - why are they even a good idea? What's the deal here? How can it possibly help?
* Maximum Mean Discrepancy trick
* Replica trick and log-partition function (??)
* Gumbel-max trick
* Russian roulette trick
* Bayesian conjugacy trick
* Reparameterization trick
* Duality trick, easier Fenchel
* Doubly Reparameterized Gradient Estimators
* Do people use leakyReLus often? Do they prefer them for pruning, or is it ignored?
* inception blocks
* noise-contrastive estimation
* self-supervised boosting
* Wavenet - that famous convolution network that generates audio on-the-fly

# ESL

Current position: p290

Parked parts of ESL that need to be revisited:
* p161 to p218 - Smoothing, wavelets, smoothing kernels, local regression, kernel size choice
* p219 - a whole chapter on Model selection, Bias-Variance Tradeoff, effective number of parameters, cross-validation, and bootstrapping
* p261 - another whole chapter. Model averaging, max likelihood, some Bayesian inference, more bootstrapping, the EM algorithm (Expectation-Maximization), Gibbs samping, bagging, MCMC

# Other math

* How to calculate single-value-decomposion and eigenvector decomposition in practice? How do they code it for numpy, for example?
* FFT
* Lagrange multiplyier - is there an easy (not formal, but intuitive) proof that it works?
* Hamiltonians and how dynamical systems invoke them
* Perron–Frobenius theorem - something related to centralities and or network graph eigenvectors
* kuramoto oscillations

# Business

* Deconstruct this: https://boards.greenhouse.io/doordash/jobs/2173764
* Customer satisfaction surface - how they look like, how they are delivered, how they feel from the data point of view

# Refs

Lists of topics used by other people:
* https://github.com/amitness/learning

# Temp Dump

* https://rpubs.com/wgervais/667244
* https://www.youtube.com/watch?v=gXbThCXjZFM&list=PLMrJAkhIeNNQV7wi9r7Kut8liLFMWQOXn&index=21
* https://arxiv.org/pdf/2009.06489.pdf
* https://gema-parreno-piqueras.medium.com/real-world-games-look-like-spinning-tops-b77411a54b57
* https://arxiv.org/pdf/2004.09468.pdf
* https://www.youtube.com/watch?v=sQFxbQ7ade0
* https://www.euclidea.xyz/en/game/packs/Epsilon/level/LineAlongPoints
* https://www.youtube.com/watch?v=i5tUMd__tC0
* https://www.youtube.com/c/YannicKilcher
* https://aeon.co/essays/the-rise-and-fall-of-the-oxford-school-of-fantasy-literature
* https://en.wikipedia.org/wiki/Bradley%E2%80%93Terry_model
* https://www.researchgate.net/profile/Jasmin_Saric/publication/224890616_Literature_mining_for_the_biologist_from_information_retrieval_to_biological_discovery/links/00b7d51e65b75b50cf000000/Literature-mining-for-the-biologist-from-information-retrieval-to-biological-discovery.pdf
* https://journals.sagepub.com/doi/pdf/10.1177/1536867X0200200405
* https://arxiv.org/abs/1801.01253#
* https://www.builtinboston.com/f
* https://www.youtube.com/watch?v=0--5AxiZefg
* https://www.youtube.com/watch?v=DG7YTlGnCEo
* http://web.stanford.edu/class/cs224w/
* https://en.wikipedia.org/wiki/Yarsanism#Worship
* https://www.bbc.com/news/world-middle-east-50378946
* زرده (دالاهو)