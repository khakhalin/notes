# CNN, Convolutional Neural Network, convnets

#dl #convnet

Parent: [[dl]]
See also: [[pooling]], [[residual]]

Subtopics:
* [[vision]] - everything about comparing DL model to brain vision
* [[wavenet]] - a conv-net for sound?

Papers:
* [[alexnet]] - the breakthrough paper from 2012
* [[Bai2018sequence]] - convnets consistently outperfm RNNs in temporal sequence modeling

# To read

Zeiler, M. D., & Fergus, R. (2014, September). Visualizing and understanding convolutional networks. In European conference on computer vision (pp. 818-833). Springer, Cham.
https://cs.nyu.edu/~fergus/papers/zeilerECCV2014.pdf

Huang, G., Liu, Z., Van Der Maaten, L., & Weinberger, K. Q. (2017). Densely connected convolutional networks. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 4700-4708).
https://openaccess.thecvf.com/content_cvpr_2017/papers/Huang_Densely_Connected_Convolutional_CVPR_2017_paper.pdf
14k citations. If I get it right, it's about preventing information loss by introducing some additional "skip" connections between early and late layers in a convnet. That somehow improves stuff?

http://cs231n.github.io/convolutional-networks/ - nice concise introduction to CNNs

Simonyan, K., & Zisserman, A. (2014). Very deep convolutional networks for large-scale image recognition. arXiv preprint arXiv:1409.1556. https://arxiv.org/abs/1409.1556
50k citations. A summary of solutions with tables that compare them. No fun pictures. It it practical, or a just a nominal citation?

A paper from 2014 that fights pooling layers, and claims that pooling is bad, and that in the future we won't use it. What's the current consensus on this? Do we still believe in it, or is it a fringe opinion?
Springenberg, J. T., Dosovitskiy, A., Brox, T., & Riedmiller, M. (2014). Striving for simplicity: The all convolutional net. arXiv preprint arXiv:1412.6806.

# Summary

Images, or other **spatially arranged data** (points that are close to each other share information, or to put it differently, them located near each other is part of information. If reordering preserves data - convnets won't help). By assuming that essentially different parts of each data point (say, image) may be treated as a "nested dataset" with shared patterns and statistics (in this example, visual primitives), we can radically save on network complexity, and thus scale up to richer models without an increase in the number of parameters.

### Convolutional layer

**Convolutional layers** for images are 3D, not 1D as common for "basic" deep networks: we have heights and width (the natural dimensions of an image), but also depth (the coordinates in which we decompose the image, aka **filters**). See [[tensor]]. Sometimes people think of convolutional layers and activation function layers (typically ReLU, see [[activation_functions]]) that sit between them as separate layers, but in Keras for exampel (see [[tensorflow]]) activation function to use on the outputs of a layer is just one of the parameters.

**Hyperparameters** of a single convolutional layer: 
* width & height - for images, typically between 3 and 11; wider early on, narrower later
* output depth (n filters) - typically from 64 to 256; shallower early on, deeper later
* stride (in case not every possible input is considered) - almost always 1, as it's almost always better.
* edge behavior (do we only look at valid inputs, or do we extend the input with **padding**). Usually people use zero-padding, as it improves performance.

There's also a version of conv-layer light, so to say, that doesn't share parameters, called a **locally connected layer**. So instead of all-to-all, we still have only each neighborhood projecting forward, but weights are not shared. Apparently useful for things like centered faces, where filters for an eye and for the mouth are likely to be different, but the expense of translational invariance. _Do people actually use it? Immediate googling links seem a bit dated, so I'm not sure it's a common method._

1x1 convolutions are also possible: essentially a dense layer on each depth-vector, but without looking at neighboring vectors.

**Dilated convolution**: instead of looking at a narrow neighborhood look at at points with increasing steps, like i+1, i+3, i+7. May help funneling of information that is otherwise all delegated to pooling (below). _But it looks weird, no? Wouldn't there be artifacts? Wouldn't pooling + skipping layers (as in [[residual]]) to preserve local info be preferable?_

It is possible to convert back and forth between fully connected and convolutional layers: for "conv→full" we just get some sort of a block-like matrix, while with "full→conv" we just get a trivial case of 1 input and very deep output. The reason why even this stupid operation may be useful in practice, is that you can train a network on a small image, and then use this network to look for this type of objects in a larger image, by treating these "locally trained fully connected areas" as convolutions. Apparently people do it in practice.

### Pooling

Convolutional layers don't change the dimensions of the signal (output == input, except for maybe slight reduction at edges, but usually not even that because of padding). They just translate it from the language of one set of filters to another (one can think of it as a "spatially nested" dense layer working on depth vector, but also looking at nearby depth vectors). But as neighboring points  share information, a transformation by a convolutional layer is hightly redundant (too much information is shared between neighboring depth-vectors), and can be compressed (something like downsampling, but smarter). This is achieved by **pooling layers**, typically - **max pooling**. See [[pooling]]. 

Pooling layers don't have trainable parameters, but has hyperparameters that may be optimized (width, stride). Most commonly used pooling layers: width=3, stride=2 (aka "overlapping pooling"), and width=2, stride=2 (most common type of pooling). Wider max-pools are usually too destructive.

In the past, average-pooling was common, but fell out of favor, as max pooling works better in practice. _If I get it right, the use of maxpooling should also favor sparse coding, so that max(feature) made sense. So it probably works better in combination with deep tensors, and may even be a motivation behind these really deep convnet outputs with ~256 filters. Right?_

Some people hate pooling, and believe that in the future we'll get rid of it. See for example "All convolutional net" (Springenberg 2014). Apparently, pooling layers are also not used in autoencoders and generative models (see [[autoencoder]], [[gan]]) - is it true?

Footnotes:
* Springenberg, J. T., Dosovitskiy, A., Brox, T., & Riedmiller, M. (2014). Striving for simplicity: The all convolutional net. arXiv preprint arXiv:1412.6806.

### Other layers and overall architecture

Towards the end we always have a few **fully connected** (aka **dense**) layers, and maybe a softmax ([[softmax]])  at the very end, in case of classification.

Overall, a stack of small convolutions is better than one wide convolution with the same number of parameters. Network typically look like a funnel (see [[alexnet]] for example).

A recent development to keep the benefits of pooling without its downsides: creating parallel flow of information for local features, and later re-mix with with pooled global features; **Inception** and **Residual Networks**: see [[residual]].

### Limitations

IRL signals, including images, come in different shapes, but convnets (similar to other DLL networks) want all images to be scaled similarly. Convolutional layers actually don't care, and can process input of any shape, but towards the end, as we transition from them to dense layers, we have to arrive to a certain particular dimensions of an input.

> How to deal with it in practice?

# General Refs

Really cool intro lecture from Brandon Rohrer: https://www.youtube.com/watch?v=JB8T_zN7ZC0

Nice textbook-like chapter from Stanford: https://cs231n.github.io/convolutional-networks/

One of the very first papers about convnets, with 30k citations by now. Interestingly, it explicitly mentions Hubel & Wiesel (mammalian vision) as the inspiration for the approach:
LeCun, Y., Bottou, L., Bengio, Y., & Haffner, P. (1998). Gradient-based learning applied to document recognition. Proceedings of the IEEE, 86(11), 2278-2324. http://www.iro.umontreal.ca/~lisa/bib/pub_subject/finance/pointeurs/lecun-98.pdf