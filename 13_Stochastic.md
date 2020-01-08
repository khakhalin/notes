# Stochastic approaches

#todo :
Visual guide to Evolution strategies:
http://blog.otoro.net/2017/10/29/visual-evolution-strategies/
(Must, because visual and nice!!)

https://en.wikipedia.org/wiki/Mean_field_particle_methods

## Variational Inference
Links for now:
* https://en.wikipedia.org/wiki/Variational_Bayesian_methods
* https://arxiv.org/abs/1601.00670
Variational Inference: A Review for Statisticians
David M. Blei, Alp Kucukelbir, Jon D. McAuliffe

## Gaussian processes
Relevant cards:
* [[Gortler2019gaussian]]

## Markov Chain Monte Carlo (MCMC)
General references:
* https://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo
* https://en.wikipedia.org/wiki/Metropolis%E2%80%93Hastings_algorithm
* https://en.wikipedia.org/wiki/Slice_sampling
* https://en.wikipedia.org/wiki/Gibbs_sampling

Say, you have a multidimensional distribution with probability P(x). You don't know P(x) exactly, but you have a guess f(x) that is proportional to P(x). (Apparently this is an important nuance, as understanding which x are more probable and which are less probable is relatively easy, but ot know the P exactly requires integraing it over x, which is often an unachievable luxury). You want to sample points from P(x). Usually, because you want to integrate something over P(x).

To generate these points, we use an interative approach ("walkers"). Pick a random point x_t. Find a random step (from a good symmetrical distribution, usually Gaussian). Calculate how much probable it is compared to current x: α = f(x_next)/f(x_current). Accept the jump with probability α.

Obviously, it makes consecutive samples highly correlated. One option is to throw most points away. Another, use wide jumps (but then we'll reject most jumps of course). Convergence may be very slow. Also, first few samples (before it finds the dense part) are also bad (known as *burn-in period*).

Not sure if these methods are considered **Bayesian**, or are just similar to them, but clearly related.

Various attempts to improve (listed on wiki):
* Calculate gradient and try to use it
* Jump back if dead-ended
* Use momentum

Alternatively, instead of a random multivariate step, we can iteratively construct a step, dimension by dimension, using various univariate probability functions (say, conditional probabilities). *Gibbs sampling* seems to work like that, if I got it right.

# Evolutionary algorithms

## Quality-Diversity
An approach to evo algorithms in which you don't just pick individuals based on top fitness, but to to optimize the diversity of solutions. Apparently also known as **illumination**.
https://quality-diversity.github.io/

List of papers: https://quality-diversity.github.io/papers

One of the productive algorithms in this family: Multi-dimensional Archive of Phenotypic Elites (MAP-Elites). The idea is that on top of a objective function (target to maximize or minimize) we introduce dimensions (like, different aspects, properties of the solution). Then each dimension is divided into bins, and thus the entire space is sections into cubes. The algorithm looks for a good solution within each cube, for each intersection of bins, and retains top performances in each cube. If the cube is empty, so be it.

Mouret, J. B., & Clune, J. (2015). Illuminating search spaces by mapping elites. arXiv preprint arXiv:1504.04909. https://arxiv.org/pdf/1504.04909.pdf