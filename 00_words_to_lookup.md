# General To-Dos

Key tags:
* #halfthere - for projects that are close to completion.
* #todo - for follow-ups that need to be done 

Videos
* Conway, the free will theorem: https://www.youtube.com/watch?v=ftIllWczf5w
* Laplace operator: https://www.youtube.com/watch?v=oEq9ROl9Umk

See also: [[system_design]] - has it own "todo" list of words and terms

Google crash course, about ML production:
https://developers.google.com/machine-learning/crash-course/production-ml-systems

What is a Computation Graph and Who Cares?
https://end-to-end-machine-learning.teachable.com/courses/advanced-neural-network-methods/lectures/12194418

https://medium.com/dair-ai/an-illustrated-guide-to-graph-neural-networks-d5564a551783

Music: https://www.youtube.com/watch?v=7NOSDKb0HlU
Kick-start videos: https://www.youtube.com/playlist?list=PLllx_3tLoo4csmLveWHpjcRTXVMCcvvmc

# Words and topics to look up

**ML and Algos**
* [[red-black_tree]] red-black_tree - document and implement
* queens on a board
* in a sequence, if any k elements sum to target 
* Timsort - reimplement
* how to solve sudoku?
* https://www.youtube.com/watch?v=oEq9ROl9Umk
* Minimal spanning tree on a lennon
* Relax trees to ribbons, do it on a lennon
* How does garbage collection work in Python? What's the logic, and how fast is it? ([link](https://gist.github.com/osavsunenko-ring/205fa72c65d6343eaede0dc43f1c79d4))
* Re-implement [[avl_tree]]: make it neater, and fix delete (it's wrong now I think). Make everything very recurrent. Can we avoid storing parents?
* string sort from the red book
* scrum
* finish Google production thing
* splay tree
* radix sort
* Do Bellman-Ford
* collections - or whatever is this common module ppl often use with prepackaged datastructures
* refresh O: https://www.bigocheatsheet.com/
* Code two types of hash-tables from memory
* Look up at all keywords from the systems part, put them in the dictionary
* Scan [this helpful list](https://www.freecodecamp.org/news/coding-interviews-for-dummies-5e048933b82b/); digest into unit-items if needed
* Rotate array in-place with juggling
* treap
* Ruby on rails - what is it, how it works, and why so popular?
* finish scalability lecture, and watch any other one, from systems design
* what is NP: definition ([this youtube again?](https://www.youtube.com/watch?v=YX40hbAHx3s))
* Bloom filter (refs: [1](https://dominikschmidt.xyz/bloom-filter/))
* summarize [Python sets](https://docs.python.org/2/library/sets.html)
* Make sure [this list from geekforgeeks](https://www.geeksforgeeks.org/top-10-algorithms-in-interview-questions/) is exhausted
* implement Dijsktra
* collision-detection algorithms
* https://en.wikipedia.org/wiki/Universal_approximation_theorem
* refresh "read from a console" and read from file in Python
* Command line intro: https://learnpythonthehardway.org/book/appendixa.html
* https://www.geeksforgeeks.org/binary-search-tree-set-2-delete/
* Go through all subtopics here:
https://www.geeksforgeeks.org/binary-search-tree-data-structure/
* https://towardsdatascience.com/8-useful-tree-data-structures-worth-knowing-8532c7231e8c
* https://en.wikipedia.org/wiki/Fibonacci_heap
* https://en.wikipedia.org/wiki/Pairing_heap
* minimal spanning tree - code
* Regular expressions, including in Python
* that way to create ranking from asymmetric preferences via graphs
* https://cp-algorithms.com/data_structures/fenwick.html
* https://cp-algorithms.com/data_structures/segment_tree.html
* Revisit [this link](https://cs.stackexchange.com/questions/111353/analyze-time-series-data-to-get-min-max-over-past-period-of-time) about keeping running max & min of a time series (monotonous wedge?) - describe it, code it. [Pdf of the paper](https://arxiv.org/pdf/cs/0610046.pdf).
* Laplacian operator (create a new page for basic vector calc) [1h youtube](https://www.youtube.com/watch?v=oEq9ROl9Umk), [wiki](https://en.wikipedia.org/wiki/Laplace_operator)
* Convex hull: [wrapping algorithm](https://www.geeksforgeeks.org/convex-hull-set-1-jarviss-algorithm-or-wrapping/)
* Why autoencoders use KL and not L2 as loss? What's the logic of when KL is used?
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
* Can I code a line graph algorithm? Refs: [wolfram](https://mathworld.wolfram.com/LineGraph.html), [wiki](https://en.wikipedia.org/wiki/Line_graph), [overflow](https://stackoverflow.com/questions/13700954/graph-transformation-vertices-into-edges-and-edges-into-vertices)

**Open Python questions**
* should nodes self-delete and not just plug references with Nones?

**Tools**
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

