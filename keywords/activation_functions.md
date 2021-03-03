# Activation Functions for DL

#dl #bib

Parent: [[06_DL]]
See also: [[regularization]], [[hyperparameters]], [[backprop]]

See also (special types of units):
* [[lstm]] - For RNs, Long-Short-Term-Memory
* [[gru]] - Gated Recurrent Unit; Similar to lstm, but with fewer parameters
* [[softmax]] - good for output

## ReLU

max(x, 0): nice, simple, easy to calculate. Allows high activation values, no vanishing gradients on the right, which means that we don't encourage super-high weights. Once weights to a ReLU drive the input to <0, it cannot recover on its own though. The gradient is forever zero (aka **Dead ReLU**). In some situations it may be a blessing in disguise however, as it makes outputs of each layer sparse, which can make calculations even faster.

#todo :
**ReLU Breakpoints**: https://discourse.numenta.org/t/the-breakpoints-of-a-neural-network/6775

Footnotes:
* https://mlfromscratch.com/activation-functions-explained/#relu

## LeakyReLU

Has a slope α for x<0 as well (this α becomes a hyperparameter). Apparently works slightly better then ReLU. There's also a thing called **randomized Leaky ReLU** (where α is different in every neuron, is sampled from some distribution and then fixed) that apparently works again slightly better than a simple Leaky ReLU (but I have no idea why).

## ELU

**Exponential linear unit**: x on the right, but $α(e_x-1)$ on the left. avoids dead relu problem, but is harder to compute. See SELU below tho!

## Softplus

$\log(\exp(x)+1)$, looks very similar to ReLU, but with a smooth curve around 0 (from about −0.5 to 0.5). Smoother, differentiable, doesn't produce 0 gradient exactly at 0, which are all good features.

## Sigmoid

Classic $σ(x) = 1/(1+e^{-x})$. Nice derivative: $\displaystyle σ'(x) = \frac{-e^{-x}}{(1+e^{-x})^2} = σ(x)\frac{-e^{-x}}{1+e^{-x}}=σ(x)\left(1-\frac{1+e^{-x}}{1+e^{-x}} - \frac{-e^{-x}}{1+e^{-x}}\right)=σ(x)(1-σ(x))$. But it falls down too fast as x increases, leading to **vanishing gradients**, and its twin problem of **exploding gradients** (see [[backprop]]). It is a good smooth function for shallow networks, but it makes deep networks impossible to train.

> Wouldn't activation normalization help? Or do we consider it too slow to be practical?

## Tanh

> Was very popular in the past, but apparently trains slower than ReLUs due to gradient saturation. But isn't it still popular in [[07_RNNs]]? (With a caveat that RNNs themselves are no longer popular haha)

## SELU

**SELU** stands for **Scaled Exponential Linear Unit**: $f(x) = λx$ for x≥0, and $λα(e^x - 1)$ for x<0. Variables λ and α are defined precisely (in theory, with ~10 digits), but roughly λ≈1.05, and α≈1.67. The plot looks like almost-relu on the right, and an exponential decay towards ~ −1.75 on the left; with a kink at 0 (the derivative isn't smooth, as it decays a bit slower immediately on the left).

The magic of this particular carefully selected equation is that it is **self-normalizing**, meaning that if a layer weights are initialized as **LeCun normal** (see [[init_dl]]) then after going through this layer normally (and standardly) distributed activations (μ=0, σ=1) produce an output with same μ and σ. You also cannot use standard dropout with this activation function.

Footnotes:
* https://mlfromscratch.com/activation-functions-explained/#selu

## GELU

**Gaussian Error Linear unit** Used in transformers ([[transformers]]). Linear on the right, goes down, and then to 0 again on the left.

Footnotes:
* https://mlfromscratch.com/activation-functions-explained/#gelu

## Fancy and weird

Sitzmann, V., Martel, J. N., Bergman, A. W., Lindell, D. B., & Wetzstein, G. (2020). Implicit Neural Representations with Periodic Activation Functions. arXiv preprint arXiv:2006.09661.
https://arxiv.org/abs/2006.09661
Video abstract:
https://www.youtube.com/watch?v=Q2fLWGBeaiI
Claim that sinusoidal activation is much better than anything else, at least in domains that resonate well with Fourier transform: images, audio, 3d modeling, and fitting wave function solutions. Vaguely connect their idea to **grid cells** (coz periodicity of RF).

# Refs

Activation functions supporte by Keras: https://keras.io/api/layers/activations/

A nice summary on activation functions: https://mlfromscratch.com/activation-functions-explained/#/