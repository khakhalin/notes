# Dropout

Path: [[06_DL]] / [[regularization]]
See also: [[05_Ensembles]], [[weight_decay]]; very indirectly: [[sleep]], [[transfer]]

#regularization #dl


**Dropout** (formerly also known as **Dilution**): proposed 2012 (Hinton) as a way to avoid overfitting in deep networks when data is relatively scarce. Let's randomly inactivate a decent share (~50%) of all units in every layer at every training step; then only consider the output of remaining units; and only train these remaining units. When all weights are learned together, many of them slack out by just running into the ground (zero), and not contributing to anything (would have to be pruned, if we were to prune the network). But with drop-out, we have something of a built-in ensemble method when many sub-networks have to train in parallel.

# Math

At the forward step, we inactivate some random subset of nodes with probability (1-p). If we use an index vector δ_i (with elements equal to either 1 or 0), instead of Wx+b we will now use W(x⨀δ)+b. As the average of inputs each unit receives is now weakened by 1-p (as 1-p share of all nodes is inactivated), we need to scale the remaining inputs up during training by 1/(1-p). Meaning that the final equation for one layer is $y_i = ϕ(\frac{1}{1-p}\sum_k W_{ik} δ_k x_k + b)$. (This is technically called **inverted dropout**, as the original implementation scaled unit activations down at the testing step, not up at the training step, but this is how tensorflow does it now, as it is more computationally efficient).

For a simple linear regression (as shown in the original 2012 paper) gradient of a dropped-out network is the same as for a full network, but with an additional **L2 regularization** term for activation: $∂J'/∂w = ∂J/∂w + p(1-p)wΓ^2$, where p is the probability of elements staying active at each step, and $Γ^2 = \text{diag} (X ^\top X)$. But this is only true for this simplest case of linear regression; once non-linearity is added, and then layers multiply, this simplest logic no longer holds.

> Where does this equation above come from? I think they write quadratic loss and take expectation over the random matrix of δ_k values. And then somehow between squaring, expectation operation, and Bernully distribution, they get p(1-p), but I'm not sure, and I cannot find a micromanaged explanation of this transition from the 2012 paper.

In practice, each "submodel" (a network with inactivated units) sees not an individual point, but a minibatch.

# Intuitions

The original paper by Hinton (2012) introduced dropout in terms of a ensemble technique that allows training many overlapping networks in parallel, and thus improving the solution. Specifically, it kinda feels similar to [[bagging]], as every subnetwork sees a slightly different subset of data.

One way to understand dropout is that it breaks co-adaptation of nodes that reduces network expressiveness (it's like, when one node just learns to carefully counteract another node, and then both don't help).

Another potential explanation: to generalize well, a model should ideally be in a "flat area" in regards to some changes of inputs: changes, orthogonal to the space of meaningful dimensions shouldn't change the output. In practice, to make the output flat, we should drive some elements into saturated state. But on the other hand, with saturated hidden units, a model can no longer learn, as gradients vanish. (Hahn 2020) hypothesize that dropout solves this problem and allows to saturate inputs while still having access to meaningful gradients.

# Lore

Because of the p(1-p) term, dropout is most effective for p=0.5 (it maximizes p(1-p)). In practice, it's OK to set p at 0.5 for intermediate layers, but it should be higher (~0.8) for input layers, to avoid undersampling. _What? Why?_

Keras takes 1-p as an argument (p of dropping, not p of leaving), so adjust your thinking accordingly. _Weird, right?_

Some sources claim that dropout can, or even should be combined with L2 regularization, but some state that it is unnecessary, as they act similarly, and only hurt each other.

[[batch_normalization]] allegedly makes droupout unnecessary. _Some sources seem to imply that it is strictly better in some way._

Dropout **should not be used on convolutional networks**, as convolution introduces coordination between changes in different weights, which contradicts the very idea of dropout. Or, another way to put it, there's no risk of overfitting with convolutions, as the weight matrix is relatively small, while the data is abundant, which means that in general regularization is not necessary. Refs: [1](https://towardsdatascience.com/dropout-on-convolutional-layers-is-weird-5c6ab14f19b2), [2](https://www.kdnuggets.com/2018/09/dropout-convolutional-networks.html)

