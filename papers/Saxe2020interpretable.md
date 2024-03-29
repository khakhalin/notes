# Designing Interpretable Neural Networks

Two blog posts:
* https://ryansaxe.com/fundamentals/2021/02/21/LinearNN.html
* https://ryansaxe.com/transparency/2021/02/21/GANN.html

Parents: [[06_DL]] / [[02_Regression]] / [[interpretability]]
Related: glm, [[linear_regression]], [[regularization]]

#blog #interpretability


People love GLMs (linear models) because all effects are easily interpretable; and we are often to forgive GLMs their obvious shortcomings (low expressiveness) to retain this clarity. DL is on the other end of the spectrum: a black box that can, at least in theory, find any tiny relation hidden in the data, but is utterly unable to explain them, or disentangle them. But what if there were a middle-ground?

What Ryan suggests is to have a bunch of DL models, each implicitly fitting one variable (or rather, a subset of variables), each converging on a single output element, then a dense linear layer with bias mixing this output layers. It's like an additive model in a curvilinear regression, except the non-linear transformations of each variable (a subset of variables) are also learned! This approach is called **GAM**, or **Generalized Additive Models**.

The benefit of course is **interpretability**. We can look at the shape of each individual "component" (except that be warned that the model learns it up to an intercept; as there's linear mixing after, intercepts have some freedom to them). And then we can look at the mixing coefficients for each component.

> Can this system cheat, and hide some aspects from us, overfitting the data, by emulating non-additive effects through carefully crafting narrow peaks in individual models? It feels like regularization would be paramount here.

For each of the sub-models, Ryan uses an interesting architecture of 4 dense layers, each 8 elements wide; first one with ReLu, and then three with **softplus** (see [[activation_functions]]). The ReLU layer allows for fitting jagged, piecemal functions, as ReLUs turn on and off (something that's called **ReLU breakpoints**, as shown in illustrations). And then after each section is defined, softplus learns smooth, warm, continuous non-linearities within each of the segments. And the final output layer, dense, linear. This is probably very problem-dependent, but sounds like a curious hand-crafted solution.

Some really nice Keras code that is worth reproducing!

If needed, we can even do interactions as well (pairwise products of some components?), or any other given structure. You just calculate cross-products of outputs of some estimators manually, and provide then as input to the final linear (GLM) layer. The basic idea is that instead of an all-to-all block, we have a bunch of independent streams, and then a tightly controlled, restricted way of combining them. And there's a sample code for a twisted formula Ryan imagined that combines a binary feature with a linear feature, and whatnot. Just the question of carefully crafting each of the components, splitting inputs as needed, and combining outputs together as needed. Then compile and fit the model as any other TF model.

# Refs

Agarwal, R., Frosst, N., Zhang, X., Caruana, R., & Hinton, G. E. (2020). Neural additive models: Interpretable machine learning with neural nets. arXiv preprint arXiv:2004.13912.
https://arxiv.org/pdf/2004.13912.pdf