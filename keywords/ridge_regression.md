# Ridge regression

#regularization

Parents: [[02_Regression]]
See also: [[regularization]]

Also known as **Shrinkage Methods**, closely related to **Tikhonov regularization**. Modern DL name: **L2 regularization** (see DL chapter).

Motivation: Consider Ax = b, and x doesn't exist. In a most typical practical case, it gives a superposition of an over-determined (still unsolvable) problem Aξ = b for the component of x that is in the row-space of A ( ξ = proj(x→RowSpace(A)) ), and so is affected by the actual value of the matrix A, and an under-determined problem A(x-ξ) = 0 for the components x-ξ that are in the null-space of A, orthogonal to the RowSpace(A), and thus projected to 0. (A reminder: null-space is orthogonal to the row-space, as from Ax=0 we can conclude that x is orthogonal too every row of A). But it means that the part x-ξ can be chosen arbitrary without any effect on the value of Ax. So we are free to pick this part in some "nice" fashion, to satisfy some priors, for example, or minimize solution complexity. 

Compared to variable selection, this approach is smooth and stable: while with model (variable) selection loss may oscillate pretty wildly as you add more features, with shrinkage it usually changes monotonously.

This is especially important in case of **Multicollinearity**, when you're trying to predict y from many variables X as y = Xθ, but some of x_i (columns = variables) are strongly correlated. In this case trying to painfully minimize y-Xθ would be counter-productive, as we'd be trying to fit noise in y with noise in x_i. Imagine an extreme case: all p columns of X are the same (rank=1), but are observed with noise, and noise is independent (so formally rank = p). What we actually need is only one (doesn't matter which one) θ_i>0, and all others ≡0. But what will happen, is that we'll fit noise in y with noise in X.

**Ridge regression**: we add a penalty on θ in the form of λ∑θ². Note that this sum runs from 1 to p, as $θ_0$ (intercept) is not penalized. Included in the formula for h, but is not penalized. Alternatively, one can just insist that λ∑θ²<t: because the rest of the loss is defined by {X,y}, for any given task there's an exact match between λ and t. As the values of θ depend on the scale of x_i, **standardization** of x (de-biasing + normalization) becomes critical.

> If using shrinkage methods, **always standardize your X!**

The name "**ridge**" comes from a visual example of what is describe above. Imagine that only a part of the solution is well defined, and the rest is close to the null-space of X. Then the "true solution" is a "generalized cylinder" made of the true solution (in those coordinates that make sense), which is nice, convex, and has one smooth minimum, and its arbitrary extension across all "irrelevant coordinates", as any vector from the null-space of X will be zeroed by the multiplication by X. Moreover, small changes in training data (right side of the Xθ=y equation) would wildly sway the solution along this "ridge". By adding regularization, we change a "ridge" into a "peak" (lines turn into parabolas), which stabilizes the solution against perturbations in both X and y.

**How to pick  λ?** One method: **Ridge trace**: plot found coefficients as a function of λ, and eyeball a value at which they stop oscillating, and switch to a  monotonous change regime (they never become const, but switch from curving to almost-linear, like willow twigs in a vase). A better approach, of course, is **cross-validation**. Also, for any given λ, one can estimate the effective degrees of freedom (as a trace of a certain matrix, see ESL p68), so for these plots it's common to use df(λ) for x axis, not λ itself.

Interestingly, if all x_i (columns, variables) are orthonormal, ridge regression has no reasons to penalize any one θ in particular, and just all of them get equally scaled by 1/(1+λ).

> I am not quite sure why this is the case, even though it feels like something that could be intuitive and visual. It seems that the point where an ellipse (minization of y-Xθ) and a circle (λ∑θ²) touch doesn't necessarily lie on a straight line between the centers of said circle and ellipse... #todo : figure it out!

**Tikhonov regularization**: a generalization of Ridge. In this case we minimize |Aθ-b|² + |Γθ|² where Γ is some matrix. If Г=identity matrix I multiplied by a coefficient λ, we have a sum of squared x_i, and so Ridge regression. 

> ESL p67 also seems to suggest that ridge regression is somehow related to PCA, or can be understood in terms of first moving to the PCA space, then doing something, then projecting back, but the math is sketchy, and no good comments, so I'll park it for now.

#todo : Deconstruct this post that seems to have a reasonably concise and systematic summary of all math involved in parallels between shrinkage and PCA, and in the same system as ESL: https://stats.stackexchange.com/questions/220243/the-proof-of-shrinking-coefficients-using-ridge-regression-through-spectral-dec

# Refs

* [Stackexchange](https://stats.stackexchange.com/questions/118712/why-does-ridge-estimate-become-better-than-ols-by-adding-a-constant-to-the-diago/119708#119708), on visual interpretations
* ESL p61