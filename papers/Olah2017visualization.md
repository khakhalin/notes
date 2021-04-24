# Feature Visualization

Olah, C., Mordvintsev, A., & Schubert, L. (2017). Feature visualization. Distill, 2(11), e7.
https://distill.pub/2017/feature-visualization/

Related: [[interpretability]], [[illusions]], [[superstimulus]]
* Follow-up: [[Olah2018interpretability]]

#image #interpretability #diversity


Really pretty and weird pictures in this one.
(See also a huge visualization in Appendix1: https://distill.pub/2017/feature-visualization/appendix/ )

For image-classification, let's try to find images that excite different feature detectors the most. Because networks are differentiable in respect to their inputs, we can use gradient descent to optimize inputs.

One can optimize for:
* **Response of 1 neuron** - shows their contribution. Typically looks like a pattern bound by a receptive field, and fading into nothing.
* **One filter** (a certain depth across the depth dimension in [[convnet]] latent representation) - typically looks like a funny pattern covering the entire field
* **Entire layer** (except in this case it's not ∑activations, but ∑activations²) - That's like "What makes this network interested? What looks non-random to it?". Apparently that's how **DeepDream** works (?) Looks like psychedelic art
* **pre-softmax classifier outputs** (most "dog" picture ever)
* **post-softmax class probability** (most "dog" and most "not anything else" at the same time)

For those activations that have a sign, we can also compare "best activators" to "strongest anti-activators".

> The images they show are a great illustration that actually direct optimization for a layer are quite hard for a human to interpret, while collections of "good" and "bad" images do make sense. As classifiers don't learn the constraints within an image, it is hard to relate to their preferences, when they are presented in this unconstrainted psychedelic delirious way.

A way to go even deeper: optimize for several examples at once (generate a [[superstimulus]]), but also try to make these several examples as diverse as possible (and start with different inits for optimizing). For some neurons, creates pretty pictures that are indeed more human-relateable, but for some neurons - shows that they respond to more than one category (they show an example of a fox+car neuron).

Other notes:
* Some of the random directions in the latent activation space are also interpretable, but not all? _This seems obvious, no?_
* Joint optimization for 2 neurons, when compared to optimization for each of them separately, may help to understand how they interact.
* Some semantic arithmetic is happening here (similar to how they have it for [[word2vec]] and other semantic [[embedding]]s)

# How to optimize?

Just optimizing inputs doesn't work, as you end up generating an #adversarial example: a weird noise-like high-frequency image that produces high activation, but is totally not helpful. So we need to use some #regularization. 

They then have a really cool summary of previous attempts at regularization (with references and sample pics):
* **Penalization of high frequencies**
* **Transformation robustness**: jitter rotation, and scale. Even small stochasticity here cancels high-frequency adversarial noise.
* **Learned priors:** it's like searching in a dataset instead of a direct visualization (finding a real picture that activates each neuron most strongly), except instead of a real dataset, we'll sample from a constained space of "inputs that look kinda like images". In practice you can use either a [[gan]], or an [[autoencoder]] to generate these inputs. And the "realism" criteria for these generative models are lower than in a standard generation case (which is great!), as after all we don't want a realistic image; we just want an image that has somewhat reasonable statistical properties.

Two winners in terms of interpretability seem to be #gan (do they gradient-descent the latent representation or something? That would be the most logical option here, but I'm not sure) and **Transformation robustness**, perhaps together with a bit of high-frequency penalization.

Next idea: maybe optimize not in the space of images, but in a different basis, like a Fourier basis? This won't change the position of minima, but the direction of the gradient will be different (aka **gradient preconditioning**). It also helps a lot to avoid "bad" minima, and find "good" ones.

The tool is open-sourced (see appendices).