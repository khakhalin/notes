# Curriculum learning

#curriculum

Related: Kolmogorov compexity

Papers:
* [[Baker2019autocurricula]] - Hide-and-seek RL from OpenAI
* [[Lillicrap2019understand]] - What does it mean to understand a network?
* [[Wu2021whencurricula]] - From the performance POV, curricula only make sense when training is very costly.

# Random musings

Did anybody try to perform stochastic gradient descent with automated adaptive curriculum? Could there be a value in periodically looking at the directions of the gradient for “best” data points, picking those that are “most interesting”, and paying more attention to them? Like, what if we put data points in a priority queue based on their error, or abs(gradient), kinda like people do when studying foreign languages with Anki. Then do SGD. Useful points would be revisited more often, then fall back once they become useless? It kinda sounds vaguely like gradient boosting, but may also make sense geometrically: if gradients for most points point lie within a hyperplane (as we reached a saddle), but for a few points you have vectors that stick out, maybe we should follow them?

Curriculum length may be used as a measure of network complexity; or rather, the ratio of curriculum entropy to network full description entropy. For flat weights and for random weights, the ratio is close to 1, or maybe some practical constant. This is kinda close to Kolmogorov Complexity, as for a random network the length of its description is equal to the length of a program that builds it (no structure). But a meaningful network is building order from chaos, for example, linking a bunch of non-random patterns to a bunch of outputs. Which means that it can be taught with a shorter curriculum. Making the ratio of curriculum / description shorter.

> Is it true? It's probably not quite true, as we don't care about the exact weights, but only about the input-output characteristics of the network, no? This needs to be thought through more carefully.

The generalized question here would be: can one somehow evolve curricula, and use their complexity to measure the network complexity? As a proxy for Kolmogorov Complexity, just in a narrow subset of possible "programs"?