# Vector and matrix calculus

#math #calculus

See also: [[linalg]]

# Differentiating

Differentiating **a scalar by a vector** is called a **gradient**: $∇y = \frac{∂f}{∂\vec x} = [… \frac{∂f}{∂x_j} …]$. Note that if x-vector is a column, then the gradient is a row, and the other way around.

That's because to use derivatives in a vector space, we have to assume a certain **Layout convention**: namely, what happens if both y and x are vectors, and you're considering ∂y/∂x. For consistency, a derivative of scalar by a vector (like, gradient), and a derivative of vector by a scalar (like, speed) should have opposite orientations (if one is a row, the other one should be a column). Or you won't be able to work with vector fields (as one way or another, you need to get a matrix once you differentiate vector by a vector).

* If you want gradient of a scalar be a usual vector (column), it's called **denominator layout**, or _Hessian formulation_. It makes some ML formulas easier (you can write gradient descent as θ = θ + α∇θ). But then derivative of a vector by a scalar has to be transposed compared to the vector, which breaks physics (speed and acceleration now have different shape, so Euler integration of trajectories looks like x = x + mvᵀ!). Also, ∇(Ax) = Aᵀ (see below).
* If on the other hand if you want the derivative of a vector have the same shape as the vector itself, it's called **numerator layout**, or _Jacobian formulation_. It preserves mechanics formula, but gradient has to be a flipped vector (from a row-vector to a column vector), so gradient descent would use a ∇ᵀ operator. But ∇(Ax) = A (see below).
* The third option is to give up on ∂vector/∂vector altogether, and pretend that both derivatives produce column vectors. But it makes matrix calculus impossible.

Next, as a generalization of this case, differentiating a **vector by a vector** has to now produce a matrix. Say, if both x and y are normal vectors (column-vectors), it would be consistent to define ∂y/∂x as a matrix D with elements  $\displaystyle d_{ij} = \frac{\partial y_i}{\partial x_j}$. Each component of vector y (column-vector) creates a new row (so its influence is still columnar), while vector x works in a "flipped way", creating elements within each row. Again, that's a consistent "numerator layout".

In this layout, ∇(Ax) = A. Proof: $\displaystyle [∇(\textbf{Ax})]_{ij} = \frac{∂(\textbf{Ax})_i}{∂x_j} = \frac{\sum a_{ik}x_k}{x_j} = a_{ij}$. (Note that not all books follow this layout; VMLS for example insists that ∇Ax = Aᵀ).

Now, how to differentiate **a scalar by a matrix**? We can generalize the idea of differentiating by a vector, which also produces a vector, as we differentiate by each component independently, except that the resulting vector is flipped. So let's do the same thing for the matrix: differentiate by each element, and flip the result: $\displaystyle \left. \frac{∂y}{d\textbf{X}}\right|_{ij} = \frac{∂y}{∂x_{ji}}$.

Is it possible to differentiate a vector by a matrix, a matrix by a vector, or a matrix by a matrix? Conceivably, one could try to define them, but they are rarely used, and we'll pretend that they don't exist.

# Identities

A bunch of useful identities (similar to ∇(Ax) = A) are given here:
https://en.wikipedia.org/wiki/Matrix_calculus#Identities

# Refs

https://en.wikipedia.org/wiki/Matrix_calculus#Layout_conventions

https://en.wikipedia.org/wiki/Gradient

https://atmos.washington.edu/~dennis/MatrixCalculus.pdf