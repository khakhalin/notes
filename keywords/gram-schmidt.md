# Gram-Smidt

Parents: [[linalg]], [[02_Regression]]

#linalg

> #todo: this description doesn't make sense currently. It's supposed to summarize ESL p54, but I cannot understand it upon re-reading. Rewrite!

GS is a computationally solid approach to calculating θ, which is much better computationally than calculating an inverse matrix. The idea: lets's break (decompose) y into x_j and everything else, iteratively, as if the basis was orthonormal; calculate everything that remains, continue.

**Algorithm** (without normalization):
1. Set xi (the current best guess for x) to x0
2. Replace xi with its projection to the complement of all previously considered coordinates: $q_i = x_i - ∑_j\text{proj}(x_i→q_j)$ $= x_i - ∑_j q_j⟨x_i,q_j⟩/⟨q_j,q_j⟩$ where j=0 to i-1. Remember all scalar products from this formula as $⟨x_i,q_j⟩=c_{ji}$.
3. Find the best projection of y onto qi: γi = ⟨y,qi⟩
4. Shift "current" x from xi to x_i+1. Go to step 2.

This produces a split of X into X = QR where Q with dimensions (N × p+1) is column-orthogonal, and R with dim of (p+1 × p+1) that is upper triangular, with elements cji=⟨xi,qj⟩/⟨qj,qj⟩ (in case or not-normalized basis, 1s on the diagonal). And we get a set of estimates {γ_i} that represent y in the basis Q.

Now as X = QR, our Xθ = y becomes QRθ = y, but we also just found y = Qγ, so Rθ = γ. It gives θ = R⁻¹γ. 

> What I still not quite understand here is why R remains, meaning that we still have to calculate an inverse for it. Could not we somehow manage to tuck it into the calculation as well, to get θ directly? And if not, how do we calculate the inverse in practice? (How to make it computationally effective)? Park for now.

Refs: ESL p54, [some lecture](http://homepages.ulb.ac.be/~majansen/teaching/STAT-F-408/slides01multipleregression_4.pdf)

In a general case, coordinates of X may be dependent, or be very close to it. In this case, XᵀX from the general formula is singular, and doesn't have an inverse, meaning that θ is not uniquely defined. If we use orthogonalization, and two columns of X are highly correlated (say, x1~x2), then x1 will get a normal score, but on the next step we'll have to face a vector that is pure noise: q = x2-proj(x1→x2) ~ x2-x2 = 0. Then both qᵀy and qᵀq will be very small, and θ_2 = qᵀy/qᵀq will be unstable. Estimate for variance: var(θ_2) = σ²/qᵀq. Practical consequences for variable selection (dangers of collinearity): **conjunctive dim reduction**, **ridge regression** (why not just zero this θ if it's so bad?) etc.

If Y is multivariate as well, but all errors in Y are independent, we just have several independent linear regressions. If errors in Y are correlated, the very definition of RSS is changed (to encorporate the covariance matrix), but we don't go deep into it here. Refs: ESL p56
