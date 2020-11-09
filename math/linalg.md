# Linear algebra

#math #linalg #bib

Related: [[calc4]]

Subsections:
* [[la_net]] - Linear algebra for graphs
* [[la_sig]] - Linalg for signal processing

Subtopics: [[dual_basis]], [[svd]]

# Intro

**Linear** means f(ax) = a f(x), and f(a+b) = f(a)+f(b). Examples of linear: negation, reversal, running sum, demeaning (subtract mean(x) from all x_i elements of x).

*Th*: If f(x) is linear, it can be represented as a matrix multiplication Ax
Proof: x = sum x_i e_i , and as f is linear, f(x) = sum x_i f(e_i) where f(e_i) is in out-space (codomain) of f. If we write coordinates of each f(e_i) in this space, we get a matrix with columns of f(e_i), and f(x) = Ax. QED 

What most people think of as "linear" is actually **affine**: Ax+b (linear + const). This representation is unique, and A = (f(x)-f(0)) - because -f(0) kills the "b" part, and then we have f(x)-b linear, so see above.

Examples:
* Cumulative sum, as applied to a vector length n, and producing vector lengths n. A lower triangular matrix of ones.
* Difference matrix: the opposite of that, and not square, as takes n-long x, and outputs n-1-long with elements x2-x1 etc. Elements of this matrix are like -1 1 shaped into a diagonal, which fits fine as the matrix has a shape of (n-1)n.
* First-order Taylor approximation: f(x) ~ f(x0) + Df(x0)(x-x0) where D = [[Jacobian]], or matrix of all possible derivatives of coordinates of f over coordinates of x: Df_ij = df_i/dx_j . Obviously affine.

# Examples of linear equations

Ax = b means that b is a linear combination of columns in A. If x like that exists, then b can be represented via columns of A. An example: **Polynomial interpolation**: $y_i = \sum c_k x_i^k$ . If we consider  A with elements a_ij = x_i^j , this turns into a matrix equation. And an A like that is called **Vandermonde matrix**.

**Markov model**, aka linear dynamics model : states in next moment of time x(t+1) = Ax(t). A is a matrix of transitions. (There may also be an input with matching dimensions). Example of a model: pandemics (not as a network model, but a basic model of transitions: health -> infected -> either recovered or dead).

Same approach can be used to numerically solve DE, as x = x + x'dt, so A = (1 dt ; 0 1) where dt is an integration step for Euler integration; would take (x, dx/dt) and produce a new pair.

_Th:_ A composition f(g(x)) of two linear functions is also linear. That's obvious.
_Th:_ A composition of affine, is also affine. Also obvious.

Example: **Second difference matrix**: a multiplication of two different matrices $D_{n-1}D_n$ . The shape of this matrix is obvious from matrix multiplication. Consider chain rule for h=f(g(x)) , where everything is multivariable. Then $dh_i/dx_j =  \sum_k dh/dy_k (g(x)) \, dg_k/dx_j$ , which totally looks like a vector equation. Or rather a matrix, as this is true for every coordinate i . A matrix of all possible derivatives of elements of f, taken along each of the dimensions (coordinates) of g, is multiplied by a matrix of all possible coordinates of g, taken over dimensions of x. Dh() = Df(g()) Dg().

If h() yields a scalar, then a vector of dh/dy is just the gradient ∇h, in which case we get:
$∇h = (∇f^T ∇g)^T = ∇g^T ∇f$.

If g() is affine Ax+b, then ∇g = Aᵀ, and so ∇h = Aᵀ ∇f(). (vlms p185)

> Note: VLMS book explicitly defines gradient as a column-vector (p 36), so when they multiply a gradient by a vector, they have to flip the gradient. My understanding is that it is a non-standard **layout convention** (see [[calc4]]). That's why above ∇(Ax) = Aᵀ, and not just A, as in most books. #todo : fix the 2 paragraphs above into a "normal" convention.

# QR factorization

**Gram matrix** or **Gramian**: for a set of vectors {v_i}, a matrix of all possible inner products: G_ij = v_i ∙ v_j. Sometimes people use the word "Gramian" for the determinant of this matrix. (ref: wiki)

