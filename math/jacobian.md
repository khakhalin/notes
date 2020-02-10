# Jacobian
#dynamic

Say we have a function f(x) that projects ℝn to ℝm. (I'd say that ℝn→ℝn is actually most fun, but this way it's more general). A jacobian J is a matrix of all possible partial derivatives of f (as a vector) by x (as a vector: that is, but all coordinates of x): D_ij = ∂f_i/∂x_j.

Then we can have a Taylor approximation of f(x) as f(x) ~ f(x0) + D(x0)(x-x0).

For a scalar F, D = transposed gradient: D = (∇f)ᵀ.

Jacobian is really helpful in finding **stationary points** in **dynamical systems**. If f(x) is a derivative of x by time, then x ~ x + f(x)dt. If f(x) = 0, then x doesn't change (stationary point). Now, what happens when x deviates a bit from the stationary point (whether it is stable or unstable) depends on the Jacobian. If x-x0 is small, then f(x) ~ f(x0) + D(x0)(x-x0), but f(x0) happens to be 0, and df/dt is mostly defined by D(x0). And at this point all depends on the eigenvalues:
* If they are complex, there's some rotation
* If real part is > 0, there's an escape to ∞ along this eigenvector
* If real part is < 0, there's attraction to a stable point along this eigenvector
* With real part = 0, we have just endless rotation

Obviously with dimensionality >1 we can have any combination of attraction and escaping.

# Refs
* http://www.scholarpedia.org/article/Equilibrium