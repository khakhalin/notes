# Isomap

#dim

Parent: [[04_Features]]
See also: [[mds]], [[umap]], [[dijkstra]]

Non-linear dimentionality reduction. Gets a **neighborhood graph**. Calculates a **geodesic metrics** between points = a sum of point-to-point distances along the closest path connecting two points (Dijskra's algorithm: [[dijkstra]]). Then follows with a linear dim reduction method, such as spectral embedding (top N eigenvectors of the geodesic matrix) as coordinates for the embedding (see [[mds]]).

Invented in 2000.

# Refs

The original paper:
Tenenbaum, J. B., De Silva, V., & Langford, J. C. (2000). A global geometric framework for nonlinear dimensionality reduction. science, 290(5500), 2319-2323.
http://web.mit.edu/cocosci/Papers/sci_reprint.pdf