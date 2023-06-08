# SVM - Support Vector Machine

Parent: [[03_Classification]]
Related: [[logreg]], [[kernels]]

#classification


**General idea:** a widest street approach: find a line, so that if you have a band around the line, it separates positive examples from negative ones as best as possible. If the line is described with a normal vector ω ⊥ line, whether a point x is far from the line can be described as ⟨x , ω⟩. If b is a good threshold value for this product (that is, if we get ⟨x, ω⟩ = b when x happens to lie exactly on the separating line), we have a classificaiton rule of: xᵀω + b ≥ 0. To find the best line, let's try to optimize for ω and b, so that for positive samples we would get xᵀω+b>1 (not just >0), and for negative samples, xᵀω+b<-1. 

We have already seen linear separators f(x) = xᵀθ = 0 before (see [[logreg]]). The hyperplane (line in 2D) it defines is ⊥ to θ, and f(x) is proportinal to the distance from x to the plane.

**Rosenblatt's Perceptron** from 1958:  tries to minimize distances from misclassified points to the boundary: $D = −∑y_i(x_i^\top θ) ≥ 0$. This assumes that most y=1 lie beyond the boundary, so if one of the y=1 points is left closer to 0 than the decision line, its positive y will get multiplied by a negative xθ, resulting in a negative value. Thus a minus sign before the sum. The original perceptron used stochastic gradient descent, visiting points one by one, with the following learning rule: $θ := θ + αy_i x_i$ (as usual, in a notation that assumes the 0th coordinate being $(x_i)_0=1$ and $θ_0$ representing the intercept). Obvious problems with this approach: it's non-deterministic, slow, doesn't converge for non-separable data (instead, runs in a slow cycle).

**Optimally Seprating Hyperplanes** To make the perceptron-like approach more robust, let's maximize a different value: $M =\min_i(y_i x_i θ)$, or the smallest distance from a correctly classified point to the boundary. In other words, let's draw the hyperplane in such a way that it achieves the maximal separateion of outputs. Here the non-intercept part of θ is constrained to be unitary: $||\hat θ|| = \sum_{i=1}^p θ^2_i = 1$. Note that here summing goes from 1, and not from 0: it is the actual "vector part" of θ that is normal (direction), while the intercept $θ_0$ is not bound (we need to be able to move the hyperplane arbitrarily in space). 

> ESL always writes $y_i(x_i ᵀ β + β_0)$, showing the intercept explicitly, but that's annoying, isn't it? Other textbooks  introduce special notation with a wave $\tilde θ$ for (θ1 … θp) sub-vector, and use θ for a full vector ([ref](https://www.dbs.ifi.lmu.de/Lehre/MaschLernen/SS2014/Skript/SupportVectorMachine2014.pdf)). I for now just try to be intentionally inconsistent here.

Instead of writing the constraint on θ explicitly, we can write the equation for an arbitrary θ that got normalized: y(xᵀθ)/norm(θ) ≥ M for ∀(x,y). Now let's move norm(θ) to the right: yxᵀθ ≥ M∙norm(θ); then assume that instead of norming to 1 we'll be norming to 1/M, so that M∙norm(θ)=1. Now instead of maximizing M, se can minimize the norm of upper part (components 1 to p) of θ, producing the following task:

Optimize θ, to achieve $\min ||\tilde θ||$, provided that $y_i ⟨x_i , θ⟩ ≥ 1 \quad ∀i$. 

The value 1/norm(θ) is called **thickness** of the decision boundary. This yields a convex problem with linear constraints, solvable with Lagrange optimization: L = |θ|² - ∑ λi ( yi ⟨x_i , θ⟩  - 1)…

> At this point (ESL p133) I give up for now. It seems that math around p134 is not how SVMs are actually implemented anyways, so it can probably wait. I'll rivisit this once I read the actual SVM chapter.

In practice, SVMs are  used in combination with **Kernel tricks** (see [[kernels]]), such as radial basis kernels [[RBF]]. This produces simple yet expressive models, where the classification is encoded by a single vector (and also the set of kernels). Allegedly, as of 2010s they were still SOTA for some niche operations, but appear to have been replaced by ensemble methods ([[05_Ensembles]]) later.

# History

The name "SVM" comes from this idea of drawing a separating hyperplane, and "supporting" it with a vector. "Machine" seems to be just an aesthetic thing, even tho the method was developed in the 1990s, so it's not that ridiculously old. Invented in AT&T Bell labs, by Vladimir Vapnik, who born in the USSR, of Jewish origin, and emigrated to the USA in the 1990s.

# Refs

A reasonable post abut SVM, by Ajay Yadav:
https://towardsdatascience.com/support-vector-machines-svm-c9ef22815589

https://en.wikipedia.org/wiki/Support_vector_machine

https://en.wikipedia.org/wiki/Vladimir_Vapnik