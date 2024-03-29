# Brute-force Modeling

Monte-Carlo, Agent-based, Evolution, Particles, automata

#bib #evo #agents


https://en.wikipedia.org/wiki/Mean_field_particle_methods

Subtopics:
* [[gen_alg]] - Genetic algorithms
* [[mdp]] - Markov Decision Process

Agent-based modeling:
* [[goap]] - modeling agents in strategy games

Automata:
* [[cellular_automata]] - topic
* [[graph_automata]] - automata on graphs
* [[neuro_cell_automata]] - automata that are also networks

The interface with DL and RL:
* [[Baker2019autocurricula]]

# Variational Inference

Links for now:
* https://en.wikipedia.org/wiki/Variational_Bayesian_methods
* https://arxiv.org/abs/1601.00670
Variational Inference: A Review for Statisticians
David M. Blei, Alp Kucukelbir, Jon D. McAuliffe

# Gaussian processes

#todo to read:
https://ekalosak.github.io/blog/gaussian_process.html
Nice intro to Gaussian Processes (in R).

See cards:
* [[Gortler2019gaussian]]

# Markov Chain Monte Carlo (MCMC)

General references:
* https://en.wikipedia.org/wiki/Markov_chain_Monte_Carlo
* https://en.wikipedia.org/wiki/Metropolis%E2%80%93Hastings_algorithm
* https://en.wikipedia.org/wiki/Slice_sampling
* https://en.wikipedia.org/wiki/Gibbs_sampling

Say, you have a multidimensional distribution with probability P(x). You don't know P(x) exactly, but you have a guess f(x) that is proportional to P(x). Apparently this is an important nuance, as understanding which x are more probable and which are less probable is relatively easy, but to estimate P(x) exactly would require integraing it over X, which is often an unachievable luxury. You want to sample points from P(x). Usually, because you want to integrate something over X with P(x) inside the integral.

To generate these points, we use an interative approach ("walkers"). Pick a random point x_t. Find a random step (from a good symmetrical distribution, usually Gaussian). Calculate how much probable it is compared to current x: α = f(x_next)/f(x_current). Accept the jump with probability α.

Obviously, it makes consecutive samples highly correlated. One option is to throw most points away. Another, use wide jumps (but then we'll reject most jumps of course). Convergence may be very slow. Also, first few samples (before it finds the dense part) are also bad (known as *burn-in period*).

> Not sure if these methods are considered **Bayesian**, or are just similar to them, but it seems related, right?

Some ways to improve:
* Calculate gradient and try to use it
* Jump back if dead-ended
* Use momentum

Alternatively, instead of a random multivariate step, we can iteratively construct a step, dimension by dimension, using various univariate probability functions (say, conditional probabilities). *Gibbs sampling* seems to work like that, if I got it right.