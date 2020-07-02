# Elastic Map

#dim #features

Parents: [[04_Features]] (for a list of MDS and alike), [[02_Regression]]
See also: [[regularization]]

**Elastic map** is a way to transform one reasonable distribution of points to another reasonable distributin of points; kinda like MDS, but without changing the number of dimensions necessarily. You do it by modling a physical object: a sort of a grid of connecting points, and making this object elastic, but only to a measure.

In a classic example, on top of a target loss (bring points to where they want to be) you also add a constraint loss that consists of 3 items:

**Approximation energy**, or a distance from all data points x_i to its **host node** w_k. Host nodes are a (much smaller than {x}) set of nodes, each equipped with its vicinity Ω_k. Each data point is only constrained by its host node (so no triangulation for data points itself, no pairwise interactions, etc.)

$\displaystyle \sum_{k} \sum_{i \in Ω_k} (x_i - w_k)^2$

This term allows us to encode large-scale transformations of the cloud more concisely, as instead of triangulating around every point and considering all edges, we do a nested approach: project each point as a vicinity of its host points, and then large-scale transform the skeleton of host nodes. A hierarchical approach, more computationally effective.

**Stretching energy**, or **membrane term**:

$\displaystyle λ \sum_{ij} (w_i - w_j)^2$

**Bending energy**, or **thin plate term**:

$\displaystyle μ \sum_{ijk} (w_i + w_k - 2w_j)^2$

Not to be confused with ElasticNet, which is a combo of L1 and L2 regularizers.

It doesn't seem that sklearn would have a ready plug-and-play version of it. Unless they call it differently.

# Refs

https://en.wikipedia.org/wiki/Elastic_map