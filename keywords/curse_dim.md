# Curse of Dimensionality

#dim #clustering

Related: [[04_Features]], [[02_Regression]]
See also: [[typical_sample]]

In high-dim, volume grows so fast with linear dimensions (r^K for K different dimensions) that most of volume lies in the thin shell, not around the center.

Say, to find a subcube with 1% of total volume inside a unit-cube in 10-dim, we'd have to take a subcube with a side of 0.6 (because 0.6^10 ≈ 0.01). Only think about it: in just 10 dim (which is not even a lot!), a half-smaller cube with the same center contains less than 1% of volume of the original cube! (and thus potentially very few points from the dataset).

Which means that a neighborhood containing 1% of points won't be local in any meaningful sense of this word (it will span 0.6 of total range for each of the dimensions! It's not what most people would call local!). And if we keep the side length (r) small, we won't capture any meaningful examples.

Similarly, if points are distributed uniformly within a unit volume, the closest point to any given point is expected to be half-way to the range boundary. Especially for highly non-linear functions (say, exponents) it presents a huge problem, as the mean of neighbors no longer resembles the "true value". (ESL p23-25)

Solution: dimensionality reduction (see [[04_Features]])

# Dim curse and Bias-Variance tradeoff

* For Bias-Variance Tradeoff, see: [[02_Regression]]

Let's consider a special case of bias-variance trade-off in a case of very high dimensions. Say, we have a linear model with error: y = Xᵀθ + ε (aka **additive error model**), where ε is normal noise with variance σ². Let's try to infer θ from a training set of Y. As θ = (XᵀX)⁻¹Xᵀy, we get it with errors of (XᵀX)⁻¹Xᵀε. Which in turns means that when we try to estimate y as h = Xθ during training, we get errors for these estimations: X(XᵀX)⁻¹Xᵀε . 

Loss J for one point xᵀ (x = one row in X) is given by a bias-variance formula: J = E( (h-Eh)² + (Eh-y)² ) = xᵀ(XᵀX)⁻¹xσ² + Bias + σ². Here Bias≡0 because linear fit is unbiased, and σ² is irreducable error.

If training process is randomized enough (??? _what is the official name for this criterion?_), (XᵀX)→N∙Cov(X), and so we have:
J ~ E( xᵀCov(X)⁻¹xσ²/N ) + σ²= trace(Cov(X)⁻¹Cov(x))σ²/N + σ² = pσ²/N + σ², where p is the number of dimensions. So J grows as a linear function of the number of dimentions p.

> (This trace formula above is pretty annoying, but it works. If we E() the left part, we get a E() of ∑_ij (x_i x_j C⁻¹ij), which is formally a sum with p^2 terms. But if you open this trace(C∙C⁻¹) on the right, you get the same expression (j coming from trace-sum, and i from the multiplication of two matrices). So E() indeed gives the trace(). And then, once this is proven, we notice that trace() only runs through p terms, and all of these terms happen to be = 1 by def of CC⁻¹, and we get what we get.)

Which means that the higher the number of dimensions, the worse the error (variance). Now, some methods are worse than others: linear regression is actually faring relatively OK, while some (like KNN) get cursed very fast (ESL p26; explanation above). But the gist is the same for all methods. If you simulate this, you have diff deterioration curves for different data assumptions. IRL, we try to be in-between these two extremes (global linear regression and 1NN), using some info (or assumptions) about the structure of the data. 