# Self-Supervised Learning
#self-supervised

Related:
* [[triplet_loss]] - aka contrastive loss (or at least one type of it)

Cool keywords:
* Triplet loss = contrastive learning (? is it true?) = negative sampling (may be a more popular term for that)

# Autoencoders

Encoder → latent space (aka code) → Decoder → loss function.

## VAE

**Variational Auto-Encoders** are similar both to classic autoencoders, and to generative models. They use Bayesian reasoning ("_Stochastic Gradient Variational Bayes_" estimator), where the Bayesian task is formulated as inferring the parameters of the model, knowing the output. Basically, we assume that actual data was actually produced by a similar decoder, and we are trying to reconstruct the parameters of these decoder from the data. Instead of a "classic" loss function, it uses some weird thing based on KL-divergence (see [[Information]]). _Parked for now_

Refs:
* [Tutorial on VAEs](https://jaan.io/what-is-variational-autoencoder-vae-tutorial/) - explains the math

# GANs

Links:
* Amazing demon on how GANs work: https://poloclub.github.io/ganlab/

# Embedding layers
Solves the problem of data sparsity (to avoid direct training on long one-hot encoded vectors). A bottleneck layer from an N-hot layer to learn an embedding. Number of units in the bottleneck = number dimensions in the enbedding. Empirical rule of thumb: n dim ~ (n possible values)^(1/4). _Where does it come from?_ With too few dimensions you won't capture the complexity; with too many - risk of overfitting, and longer to train. N dimensions should be treated as a hyperparameter, and optimized.

Classical training: get hold of some similarity or co-occurance measure; use to produce groups, then "withhold some", predicting missing members of a group based on those that are visible. But look also: [[Weng2019self-supervised.md]]

Examples:
* [[word2vec]]

# GANs
#gan
Odena, "Open Questions about Generative Adversarial Networks", Distill, 2019.
A nice 	short review.
https://distill.pub/2019/gan-open-problems/
* Good in modeling within one type of object (CelebA), worse if data is diverse and sparse (Imagenet)
* Problems with discrete output (text), but doable through gradient estimation
* As of April 2019, they know only one application to graphs
* Training may diverge; convergence guarantees aren't clear (?)