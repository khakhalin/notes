# Alexnet

Krizhevsky, A., Sutskever, I., & Hinton, G. E. (2012). Imagenet classification with deep convolutional neural networks. Advances in neural information processing systems, 25, 1097-1105.
https://kr.nvidia.com/content/tesla/pdf/machine-learning/imagenet-classification-with-deep-convolutional-nn.pdf

Parent: [[convnet]]
See also: [[activation_functions]]

#convnet #vision #image

The famous revolutionary paper on convnets that became a SOTA for years, and started a new era. As of 2021, 80k citations!

Their breakthroughs:
* Switched to ReLUs from tanh (see [[activation_functions]]), as it trains so much faster
* Trained in parallel on several GPUs
* batch normalization
* Convnets with overlapping pooling
* Dropout p=0.5, which was young at the time

ImageNet dataset (15 million images; introduced in 2010).

**Architecture**: 8 trained layers: 5 convolutional and 3 dense:
* Input: 224×224×rgb
* 1. Convolution: 11×11×3 , stride 4 → 96 kernels (filters). Output: 55×55×96 block.
* Batch-normalize, max pool 2×2	 stride 2 ([[pooling]]). Output: 27×27×96.
* 2. Convolution: 5×5 → 256 filters. Output: 27×27×256 block.
* Batch-normalize, max pool 2×2 stride 2. Output: 13×13×256.
* 3. Convolution 3×3 → 384 filters. Output: 13×13×256.
* 4. Convolution 3×3 → 384 filters. Output: 13×13×192.
* 5. Convolution 3×3 → 256 filters. Output: 13×13×192.
* Dense layer of 4096 neurons
* Dense layer of 4096 neurons
* Output layer of n_categories neurons (softmax)

Actually they also split these filter banks, so for example 2nd convolution wasn't taking an input of 5×5×96 as one could have guessed, but only 5×5×48, and the other 48 kernels from the 1st convolution layer were served to a different convolution. But that seems to be for performance only, and later reimplementations of this model (like a Keras version references below for example) seem to ignore this nuance.

Comments:
* Not sure how they got 55 from 224. My understanding is that if you have a width of w (that is, w/2 on both sides from your "central point"), and a stride of s, which results in k different "snapshots" being taken, then the total size of the original layer $n = (w-1)/2 + (1 + (k-1)s) + (w-1)/2 = w + (k-1)s$ . From this, we would have $k = 1 + (n-w)/s$. But 224-11 = 213, and it doesn't divide by 4. The only way to get 55 from 224 is to divide by 4 and subtract one. Not sure what's going on, but probably that's how they understand "padding=same". My formula is strictly speaking "padding=valid".
* There seems to be a disagreement between the text and the image, in terms of where exactly max-pool layers go.

Reached accuracy of ~65%, which was SOTA for a year. (As of 2021 it's about 90%: https://paperswithcode.com/sota/image-classification-on-imagenet )

# Refs

Implementation in Keras:
https://analyticsindiamag.com/hands-on-guide-to-implementing-alexnet-with-keras-for-multi-class-image-classification/