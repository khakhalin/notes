# Optimizers in DL

Parents: [[hyperparameters]]
See also: [[regularization]], [[archsearch]]

#optimizers


For "optimiziers" in the sense of linear programming, see [[linprog]]

Subtopics:
* [[adam]] - the most famous optimizer
* [[Ruder2016descent]] - an overview of different optimizers

# Learning rate

**Goldilocks principle** - the best learning rate should "magically" put you in the minimum in a very few steps. Large learning rate leads to noisy oscillations after what looked like a convergence. It may even break everything after convergence (unstable).