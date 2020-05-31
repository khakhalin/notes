# Words and topics to look up

Key tags:
* #halfthere - for projects that are close to completion.
* #todo - for follow-ups that need to be done

Videos
* Conway, the free will theorem: https://www.youtube.com/watch?v=ftIllWczf5w
* Laplace operator: https://www.youtube.com/watch?v=oEq9ROl9Umk

Google crash course, about ML production:
https://developers.google.com/machine-learning/crash-course/production-ml-systems

What is a Computation Graph and Who Cares?
https://end-to-end-machine-learning.teachable.com/courses/advanced-neural-network-methods/lectures/12194418

Kick-start videos: https://www.youtube.com/playlist?list=PLllx_3tLoo4csmLveWHpjcRTXVMCcvvmc

**The missing semester of CS education**
https://missing.csail.mit.edu/
lots of useful practical bits and pieces: the shell, debugging, metaprogramming and what not. Also has [lectures on youtube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J).
(the link is also in [[01_Tools]] / Resources, so fee free to delete it from here later)

# Algos
* https://blog.reverberate.org/2020/05/29/hoares-rebuttal-bubble-sorts-comeback.html
* finish Google production thing
* How to color a graph in 4 colors?
* wrong assortativity on different circuits	 - for blog post
* check out "Competitive programming" textbook
* sieve of eratosphenes
* Code a fast 	spanning tree of a graph (using bfs or dfs)
* finish scalability lecture, and watch any other one, from systems design
* splay tree
* read / watch kickstarter competition advice (on their site)
* in a sequence, if any k elements sum to target - code from scratch quickly
* Timsort - reimplement
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
* radix sort
* Regular expressions, including in Python
* Do Bellman-Ford
* refresh and summarize O: https://www.bigocheatsheet.com/
* Code two types of hash-tables from memory
* Scan [this helpful list](https://www.freecodecamp.org/news/coding-interviews-for-dummies-5e048933b82b/); digest into unit-items if needed
* Rotate array in-place with juggling
* treap
* what is NP: definition ([this youtube again?](https://www.youtube.com/watch?v=YX40hbAHx3s))
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

# Python and tools
* f-strings in Python ([ref](https://docs.python.org/3/library/string.html#format-specification-mini-language)): summarize in a separate doc
* How does garbage collection work in Python? What's the logic, and how fast is it? ([link](https://gist.github.com/osavsunenko-ring/205fa72c65d6343eaede0dc43f1c79d4))
* collections - or whatever is this common module ppl often use with prepackaged datastructures
* refresh "read from a console" and read from file in Python
* Command line intro: https://learnpythonthehardway.org/book/appendixa.html
* Lint (pylint) - what is it and why? It seems that it can catch errors, type mismatches etc.
* Keras heckpoints
* Keras hyperparameter optimization
* Python collections: deque, Counter, defaultdict etc.
* Javascript React - how does it look like?
* Python closures, why they are a thing, what's dangerous about them, and how to use them
* bash

# Systems

See also: [[system_design]]

List from Google
* Processes, threads, concurrency
* locks, mutexes, semaphores, monitors
* deadlocks, livelocks, and how to avoid them
* resource allocation
* context switching
* scheduling
* multicore concurrency

Go through 2-3 youtube lectures in the queue that describe some of the existing solutions:
* Scalability lecture: https://www.youtube.com/watch?v=-W9F__D3oY4
(I'm at 24 m right now)
* Scaling Etsy: https://www.youtube.com/watch?v=eenrfm50mXw
* Docker and Kubernetes: https://www.youtube.com/watch?v=u8dW8DrcSmo
* How to design Twitter: https://www.youtube.com/watch?v=KmAyPUv9gOY
* Ruby on rails - what is it, how it works, and why so popular?
* Hadoop vs Spark video: https://hackr.io/blog/hadoop-vs-spark
* Kubernetes and Docker video (30 min): https://www.youtube.com/watch?v=u8dW8DrcSmo
* JSON - how does it work, why everybody love it, and is it the present or the future? ([wiki](https://en.wikipedia.org/wiki/JSON))
* YAML - some standard for keeping stuff that's like XML, but human-readable. Who uses it? Is it popular? ([wiki](https://en.wikipedia.org/wiki/YAML))
* Avro
* Parquet (data storage format)
	* data warehouse technologies
* difference between columnar and row-oriented data stores
* star schema
* difference between dimension and fact tables

* https://www.hiredintech.com/app - a whole intro mini-course on systems design?
* https://www.byte-by-byte.com/3-ways-to-ace-your-system-design-interview/
* Go through the list of terms and write down a definition for each of them
* https://github.com/donnemartin/system-design-primer
    * https://github.com/donnemartin/system-design-primer#index-of-system-design-topics
* https://github.com/checkcheckzz/system-design-interview

Every solution depends on parameters of scale: amount of data, n users, requests per seconds, expected respont time, read/write ratio.

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

**Practicality - web**
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

**Solutions and tools**
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
* Hadoop
* Flink

# Classic ML and related math
* Why autoencoders use KL and not L2 as loss? What's the logic of when KL is used? Older papers, like [[Hinton2006dim]] used cross-entropy. Why?
* Laplacian short video: https://www.youtube.com/watch?v=oEq9ROl9Umk
* https://en.wikipedia.org/wiki/Universal_approximation_theorem
* that way to create ranking from asymmetric preferences via graphs
* Laplacian operator (create a new page for basic vector calc) [1h youtube](https://www.youtube.com/watch?v=oEq9ROl9Umk), [wiki](https://en.wikipedia.org/wiki/Laplace_operator)
* dual bases: why is this term a thing? How does it help? Describe here: [[la_01]]
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
* the difference between tSNE and UMAP
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
* Why drop-out can be considered a type of regularization? What's the intuition, and what's the math?
* Eutoencoders in Keras: https://blog.keras.io/building-autoencoders-in-keras.html
* figure out how to do automatic hyperparameter optimization on Keras
* Why rotate pictures (augmentation) if we can just enforce rotationally-symmetric kernels. Is it possible? Is it desirable?
* What's so eager about eager mode, and why people like it?
* How to tell the receptive field of a neuron in a deep network? Do they gradient descend on it?
* Word2vec - how does it work? And does it come pretrained or what?
* residual blocks - why are they even a good idea? What's the deal here? How can it possibly help?
* Alexnet - some sort of efficient visual net that people use as a module. Is it pre-trained?
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

# ESL progress

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