Dropout also doesn't work with RNNs. 

There's apparently a way to make it work both with convnets and RNNs, but it's not easy. One needs to synchronize dropped-out elements either in time (for RNNs), or in respect to lateral moves (for convnets). And because of the recent reservations about dropout, it's not clear if this added complexity is worth it. Arxiv refs:
* https://arxiv.org/abs/1512.05287
* https://arxiv.org/abs/1603.05118

# Variations

**Gaussian drop-out**: instead of completely eliminating a node at each step, just multiply each activation by 𝒩(1,σ). A cool thing is that it doesn't change the gradient on average, so nothing needs to be scaled. (_What does it mean? Do people use it?_)

# Refs

Original paper by Hinton (21k citations):
Srivastava, N., Hinton, G., Krizhevsky, A., Sutskever, I., & Salakhutdinov, R. (2014). Dropout: a simple way to prevent neural networks from overfitting. The journal of machine learning research, 15(1), 1929-1958. https://www.cs.toronto.edu/~rsalakhu/papers/srivastava14a.pdf
* Interestingly, claims, among other things, that the motivation for dropout comes from sexual selection in evolution (experimenting with combinations without committing too strongly to any of them)

Hahn, S., & Choi, H. (2020). Understanding dropout as an optimization trick. Neurocomputing.
https://www.sciencedirect.com/science/article/abs/pii/S0925231220302575
Starts as a nice review, but then authors start to promote their idea of why dropout works, and introduce an alternative technique that is supposedly better.

Gao, H., Pei, J., & Huang, H. (2019, January). Demystifying dropout. In The 36th International Conference on Machine Learning (ICML 2019).
http://proceedings.mlr.press/v97/gao19d/gao19d.pdf
Starts like a decent review of the topic, then leads into a wild bunch of computational experiments that supposedly demistify best practices (but not really: too much different stuff happening).

Wager, S., Wang, S., & Liang, P. S. (2013). Dropout training as adaptive regularization. In Advances in neural information processing systems (pp. 351-359).
http://papers.nips.cc/paper/4882-dropout-training-as-adaptive-regularization.pdf
The original paper stating that dropout is equivalent to indirect L2 regularization. The math is peculiar though (they describe dropout in terms of noise, and so first solve some equations for a more general case of additive noise).

Baldi, P., & Sadowski, P. J. (2013). Understanding dropout. In_Advances in neural information processing systems_(pp. 2814-2822).
https://papers.nips.cc/paper/4878-understanding-dropout.pdf
Formal paper, lots of stats formulas, then some simulations. Not interesting unless a deep understanding is needed.

How the formulas inside TF work in practice:
https://leimao.github.io/blog/Dropout-Explained/
https://pgaleone.eu/deep-learning/regularization/2017/01/10/anaysis-of-dropout/

That dropout is a big like bagging
https://srdas.github.io/DLBook/ImprovingModelGeneralization.html

Is dropout still popular? (thread from 2017)
https://www.reddit.com/r/MachineLearning/comments/5l3f1c/d_what_happened_to_dropout/
* Yes, it's popular
* Not used for conv nets and RNNs, but there are ways to use it (referenced above)
* Dropout is often computationally expensive, but there are ways to make it cheap
* Apparently **batch normalization** became more popular. _Is it still true?_

Yarin Gal, 2015, What My Deep Model Doesn't Know… A blog-style, informal, but at the same time quite mathy overview of dropout, that also uses the knowlege of Gaussian processes to explain how it works :\
http://mlg.eng.cam.ac.uk/yarin/blog_3d801aa532c1ce.html
OK, I have to admit that I dont' understand to. I really want to, as it looks so promising, but it requires too much background work to get the math.

[Simplified math](https://towardsdatascience.com/simplified-math-behind-dropout-in-deep-learning-6d50f3f47275), by Chitta Ranjan - not a good intro, even though it is firmly the first Google result.

Other potential reading:

Wager, S., Wang, S., & Liang, P. S. (2013). Dropout training as adaptive regularization. In Advances in neural information processing systems (pp. 351-359).
http://papers.nips.cc/paper/4882-dropout-training-as-adaptive-regularization.pdf