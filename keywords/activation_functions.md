# Activation Functions for DL

#dl #bib

Parent: [[06_DL]]
See also: [[regularization]], [[hyperparameters]]

See also (special types of units):
* [[lstm]] - For RNs, Long-Short-Term-Memory
* [[gru]] - Gated Recurrent Unit; Similar to lstm, but with fewer parameters
* [[softmax]] - good for output

## ReLU

**ReLU Breakpoints**: https://discourse.numenta.org/t/the-breakpoints-of-a-neural-network/6775
#todo

## LeakyReLU

## Softplus

$\log(\exp(x)+1)$, looks very similar to ReLU, but with a smooth curve around 0 (from about −0.5 to 0.5). Smoother, differentiable, doesn't produce 0 gradient exactly at 0, which are all good features.

## Tanh

## Sigmoid

## SELU

**SELU** stands for **Scaled Exponential Linear Unit**: $f(x) = λx$ for x≥0, and $λα(e^x - 1)$ for x<0. Variables λ and α are defined precisely (in theory, with ~10 digits), but roughly λ≈1.05, and α≈1.67. The plot looks like almost-relu on the right, and an exponential decay towards ~ −1.75 on the left; with a kink at 0 (the derivative isn't smooth, as it decays a bit slower immediately on the left).

The magic of this particular carefully selected equation is that it is **self-normalizing**, meaning that if a layer weights are initialized as **lecon_normal** (see [[init_dl]])

## Fancy and weird

Sitzmann, V., Martel, J. N., Bergman, A. W., Lindell, D. B., & Wetzstein, G. (2020). Implicit Neural Representations with Periodic Activation Functions. arXiv preprint arXiv:2006.09661.
https://arxiv.org/abs/2006.09661
Video abstract:
https://www.youtube.com/watch?v=Q2fLWGBeaiI
Claim that sinusoidal activation is much better than anything else, at least in domains that resonate well with Fourier transform: images, audio, 3d modeling, and fitting wave function solutions. Vaguely connect their idea to **grid cells** (coz periodicity of RF).

# Refs

Activation functions supporte by Keras: https://keras.io/api/layers/activations/

A nice summary on activation functions: https://mlfromscratch.com/activation-functions-explained/#/