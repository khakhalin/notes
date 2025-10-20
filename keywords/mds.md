# Multidimensional scaling

#dim

Parents: [[features]], [[subspace_methods]]
See also: [[embedding]], [[isomap]], [[spectral_clustering]], [[graph_spectra]]

A way to embed high-dim points into a low-dim space by minimizing a certain loss, called **strain**, between pairwise distances in the original space, and point positions in the new space.

> From Wiki it's not quite clear what is really optimized here, and how this strain works. Lower on the page they hint that we're minimizing the squared distance between real distances and embedded distances. Which is equivalent to maximizing the correlation between the two, if I'm not mistaken. Which is the way I think I've seen it presented before. But what is this "strain" thing? Not sure.

Algorithm for **classic MDS**:
1. For every pair of points, calculate a square distance between them, form a matrix D. We explicitly assume Euclidean distances here, so the method will break for custom dissimilarity metrics.
2. Remove the mean from each row and column, aka **double-centering**. Analytically, it may be represented as left and right multiplication on a certain "centering matrix"
3. Calculate M largest eigenvectors of this double-centered matrix. Use them as coordinates.

# Refs

Wiki
* https://en.wikipedia.org/wiki/Multidimensional_scaling
* https://en.wikipedia.org/wiki/Centering_matrix

