# Stacking

#ensemble

Parent: [[05_Ensembles]]
Related: [[bagging]]

A practical way to create a reasonable linear combination of several non-linear models using cross-validation.

**Stacked generalization** or **Stacking**: Say you have a way of building various models f_m, and you train each of them on a dataset X. Models f_m may belong to the same class, or to different classes, but it is important that they are sufficiently different: that is, they need to be defined both by the training set X, and by some hyperparameters that come with the model itself. So while each model is a function of a dataset, f(X), two models f_m1(X) and f_m2(X) should still be different.

Consider training each model f_m() on a dataset  X minus observation xi (a type of [[bagging]]?). It would create a family of predictors $f_m^{-i}(x)$ for each model class f_m(), providing a good way of assessing the accuracy of m-type models f_m() via cross-validation: just predict y_i by each $f^{-i}$, and sum all errors. But instead of just picking the best model, we will build the best linear combination of all these models, by finding an optimal vector of weights w, so that $\sum_i \big( y_i - \sum_m w_m f_m^{-i}(x_i)\big)^2$ is minimal. In other words, we find a set of coefficients w, so that the linear combination of all f_m gives the best total cross-validation across all "remove one x_i". It also helps to constrain w to w_i>0 ∀i and ∑w_i = 1, as it turns it into a quadratic problem.

# Refs

ESL p282-290