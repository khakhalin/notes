# Spectral radius

Parent: [[graph_spectra]]

#spectral

For a matrix, the maximal absolute eigenvalue of W matrix: $ρ = \max_i λ_i$. For a graph, a radius of its adjacency matrix.

In [[python]]:
* One way is to use `numpy.linalg.eigvals(a)`, then take the biggest one. But it finds all eigenvalues; not sure how much slow-down it would be.
* Another option: `scipy.linalg.eigh(a, eigvals=(N-1,N-1))` where N is the rank of matrix a. Also set `eigvals_only=True` to get only λs, without eigenvectors.

# Refs

https://en.wikipedia.org/wiki/Spectral_radius

Advice on Python
* https://stackoverflow.com/questions/12167654/fastest-way-to-compute-k-largest-eigenvalues-and-corresponding-eigenvectors-with
* https://docs.scipy.org/doc/scipy/reference/generated/scipy.linalg.eigh.html
* https://numpy.org/doc/stable/reference/generated/numpy.linalg.eigvals.html