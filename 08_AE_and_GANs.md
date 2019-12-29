# Autoencoders and GANs

#gan

Odena, "Open Questions about Generative Adversarial Networks", Distill, 2019.
A nice 	short review.
https://distill.pub/2019/gan-open-problems/
* Good in modeling within one type of object (CelebA), worse if data is diverse and sparse (Imagenet)
* Problems with discrete output (text), but doable through gradient estimation
* As of April 2019, they know only one application to graphs
* Training may diverge; convergence guarantees aren't clear (?)