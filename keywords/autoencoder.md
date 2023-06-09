# Auto-Encoders

Parents: [[08_Unsupervised]]
Related: [[gan]]

#bib #vae #self-supervised #unsupervised


**Autoencoders**: Encoder → latent space (aka code) → Decoder → loss function. Introduces an informational bottleneck.

**Denoising autoencoder** - learn to reconstruct original data from damage data. Same thing for neural inpainting (reconstructing a missed part of an image).

**Variational Auto-Encoders** are similar both to classic autoencoders, and to generative models. They use Bayesian reasoning (**Stochastic Gradient Variational Bayes** estimator), where the Bayesian task is formulated as inferring the parameters of the model, knowing the output. Basically, we assume that actual data was actually produced by a similar decoder, and we are trying to reconstruct the parameters of these decoder from the data. Instead of a "classic" loss function, it uses some weird thing based on KL-divergence ([[kl]]).

In VAE, instead of mapping onto a normal vector, we map on a vector of means and a vector of standard deviations, then we **sample a vector** from a distribution defined by these μs and σs, and make the decoder deal with this vector. And a loss function is different: L = Reconstruction_Loss − KL, where Reconstruction Loss is the same as for a normal autoencoder (except averaged across different sampled vectors), and KL is run on the vector of latent parameters, keeping it close to a standard normal distribution.

But how to backpropagate through a sampling operation? You just have a stochastic node ε that is just normal noise (takes a unique value on each iteration), and define sampling operation as μ + σε, where both μ and σ are outputs of the encoder. Backprop ignores ε, but updates all weights that get to μ and σ as for any normal network.

# Refs

Good 15m video explaning VAEs, with an example in tensorflow: https://www.youtube.com/watch?v=9zKuYvjFFS8

Tutorial on VAEs, with math explained: https://jaan.io/what-is-variational-autoencoder-vae-tutorial/

Denoising autoencoder in keras [[tensorflow]]: https://keras.io/examples/vision/autoencoder/