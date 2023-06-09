# RandAugment

Cubuk, E. D., Zoph, B., Shlens, J., & Le, Q. V. (2019). RandAugment: Practical data augmentation with no separate search. arXiv preprint arXiv:1909.13719.
https://arxiv.org/abs/1909.13719

Parent: [[augmentation]]
See also: [[Zoph2020pretraining]] - show how augment interacts with pre-training

#augmentation


The key part seems to be that it automatically finds a sequence of image augmentations to boost your accuracy when learning on an image dataset. And it does it computationally cheaper than their previous solution called **AutoAugment**, and has fewer hyperparameters (two). Originally they thought of it as a sort of #curriculum learning, as you want to start with small augmentation amplitudes, and then gradually increase them. But then they realized that actually this curriculum doesn't matter at all, and you can as well always use const amplitude, or random amplitude; you get exactly same final results.

I didn't understand the story about some "small subnetwork" that they need to find something something? Not sure.

What I did get, and liked, is a part where they look at the effect of adding more, and different transformations, on learning quality. Shear (y) and translate are OK, but rotation is the key! Rotation  is the most informative type of augmentation. On the contrary, in turns out that simple changes of color (contrast, brightness) don't matter, while things like solarize and posterize actually _harm_ performance. In terms of the number of different transformations, rotations also saturate really fast (relatively few rotations bring you most of the way to the full effect), while with cuts and translations you need to get dozens of those for every image before you reach similar performance.

# Refs

Video summary from HenryLabs: https://www.youtube.com/watch?v=Zzt9i3gDueE

Their previous paper on **AutoAugment**:
Cubuk, E. D., Zoph, B., Mane, D., Vasudevan, V., & Le, Q. V. (2019). Autoaugment: Learning augmentation strategies from data. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 113-123).

At least one paper [[Zoph2020pretraining]] seems to be using Auto-Augment as essentially a weaker version of RandAugment, but as the authors are practially the same, I'm not sure it's a meaningful practice. Seems more likely that they just could easily run it on two prior solutions, so they did it.