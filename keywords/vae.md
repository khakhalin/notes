# Variational Auto-Encoder

#bib #vae #self-supervised #unsupervised

Parents: [[08_Unsupervised]], [[15_generate]]
Related: [[gan]]

**Autoencoders**: Encoder → latent space (aka code) → Decoder → loss function.

**Variational Auto-Encoders** are similar both to classic autoencoders, and to generative models. They use Bayesian reasoning ("_Stochastic Gradient Variational Bayes_" estimator), where the Bayesian task is formulated as inferring the parameters of the model, knowing the output. Basically, we assume that actual data was actually produced by a similar decoder, and we are trying to reconstruct the parameters of these decoder from the data. Instead of a "classic" loss function, it uses some weird thing based on KL-divergence (see [[Information]]). _Parked for now_

Refs:
* [Tutorial on VAEs](https://jaan.io/what-is-variational-autoencoder-vae-tutorial/) - explains the math