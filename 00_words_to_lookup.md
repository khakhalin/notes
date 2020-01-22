# General To-Dos

#todo
Current point in the Google Course: https://developers.google.com/machine-learning/crash-course/production-ml-systems 

ESL progress. 
Current position: p284

Parked parts that need to be revisited:
* p161 to p218 - Smoothing, wavelets, smoothing kernels, local regression, kernel size choice
* p219 - a whole chapter on Model selection, Bias-Variance Tradeoff, effective number of parameters, cross-validation, and bootstrapping
* p261 - another whole chapter. Model averaging, max likelihood, some Bayesian inference, more bootstrapping, the EM algorithm (Expectation-Maximization), Gibbs samping, bagging, MCMC

# Words and topics to look up

* Move old recommendatinos from Sven down here
* Make sure all Eric's recommendations are here

Classic ML and friends
* purity score
* random forest
* gradient boosting
* loess - how is it different form b-splines?
* Hungarian algorithm
* Bloom filter
* Kalman filter (and its friends)
* Expectation-Maximization
* Bayesian PCA
* more on Gaussian processes
* Graphical models
* Hidden markov models
* Markov random fields (precursor of modern graph models?)
* Variational inference
* Resampling techniques: SMOTE, isotonic regression, Platt scaling
* Principal variables

Practical stuff:
* Checkpoints
* Keras hyperparameter optimization

Deep Learning:
* What's so eager about eager mode
* How to tell the receptive field of a neuron in a deep network? Do they gradient descend on it?
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

Technical stuff
* Relational databases: Snowflake, Redshift
* regex in Python
* JSON - how does it work, why everybody love it, and is it the present or the future? ([wiki](https://en.wikipedia.org/wiki/JSON))
* YAML - some standard for keeping stuff that's like XML, but human-readable. Who uses it? Is it popular? ([wiki](https://en.wikipedia.org/wiki/YAML))
* bash
* Hadoop
* Spark
* Python closures, why they are a thing, what's dangerous about them, and how to use them
* Jax: [Baysian NN in Jax](https://colab.research.google.com/drive/1gMAXn123Pm58_NcRldjSuGYkbrXTUiN2) - collab notebook

Other math
* How to calculate single-value-decomposion and eigenvector decomposition in practice? How do they code it for numpy, for example?
* FFT
* Lagrange multiplyier - is there an easy (not formal, but intuitive) proof that it works?
* Hamiltonians and how dynamical systems invoke them
* Perron–Frobenius theorem - something related to centralities and or network graph eigenvectors
* kuramoto oscillations

Neuro
* ...
