# Designing Interpretable Neural Networks

https://ryansaxe.com/transparency/2020/12/01/NAM.html

#blog #interpretability

Parents: [[06_DL]] / [[02_Regression]] / [[interpretability]]
Related: glm, [[linear_regression]], [[regularization]]

People love GLMs (linear models) because all effects are easily interpretable; and we are often to forgive GLMs their obvious shortcomings (low expressiveness) to retain this clarity. DL is on the other end of the spectrum: a black box that can, at least in theory, find any tiny relation hidden in the data, but is utterly unable to explain them, or disentangle them. But what if there were a middle-ground?

What Ryan suggests is to have a bunch of DL models, each fitting one variable (or rather, a subset of variables), each converging on a single output element, then a dense linear layer with bias mixing this output layers. It's like an additive model in a curvilinear regression, except the non-linear transformations of each variable (a subset of variables) is also learned.

The benefit is interpretability. We can look at the shape of each individual "component" (except that be warned that the model learns it up to an intercept; as there's linear mixing after, intercepts have some freedom to them). And then we can look at the mixing coefficients for each component.

> Can this system cheat, and hide some aspects from us, overfitting the data, by emulating non-additive effects through carefully crafting narrow peaks in individual models? It feels like regularization would be paramount here.

If needed, we can do interactions as well (pairwise products of some components?). Ryan doesn't describe that, but we could. The basic idea is that instead of an all-to-all block, we have several independent streams, and then a forced, very restricted way of combining them.

For each of the sub-models, Ryan uses an interesting architecture of 4 dense layers, first with ReLu, and then three with **softplus** (see [[activation_functions]]). The ReLU layer allows for fitting jagged, piecemal functions, as ReLUs turn on and off (something that's called **ReLU breakpoints**, that Ryan illustrates). And then after each section is defined, softplus learns smooth, warm, continuous non-linearities.

Nice code that is worth reproducing!

# Refs

Agarwal, R., Frosst, N., Zhang, X., Caruana, R., & Hinton, G. E. (2020). Neural additive models: Interpretable machine learning with neural nets. arXiv preprint arXiv:2004.13912.