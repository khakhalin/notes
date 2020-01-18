# MNIST
#tools

A famous database. Included as a standard dataset in both scikit-learn, and tensorflow. Comes already split into training and testing sets (60000 and 10000 examples, respectively). It seems that to keep everything comparable, it is not common to change testing split for this set.

Links:
* A zoo of all thinkable approaches to classification, applied to this dataset, each shown with a reference and a test error rate: http://yann.lecun.com/exdb/mnist/index.html
* A similar collection of best results, achieved with different methods: https://paperswithcode.com/sota/image-classification-on-mnist

The current SOTA is about 0.2% error rate (1-accuracy).

> One could wonder to what extent current SOTA aproaches are over-optimized for this testing set particularly. Is there a way to target-overfit here? I'm sure there could be a theoretical estimation for that.