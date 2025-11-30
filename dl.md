# Deep learning

Core ideas:
* [[backprop]] - backpropagation
* [[dl_conceptualized]] - meanings and limitations behind DL
* [[ticket]] (stub for now) - the lottery ticket theory

Opimizers and gradient descent methods:
* [[sgd]] - the main concept
* [[Ruder2016descent]] - overview of GD methods
* [[adam]] - most popular optimizer

Other aspects:
* [[loss]] - different loss functions
* [[activation_functions]] - from ReLU to fancy
* [[hyperparameters]] - learning rate, batch size, and how to tune them* 
* [[regularization]] - a collection of methods
* [[dropout]] - subj
* [[batch_normalization]] - another useful approach
* [[init_dl]] - how to init DL models

Architectures
* [[convnet]] - Convolutional Neural Networks
* [[transformers]] - Attention
* [[rnn]] - Recurrent networks
* [[graphs]] - Graphical networks

Layer types
* [[pooling]] - Especially important for CNNs
* [[softmax]] - Last layer in classifiers (or probability estimators)
* Layer norm - makes sure (via learning) that on average the  previous layer reports m=0 & σ=1
* Batch norm - similar but faster & approximate, as normalizes within batch (no learned params). However in TF by default it is used with `trainable=True`, which means that it learns to normalize on average, not just within each given batch. See [[batch_normalization]].

Tricks
* [[augmentation]] - data augmentation
* [[smote]] - one simple type of synthetic data
* [[curriculum]] learning

Practical tools:
* [[tensorfow]] - main tool from Google
* [[ml_lore]] - list of practical ML and DL advice


# Refs

**Reviews**

LeCun, Y., Bengio, Y., & Hinton, G. (2015). Deep learning. nature, 521(7553), 436-444.
https://www.nature.com/articles/nature14539
34k citations.

Schmidhuber, J. (2015). Deep learning in neural networks: An overview. Neural networks, 61, 85-117.
http://www.jics.utk.edu/files/images/csure-reu/2015/Tutorial/DNN/DNN-overview.pdf
11k citations.
