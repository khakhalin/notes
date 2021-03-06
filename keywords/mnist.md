# MNIST

#tools #benchmarks

Related:
* [[communicating_networks]] - can we evolve mnist from basic principles?
* [[Schneider2019benchmark]] - a review of practical benchmarks

A famous database. Included as a standard dataset in both scikit-learn, and tensorflow. Comes already split into training and testing sets (60000 and 10000 examples, respectively). It seems that to keep everything comparable, it is not common to change testing split for this set.

# Links and Ideas

GAN examples, generating inputs for a MNIST classifier:
* https://www.tensorflow.org/tutorials/generative/dcgan
* https://towardsdatascience.com/demystifying-gans-in-tensorflow-2-0-9890834ab3d9 (only visible once a month, due to paywall)
    * Notebook: https://github.com/MonteChristo46/GAN-Notebooks/blob/master/GAN.ipynb

# Core Refs

A zoo of all thinkable approaches to classification, applied to this dataset, each shown with a reference and a test error rate: http://yann.lecun.com/exdb/mnist/index.html

A similar collection of best results, achieved with different methods: https://paperswithcode.com/sota/image-classification-on-mnist

The current SOTA is about 0.2% error rate (1-accuracy).

> One could wonder to what extent current SOTA aproaches are over-optimized for this testing set particularly. Is there a way to target-overfit here? I'm sure there could be a theoretical estimation for that.