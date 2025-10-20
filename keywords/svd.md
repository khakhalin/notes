# Singular Value Decomposition, or SVD

#linalg

Parents: [[linalg]], [[features]]
Related: fourier transform, [[pca]]

A = UDV, where U and V are unitary (orthonormal, may be complex), and D is diagonal with real positive d_ii elements. Dimensions: mn = mm∙mn∙nn. More general than spectral decomposition, as it applies to all matrices on C, unlike QDQᵀ (where Q is orthonormal; see [[pca]]) that only works for symmetric matrices, or a slightly more general PDP⁻¹ that works forall  diagonalizable matrices (which is a circular name of course, but that are defined (_???_) as those for which sum dim eigenspaces = dim A _What does it mean? Refresh please!_).

The idea is to tailor the coordinate system to the task, to make it most useful for working with this particular data.

# Refs

A beginning of a lecture series about SVD: https://www.youtube.com/watch?v=gXbThCXjZFM&list=PLMrJAkhIeNNQV7wi9r7Kut8liLFMWQOXn&index=21

SVD and image compression: https://www.youtube.com/watch?v=DG7YTlGnCEo