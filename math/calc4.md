# Vector and matrix calculus

#math #calculus

See also: [[linalg]]

# Differentiating

Differentiating **a scalar by a vector** is funny: while vectors are typically columns (column-vectors), a derivative of a scalar over a vector is a row-vector: $\frac{∂f}{∂ \textbf{x}} = [… \frac{∂f}{∂x_j} …]$. This is because to use derivatives in a vector space, we have to assume a certain **Layout convention**: namely, what happens if both Y and X are vectors, and you're considering ∂Y/∂X. For consistency, a derivative of a scalar by a vector, and a derivative of a vector by a scalar should have opposite orientations (if one is a row, the other one has to be a column). If you don't stick to this, you won't be able to work with vector fields (as one way or another, you need to get a matrix if you differentiate vector by a vector).

Typically, people want a derivative of a vector by a scalar (like, a speed) to be a normal vector (so that position, speed, acceleration etc. all had the same dimensions). This **sane and obviously more popular approach** is called **Numerator layout**, or _Jacobian formulation_. The payback for this however is that ∂scalar/∂vector is row vector, and may have to be flipped to become useful (see "gradient") below.

If however you are a strange person with no friends, and you are not particularly interested in having some, and want ∂scalar/∂vector be a good vector (column) instead, this sick approach iss called **Denominator layout**, or _Hessian formulation_. It may make some ML formulas look easier (you can write gradient descent as θ = θ + α ∂θ/∂x), but it breaks physics, as a derivative of a vector by a scalar has to be transposed (speed and acceleration now have different shape, and Euler integration looks like x = x + mvᵀ).

The good news is that in practice even ML people rarely use $\frac{∂f}{∂\textbf{x}}$ directly, and rely on the **gradient** ∇ instead. A gradient is like a derivative by x, but is defined in a way that always produces "proper vectors". So even in the numerator layout, ∇y is a "normal" column vector, as the gradient is defined as $∇y = \left(\frac{∂f}{∂\vec x}\right)^\top$. (In the denominator layout ∇y is just a derivative, without a transpose). 

> Note that not all sources make this distinction. For example, as of Nov 2020, Wikipedia uses words "gradient" and ∂y/∂X as synonyms (for both layouts) on the "Matrix_calculus" page, but explicitly defines gradient as a flipped derivative at the "Gradient" page.

Next, as a generalization of this case, differentiating a **vector by a vector** has to now produce a matrix. Say, if both x and y are normal vectors (column-vectors), it would be consistent to define ∂y/∂x as a matrix D with elements  $\displaystyle d_{ij} = \frac{\partial y_i}{\partial x_j}$. Each component of vector y (column-vector) creates a new row (so its influence is still columnar, which means that a derivative by a scalar is a column-vector), while vector x works in a "flipped way", creating elements within each row (so a derivative of a scalar is a row-vector). Again, that's a consistent "numerator layout".

Incidentally, it means that ∇(Ax) = Aᵀ regardless of the notation system. In the numerator layout:
$\displaystyle [∇(\textbf{Ax})]_{ij} = \left( \frac{∂(\textbf{Ax})}{∂\textbf{x}} \right)^\top = \left[ \frac{∂(\textbf{Ax})_i}{∂x_j} \right]^\top = \frac{∂(\textbf{Ax})_j}{∂x_i} = \frac{\sum a_{jk}x_k}{x_i} = a_{ji}$. In the denominator layout, the transpose operator in 2nd and 3d stages disappears, but the derivative is defined as the 4th operator, so the end result is the same!

Now, how to differentiate **a scalar by a matrix**? We can generalize the idea of differentiating by a vector, which also produces a vector, as we differentiate by each component independently, except that the resulting vector is flipped. So let's do the same thing for the matrix: differentiate by each element, and flip the result: $\displaystyle \left. \frac{∂y}{d\textbf{X}}\right|_{ij} = \frac{∂y}{∂x_{ji}}$.

Is it possible to differentiate a vector by a matrix, a matrix by a vector, or a matrix by a matrix? Conceivably, one could try to define them, but they are rarely used, and we'll pretend that they don't exist.

# Identities (in numerator layout)

A bunch of useful identities (similar to ∇(Ax) = A) are given here:
https://en.wikipedia.org/wiki/Matrix_calculus#Identities

For example, when calculating $∂F(x)/∂x$:
* aᵀx → aᵀ
* xᵀa → aᵀ
* Ax → A
* xᵀA → A
* xᵀAx → xᵀ(A+Aᵀ), which obviously gives 2xᵀA if A is symmetric
* xᵀx → 2xᵀ (as a partial case of above)
* aᵀxxᵀb → xᵀ(abᵀ + baᵀ)

# Refs

* https://en.wikipedia.org/wiki/Matrix_calculus#Layout_conventions
* https://en.wikipedia.org/wiki/Gradient
* https://atmos.washington.edu/~dennis/MatrixCalculus.pdf

Controversial takes and arguments over whether a gradient produces a vector or not:
* https://www.physicsforums.com/threads/how-is-the-gradient-covariant.944255/