# Feature Visualization

Olah, C., Mordvintsev, A., & Schubert, L. (2017). Feature visualization. Distill, 2(11), e7.
https://distill.pub/2017/feature-visualization/

#image #interpretability #diversity

Related: [[interpretability]], [[illusions]], [[superstimulus]]
* Follow-up: Olah2018interpretability

Really pretty and weird pictures in this one.

For image-classification, let's try to find images that excite different feature detectors the most. Because networks are differentiable in respect to their inputs, we can use gradient descent to optimize inputs.

One can optimize for:
* Response of 1 neuron - like their contribution.
* One channel (a certain depth across the depth dimension in latent representation)
* Entire layer (except in this case it's not ∑activations, but ∑activations²) - That's like "What makes this network interested? What looks non-random to it?". Apparently that's how **DeepDream** works (?)
* pre-softmax classifier outputs (most dog)
* post-softmax class probability (most dog and most not anything else at the same time)

For those activations that have a sign, we can also compare "best activators" to "strongest anti-activators".

> The images they show are a great illustration that actually direct optimization for a layer are extremely hard for a human to interpret, while a collection of "good" and "bad" images do totally make sense. As classifiers don't learn the constraints within an image, it is really hard to relate to their preferences, when they are presented in an unconstrainted way.

A way to go even deeper: optimize for several examples at once (generate super-stimuli stimuli), but also try to make these several examples as diverse as possible (and start with different inits for optimizing). For some neurons, creates pretty pictures that are indeed more human-relateable, but for some neurons - shows that they respond to more than one category (they show an example of a fox+car neuron).

Some of the random directions in the latent activation space are also interpretable, but not all? _This seems obvious, no?_

There is some semantic arythmetic (similar to how they have it for [[word2vec]])

# How to optimize?

Just optimizing inputs doesn't work, as you generate an **optical illusion**: a weird noise-like high-frequency image that produces high activation, but is totally not helpful. Like an #adversarial example. So we need some #regularization. 

_They then have a really cool summary of previous attempts at regularization_

Two winners in terms of interpretability seem to be #gan (make images generated instead of directly optimized; _do they gradient-descent the latent representation or something?_) and **Transformation robustness**, perhaps together with high-frequency penalization. **Transformation robustness** means that the signal still activates the network strongly even if it is slightly perturbed via stochastic jitter, rotation, and scaling. Like some sort of a few-pixels-scale probabilistic spatial invariance.

Next idea: maybe optimize not in the space of images, but in a different basis, like a Fourier basis? This won't change the position of minima, but the direction of the gradient will be different (aka **gradient preconditioning**) and it helps a lot to avoid "bad" minima, and find "good" ones.

And then they have a huge browser with visualization of all layers:
* https://distill.pub/2017/feature-visualization/appendix/

and an open-source code of their tool.