If v_i are arranged into a matrix A (as columns), Gram matrix can be defined as G = A'A. For a wet of orthonormal {v}, A'A = I.

Properties: a transformation defined by an orthonormal matrix Ax→y preserves inner product. Indeed: (Ax)'(Ab) = x'A'Ab = xb. As a consequnce, |Ax| = |x|. And so angles between vectors are also preserved.

**Gram-Schmidt algorithm** for finding a set of orthonormal vectors {q} that span a set of vectors {a}. And also, identifies whether {a} are independent, but finding the first linear combination. Repeatedly, 1) subtract projections on all previously found vectors (proj(ai→qj) = (qj'∙ai)qj ), 2) check if you didn't get a zero (in which case - quit!), 3) if not zero, normalize.

```
for i in (1:k):
    q_i = a_i - Σ_j (q_j'∙a_i)q_j
    if q_i is 0: stop
    q_i = q_i / |q_i|
```

Why orthogonal? Prove by induction. Say, q3'∙q2 = (a3 - q1'∙a3 q1 - q2'∙a3 q2)'∙q2 = a3'∙q2 - (q1'∙a3)(q1'∙q2) - (q2'∙a3)(q2'∙q2) , which shows all key patterns. Here we consider new i and old j<i, and get crosses of j with all possible k<i. By induction q_j∙q_k is 0 for all j≠k, and the only one j=k gets cancelled with a_i∙q_j. So we get 0 for all j≠i, proving that "original q_i" are orthogonal. And then separately we notice that the actual, updated q_i, are also normal.

Now, in matrix form vectors {q} form Q. As they are orthonormal, Q'Q = I. By Gram-Schmidt column of A are in column space of Q, as they can be represented as a linear combination of them, so **A = QR**. Here Q is orthonormal, and R is upper triangular (as a_i is only expressed through q_j for j≤i).

Refs: VLMSp97 for Gram-Schmidt, VLMSp190 for QR.

**Fun claims** (from VLMS exercises):
* Orthogonal matrix with all elements non-negative a_ij≥0 has to be a permutation matrix (a_ij = 0 or 1, with only one 1 in each col)
* A block-matrix (U U; V -V)/sqrt(2) made of orthogonal U and V is also orthogonal
* K-means in matrix form: X ~ 	ZC where Z is a matrix of means, and C is a selector matrix (c_ij = 1 if vector i is assigned to a cluster j). Now with this limitation on C, k-means becomes a matrix optimization problem.

# Inverses
Left inverse: X for A such as XA = I. Not necessarily unique.

_Th:_ If left inverse exists, then columns of A are linearly independent. Proof: Say they aren't. Then ∃ a lin comb of cols of A with non-zero coeffs x that gives 0, or Ax = 0. But as left inverse exists, ∃C: CA = I. Then we can write x = Ix = CAx = C0 = 0. Contradiction with prior assumption that x≠0. □

As a consequence, only square or tall matrices are left-invertible (wide necessaritly don't have linearly independent cols).

Similarly, right inverse. Full inverse = both left and right. Therefore, invertible matrices has to be square. Five equivalent statements: that A is invertible, that it has left inverse, right inverse, all cols are independent, and all rows are independent. (Easy to prove: from side-inverse you prove independence (above), from there ability to serve as a basis, represent unit vectors in this basis, thus proving other-side inverse existence).

If full inverse for A exists, then Ax = b has a solution for every b.

**Dual bases**: columns of A, and rows of B form 'dual bases' if AB = I (for invertible A). Inner-product with vectors of {B} give coefficients in the basis of {A}. _What does it mean? Why is this name a thing? Where is it important? Figure it out and clarify it here._ #todo

A **Triangular matrix** with all non-zeroes on the diagonal, is inversible. That's because all cols are linearly independent, which is because Lx = 0 implies x=0 (by induction across all coordinates of 0, as it settles all elements of x to 0. For a lower triangular matrix, do induction from 1 to n; for an upper trianguler: from n to 1).