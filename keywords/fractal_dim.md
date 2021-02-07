# Fractal dimension

#fractal

Parent: [[complexity]] / [[fractal]]
Related:

# Box-counting dimension

Aka **Minkowski–Bouligand** dimension. If you have a collection of points, a curve, or a shape in some space, start covering it with rectangular grids (boxes) with side size ε, counting the number of boxes that covers the set (has at least one point from the set inside). Denote this value as $N(ε)$.

Then $\displaystyle d(S) = \lim_{ε→0} \frac{\log N(ε)}{\log(1/ε)}$

In a trivial (non-fractal case), for a k-dimensional cube of linear size L, you'll need $N = (L/ε)^k$ boxes, so $d = \lim \frac{k(\log(L) + \log(1/ε))}{\log(1/ε)} = k \log(L) \cdot \lim \frac{1}{\log(1/ε)} + k = k$, as the first part of this formula converges to 0.

Sometimes the limit doesn't truly exist, but has lower and higher bounds, that can also be used in practice.

May be slightly adjusted and re-defined for non-square non-box coverings (say, ball packing, or ball covering).

Footnotes:
* https://en.wikipedia.org/wiki/Minkowski%E2%80%93Bouligand_dimension

# Hausdorff dimension

Related, but much mathier, and may be smaller than the Minkowski dimension. It's also about coverings (ball covering in this case).

https://en.wikipedia.org/wiki/Hausdorff_dimension