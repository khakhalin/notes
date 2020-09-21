# Perceptual loss

#image #loss

Parent: [[bib_image]]
Related:
* [[bib_generate]]
* [[style_transfer]]
* [[Johnson2016perceptual]] - original paper

A more suitable loss for comparing images. Motivation: if you use something like an L2 loss, you will have high losses even for pairs of images that are undistinguishable to a human (say, the same pic shifted by one pixel). Because of that, **per-pixel** losses are not likely to work well, and it's better to compare images using a special pre-trained **loss network**. Examples when perceptual loss is indispensible: **super-resolution**, and **style transfer** (any pixel-level loss would go crazy).

The idea is to train a network that would encode **relevant features** of an image, then use ΣΔϕ² in the latent space of this model to compare images. How to train this model exactly is another question.

* [[Johnson2016perceptual]] just train a classifier on ImageNet, then use early layers
* ??? other approaches ??? Scholar seem to suggest that they exist...

# Refs

Deep AI glossary:
https://deepai.org/machine-learning-glossary-and-terms/perceptual-loss-function

