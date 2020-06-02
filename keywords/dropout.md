# Dropout

Path: [[06_DL]]

A breakthrough from 2012: resolves overfitting in deep networks. Randomly inactivate a large share (~50%?) of all units in every layer at every training step; then only consider the output of remaining units; and only train them. When all weights are learned together, many of them slack out by just running to the ground (zero), and not contributing to anything (would have to be pruned). But with drop-out, we have something of a built-in ensemble method when many sub-networks have to train in parallel.

> Is this intuition even true? Is dropout in any way similar to ensemble? Are ensambles and regularization secretly the same thing?

A gradient of a dropped-out network is the same as for a full network, but with an additional "regularization term" for activation: ∂J_dropped/∂w = ∂J/dw + p(1-p)w∙I² , where p is the probability of elements staying active at each step (they use fixed probability, and not a fixed number of active elements). _Ref 1 pretends to have math, but immediately plunges from obvious to confusing._

Because of that, dropout is most effective for p=0.5 (it maximizes p(1-p)). In practice, it's OK to set p at 0.5 for intermediate layers, but it should be higher (0.8?) for input layers, in order not to imitate undersampling. Keras ([[tensorflow]]) takes 1-p as an argument (p of dropping), so adjust accordingly.

**Gaussian dropout**: instead of completely eliminating a node at each step, just multiply each activation by 𝒩(1,σ). A cool thing is that it doesn't change the gradient on average, so nothing needs to be scaled. (_What does it mean?_)

> How much exactly does drop-out help? How to quantify that? How do people quantify effectiveness of things like regularization? If dropout is similar to regularization, is it strictly better than regularization, or is it the same? If we explicitly add this wI^2-proportional (but fixed) regularization, will it give the same result? I'm guessing the answer is "no", but why?

There's a claim that dropout should not be used on convolutional networks, because it does not actually work (does not inactivate elements), as convolution introduces coordination between changes in different weights. Refs [1](https://towardsdatascience.com/dropout-on-convolutional-layers-is-weird-5c6ab14f19b2), [2](https://www.kdnuggets.com/2018/09/dropout-convolutional-networks.html)

# Refs

[Simplified math](https://towardsdatascience.com/simplified-math-behind-dropout-in-deep-learning-6d50f3f47275), by Chitta Ranjan - very so-so intro, as the math isn't really simplified. Find something better.