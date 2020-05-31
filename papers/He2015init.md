# Famous He 2015 initialization paper

He, K., Zhang, X., Ren, S., & Sun, J. (2015). Delving deep into rectifiers: Surpassing human-level performance on imagenet classification. In Proceedings of the IEEE international conference on computer vision (pp. 1026-1034).

#dl #init #relu #image

Aka "He-et-al". A famous paper with a simple innovation (initialization of parameters) that everybody uses since then.

Technically, the paper is about optimizing ReLUs by parametrising them, and also finding a good initialization for the parameters, which enables deeper networks. Then they apply these innovations to ImageNet and achieve a huge improvement over the winner of the time. Also apparently the first time human performance was surpassed.

Their network is very narrow but very deep. It has 14 layers with weights:
* 2d conv 7×7×64, 1 time → 3×3 pool
* 2d conv 2×2×128, 4 times → 3×3 pool
* 2d conv 2×2×256, 6 times → Spatial Pyramid Pooling (ref below)
* fully connected, 3 times: 4096, 4096, 1000

Their parametrized modification of ReLUs (PReLU) is like a leaky ReLU (not 0, but weaker linear for x<0), but with learned coefficient ay. The right part (x>0) is still kept as f(x) = x. They reference some experiments as showing that leaky Relus are actually not better than normal Relus, but claim that they have a hope that their activations will be better, as they are trainable. _I'm not yet sure what was the verdict of time here._

But most importantly, they theoretically derive the best initialization. They claim that without this clever init (with default gaussians with fixed SD), the only way to train really deep networks is to do it in a "greedy" way by pre-training first layers before adding extra layers. Then there's Xavier Glorot init [[Glorot2010init]], which uses scaled uniform distributions, but is theoretically suboptimal.

The idea is that the variation of activation at each next level ~ var of inputs, multiplied by the variance of weights, and layer's size (for convoluational layers: depth ∙ width²). That's assuming Gaussian distribution of weights. To achieve good convergance we should make sure that the signal, and thus the gradients, don't vanish or explode. Which means that variation of weights at each layer should be related to its size, $\text{sd}(w_l) = \sqrt{2/(kw^2)}$. Apparently it's just 2 times larger than Xavier; so very similar, but numerically different, and for a very-deep network this difference can mean convergence ws no convergence. (And that's because Xavier used linear approximation instead of ReLU; that's where the 2-times difference comes from).
 
# Coverage and explanations

* https://medium.com/@prateekvishnu/xavier-and-he-normal-he-et-al-initialization-8e3d7a087528

# Related work

All you need is a good init. (2016). Dmytro Mishkin, Jiri Matas.
https://arxiv.org/pdf/1511.06422.pdf

# Refs

A link for Spatial Pyramid Pooling, from same authors, so maybe something custom that is OK not to use?
K. He, X. Zhang, S. Ren, and J. Sun. Spatial pyramid pooling in deep convolutional networks for visual recognition. arXiv:1406.4729v2, 2014.