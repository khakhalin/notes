# RandAugment

Cubuk, E. D., Zoph, B., Shlens, J., & Le, Q. V. (2019). RandAugment: Practical data augmentation with no separate search. arXiv preprint arXiv:1909.13719.
https://arxiv.org/abs/1909.13719

#augmentation

[Video summary from HenryLabs](https://www.youtube.com/watch?v=Zzt9i3gDueE).

The key part seems to be that it automatically finds a sequence of augmentations to boost your accuracy when learning on an image dataset. And it does it computationally cheaper than some other competing solutions (some thing called AutoAugment?), and has fewer hyperparameters (two). Essentially, they thought of it as a sort of #curriculum learning, as you want to start with small amplitudes, and then gradually increase them. But then they show that actually this curriculum doesn't matter at all, and you can as well always use const amplitude, or random amplitude; you get exactly same final results.

I didn't understand the story about the "small subnetwork" that they need to find something something? Not sure.

What I did get, and liked, is a part where they look at the effect of adding more, and different transformations, on learning quality. Shear (y) and translate are good, but rotation is the key! The most informative one. Simple changes of color (contrast, brightness) don't matter, while things like solarize and posterize actually _decrease_ performance. In terms of the number of different transformations, rotations also saturate really fast, while with cuts and translations you need to get dozens of those for every image before you reach same performance.