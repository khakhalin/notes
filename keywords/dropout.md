# Dropout

#regularization #dl

Path: [[06_DL]] / [[regularization]]
See also: [[05_Ensembles]], [[weight_decay]]

**Dropout** (formerly also known as **Dilution**): proposed 2012 (Hinton) as a way to avoid overfitting in deep networks when data is relatively scarse. Let's randomly inactivate a decent share (~50%) of all units in every layer at every training step; then only consider the output of remaining units; and only train these remaining untes. When all weights are learned together, many of them slack out by just running into the ground (zero), and not contributing to anything (would have to be pruned, if we were to prune the network). But with drop-out, we have something of a built-in ensemble method when many sub-networks have to train in parallel.

> Is this intuition even true? Is dropout in any way similar to ensemble? Are ensambles and regularization secretly the same thing?

# Math

A gradient of a dropped-out network is the same as for a full network, but with an additional "regularization term" for activation: ∂J_dropped/∂w = ∂J/dw + p(1-p)w∙I² , where p is the probability of elements staying active at each step (they use fixed probability, and not a fixed number of active elements). 

> #todo: figure out how this math works. We need at least a hint of a derivation for it.

# Intuitions

> How much exactly does drop-out help? How to quantify that? How do people quantify effectiveness of things like regularization? If dropout is similar to regularization, is it strictly better than regularization, or is it the same? If we explicitly add this wI^2-proportional (but fixed) regularization, will it give the same result? I'm guessing the answer is "no", but why?

# Lore

Because of the p(1-p) term, dropout is most effective for p=0.5 (it maximizes p(1-p)). In practice, it's OK to set p at 0.5 for intermediate layers, but it should be higher (~0.8) for input layers, to avoid undersampling.

Keras takes 1-p as an argument (p of dropping, not p of leaving), so adjust accordingly.

> Why 0.8 again? How does it work?

Dropout should not be used on convolutional networks, as convolution introduces coordination between changes in different weights, which contradicts the very idea of dropout. Or, another way to put it, is that there's no risk of overfitting with convolutions, as the weight matrix is relatively small, while the data is decidedly abundant, which means that in general regularization is not necessary. Refs: [1](https://towardsdatascience.com/dropout-on-convolutional-layers-is-weird-5c6ab14f19b2), [2](https://www.kdnuggets.com/2018/09/dropout-convolutional-networks.html)

# Variations

**Gaussian drop-out**: instead of completely eliminating a node at each step, just multiply each activation by 𝒩(1,σ). A cool thing is that it doesn't change the gradient on average, so nothing needs to be scaled. (_What does it mean? Do people use it?_)

# Refs

How the formulas inside TF work in practice:
https://leimao.github.io/blog/Dropout-Explained/
https://pgaleone.eu/deep-learning/regularization/2017/01/10/anaysis-of-dropout/

Is dropout still popular?
https://www.reddit.com/r/MachineLearning/comments/5l3f1c/d_what_happened_to_dropout/

Yarin Gal, 2015, What My Deep Model Doesn't Know…
http://mlg.eng.cam.ac.uk/yarin/blog_3d801aa532c1ce.html

[Simplified math](https://towardsdatascience.com/simplified-math-behind-dropout-in-deep-learning-6d50f3f47275), by Chitta Ranjan - very so-so intro, as the math isn't really simplified. Find something better.

Original paper by Hinton (21k citations):
Srivastava, N., Hinton, G., Krizhevsky, A., Sutskever, I., & Salakhutdinov, R. (2014). Dropout: a simple way to prevent neural networks from overfitting. The journal of machine learning research, 15(1), 1929-1958.