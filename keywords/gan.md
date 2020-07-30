# GAN: Generative Adversarial Networks

#bib #self-supervised #gan #adversarial

Parents: [[08_Self_Supervised]]

# To-Read

Odena, "Open Questions about Generative Adversarial Networks", Distill, 2019.
A nice 	short review.
https://distill.pub/2019/gan-open-problems/
* Good in modeling within one type of object (CelebA), worse if data is diverse and sparse (Imagenet)
* Problems with discrete output (text), but doable through gradient estimation
* As of April 2019, they know only one application to graphs
* Training may diverge; convergence guarantees aren't clear (?)

Nalisnick, E., Matsukawa, A., Teh, Y. W., Gorur, D., & Lakshminarayanan, B. (2018). Do deep generative models know what they don't know?. arXiv preprint arXiv:1810.09136.
https://arxiv.org/abs/1810.09136

# Notes

Some random notes on "tricks in training GANs", just as a sort of potential keywords:
* largest possible batch size
* higher learning rate and more updates for discriminator
* r1 penalty (scaled linearly with output feature dimensions)
* truncated latent sampler
* multiscale gradients (eg. msggan or stylegan2)
* exponentially moving average for evaluation of generator
* latent optimization
* spectral norm with hinge loss (for datasets with a lot of variety)
* conditional batchnorm (if labels are available)
* regularizers on discriminator (eg. dropout, l2, orthogonal)
* grid search for feature map multiplier or size of g and f
* Train with Two Time Update Rule (2 different learning rates)
* Sepp Hochreiter firmly believes in Adam
* Have some sort of Lipschitz constraint i.e. WGAN, WGAN-GP, SNGAN(used a lot)
* loss function is not always very important
* Use cosine learning schedule
* Train generators and discriminators alone first
* Use patchGAN discriminator alongwith a global one.
* Use a replay for discriminator
* Use random data for discriminator as well

Reference for this list: https://www.reddit.com/r/MachineLearning/comments/i085a8/d_best_gan_tricks/

# Links

Amazing demo on how GANs work: https://poloclub.github.io/ganlab/