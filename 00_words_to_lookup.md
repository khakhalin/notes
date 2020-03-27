# General To-Dos
#todo

Google crash course, about ML production: #todo
https://developers.google.com/machine-learning/crash-course/production-ml-systems

practical intro to ML projects; look through:
https://github.com/chiphuyen/machine-learning-systems-design

Matrix calculus for deep learning
https://explained.ai/matrix-calculus/index.html

Introduction to probabilistic computation:
https://betanalpha.github.io/assets/case_studies/probabilistic_computation.html
Seems to be very good; simultaneously visual and mathy. Stochastic estimators and what not. Must-read!

Good collection of short tutorials (both ML and snippets of Python code) by Chris Albon:
https://chrisalbon.com/
Look through, decide the priority; how to file.

# Words and topics to look up

#todo: [[system_design]] - sorta priority

**ML and Algos**
* finish Google production thing
* finish AVL tree
* implement AVL tree
* red-black tree
* trie
* repeat heap, now with deletion
* implement Dijsktra
* Command line intro: https://learnpythonthehardway.org/book/appendixa.html
* https://www.geeksforgeeks.org/binary-search-tree-set-2-delete/
* Go through all subtopics here:
https://www.geeksforgeeks.org/binary-search-tree-data-structure/
* minimal spanning trees
* https://towardsdatascience.com/8-useful-tree-data-structures-worth-knowing-8532c7231e8c
* https://en.wikipedia.org/wiki/Fibonacci_heap
* https://en.wikipedia.org/wiki/Pairing_heap
* what is NP: definition
* splay tree
* radix sort
* that way to create ranking from asymmetric preferences via graphs
* https://cp-algorithms.com/data_structures/fenwick.html
* https://cp-algorithms.com/data_structures/segment_tree.html
* finish gradient boosting
* finish SVM
* random forest
* random forest vs. adaboost?
* apply svm, boosting
* normal equation ([link?](http://mlwiki.org/index.php/Normal_Equation#Normal_Equation_vs_Gradient_Descent))
* implement from scratch in numpy:
    * small DL
    * SVM
    * random forest
* that visual blog post about types of DL optimizers
* Capsule neural network - some sort of generalization of convolutional networks for hierarchical data?? Never heard of them, but they are actually quite well cited!!
* the difference between tSNE and UMAP
* Kalman filter
* HyperNEAT - a paradigm for generative network evolution
* graph cuts in computer vision
* Latent Dirichlet Allocation ([ref](https://towardsdatascience.com/end-to-end-topic-modeling-in-python-latent-dirichlet-allocation-lda-35ce4ed6b3e0))
* unit tests, pytests, mocking
* Bayesian PCA
* Bloom filter (refs: [1](https://dominikschmidt.xyz/bloom-filter/))
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

**Systems and OS**
* Processes, threads, concurrency
* locks, mutexes, semaphores, monitors
* deadlocks, livelocks, and how to avoid them
* resource allocation
* context switching
* scheduling
* multicore concurrency
* how internet works (TCP, IP, DNS, HTTP)

**Tools**
* Python Decorators: in what scope do these variables live? ([link for memoization](https://www.python-course.eu/python3_memoization.php))
* Lint (pylint) - what is it and why? It seems that it can catch errors, type mismatches etc.
* Keras heckpoints
* Keras hyperparameter optimization
* Python collections: deque, Counter, defaultdict etc.
* Javascript React

**Algorithms - other**
* https://www.byte-by-byte.com/google-interview/ - check other topics
* Hungarian algorithm
* minimum cut (on graphs), with related topics

**ESL progress**
Current position: p290

Parked parts of ESL that need to be revisited:
* p161 to p218 - Smoothing, wavelets, smoothing kernels, local regression, kernel size choice
* p219 - a whole chapter on Model selection, Bias-Variance Tradeoff, effective number of parameters, cross-validation, and bootstrapping
* p261 - another whole chapter. Model averaging, max likelihood, some Bayesian inference, more bootstrapping, the EM algorithm (Expectation-Maximization), Gibbs samping, bagging, MCMC

**Deep Learning:**
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
* Xavier initialization of weights - what's cool about it? Also, is it true that variance-scaled init is better? ([claim](https://pcc.cs.byu.edu/2017/10/02/practical-advice-for-building-deep-neural-networks/))
* inception blocks
* noise-contrastive estimation
* self-supervised boosting
* Wavenet - that famous convolution network that generates audio on-the-fly

**Technical stuff**
* Hadoop vs Spark video: https://hackr.io/blog/hadoop-vs-spark
* Kubernetes and Docker video (30 min): https://www.youtube.com/watch?v=u8dW8DrcSmo
* f-strings in Python ([ref](https://docs.python.org/3/library/string.html#format-specification-mini-language)): summarize in a separate doc
* sets in Python: are they worth it?
* regex in Python
* JSON - how does it work, why everybody love it, and is it the present or the future? ([wiki](https://en.wikipedia.org/wiki/JSON))
* YAML - some standard for keeping stuff that's like XML, but human-readable. Who uses it? Is it popular? ([wiki](https://en.wikipedia.org/wiki/YAML))
* bash
* Python closures, why they are a thing, what's dangerous about them, and how to use them
* Jax: [Baysian NN in Jax](https://colab.research.google.com/drive/1gMAXn123Pm58_NcRldjSuGYkbrXTUiN2) - collab notebook 

**Other math**
* How to calculate single-value-decomposion and eigenvector decomposition in practice? How do they code it for numpy, for example?
* FFT
* Lagrange multiplyier - is there an easy (not formal, but intuitive) proof that it works?
* Hamiltonians and how dynamical systems invoke them
* Perron–Frobenius theorem - something related to centralities and or network graph eigenvectors
* kuramoto oscillations

