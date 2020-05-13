# Self-Supervised Representation Learning

Nov 10, 2019 by Lilian Weng 
https://lilianweng.github.io/lil-log/2019/11/10/self-supervised-learning.html
 
#blog #self-supervised #bib

Awesome informal review of the entire concept of self-supervized learning, with highlights from the field.

**General idea:** Come up with a useless supervised task derived from data (use any part of data as a label, predict the label from the rest). By doing that, the model will have to learn rich representations. Then use intermediate level (representation) as an input to actual your actual model.

**Bibliography:**

* Take a small image, distort and transform it in various ways, teach the model to know that all these images still belong to the same class:  [Dosovitskiy et al., 2015](https://arxiv.org/abs/1406.6909)
* Rotate images, let the model guess down direction:  [Gidaris et al. 2018](https://arxiv.org/abs/1803.07728) 
* Get a bunch of patches, let it guess their relative position:  [Doersch et al. (2015)](https://arxiv.org/abs/1505.05192) Comes with pathologies: obvious straight lines, chromatic aberration (!!!) towards the sides of each photo. To kill first, distort and jitter; for the second - drop colors, or at least shift towards BW.
* Similar idea: reshuffle patches, let the model guess the original order:   [Noroozi & Favaro (2016)](https://arxiv.org/abs/1603.09246) - also use some permutation-invariant graph network that I don't get for now.
* Colorization:  [Zhang et al. 2016](https://arxiv.org/abs/1603.08511) Some interesting rebalancing of the loss function to make sure the model cares about rare but important colors, as opposed to just the sky and the leaves. Not sure how it works (read?)
* Predict one color channel from two others:  [Zhang et al., 2017](https://arxiv.org/abs/1611.09842) 
* Remove (previously added) noise:  [Vincent, et al, 2008](https://www.cs.toronto.edu/~larocheh/publications/icml-2008-denoising-autoencoders.pdf) 
* Reconstruct a patch in the middle:  [Pathak, et al., 2016](https://arxiv.org/abs/1604.07379) 
* Bidirectional GAN: generate data from internal features, but also generate features from actual data. Let the discriminator guess which pair (features, data) is which (one has true data (image), another one doesn't). Thus, at the end, we'll have a bidirectional projection:  [Donahue, et al, 2017](https://arxiv.org/abs/1605.09782)  (I'm not sure how it helps exactly if encoder and decoder don't inform each other explicitly. 
* Tracking objects: [Wang & Gupta, 2015](https://arxiv.org/abs/1505.00687)  - I didn't understand this one. One interesting thing that I got (but that's for supervised learning) is the [[triplet_loss]]
* Guess frame order:  [Misra, et al 2016](https://arxiv.org/abs/1603.08561) , one sequence of out many that has frames shuffled:   [Fernando et al. 2017](https://arxiv.org/abs/1611.06646) , or correct time direction:  [Wei et al., 2018](https://www.robots.ox.ac.uk/~vgg/publications/2018/Wei18/wei18.pdf) 
* Colorize a video from a control frame:  [Vondrick et al. (2018)](https://arxiv.org/abs/1806.09594)  - sounds like something in-between image colorization and motion tracking, huh?
* Let the robot move an object, and while doing so, learn that all camera views during the grasp encode the same object. Even though positions and views are all different. Did I get it right?  [Jang et al., 2018](https://arxiv.org/abs/1811.06964) 
* Teach that views from multiple cameras are still views of the same thing:   [Sermanet, et al. 2018](https://arxiv.org/abs/1704.06888) 
* Imagined goals: [Nair et al., 2018](https://arxiv.org/abs/1807.04742)  - didn't understand this one; something very robotics