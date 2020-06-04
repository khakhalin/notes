# Mean shift

#clustering #dynamic #kernel

Related: [[03_Classification]]

A type of clustering algorithm in which clusters are not assigned explicitly, but instead points are interatively moved, like in a **dynamic system**, and end up converging to different end-points. These end points can later be used as **centroid** estimations. Officially, can be described as an algorithm to find distribution modes.

Here's what happens. Take a non-linear decreasing **kernel**(something like $K(x-x_0) = \exp(|x-x_0|)$ works well). Then for each point replace it with a weighted average of all other points, using this kernel for averaging. Like that:

$\displaystyle x ← \sum_i{(K(x_i-x)x_i)}/\sum_i{K(x_i-x)}$

What happens is that each point will "crawl" towards the weighted average of all other points, but because K() is strongly non-linear, the average will be heavily biased towards those other points that are located nearby to "our point". So points will aggregate and clump together. Eventually they will form tighter and tighter attractors, and all points in the vicinity will be collected into this attractor.

If K() is >0 everywhere, it will eventually converge to a single point; just very slowly. But in practice we put a threshold on K(), to define a "vicinity" of each point, so that points from the outside of this vicinity wouldn't affect them. Then we may end up with a set of final attractors that are not closer to each other than the radius of this "vicinity region", including, possibly, a bunch of isolated points. _At least that's my understanding, although Wiki doesn't say that explicitly._

# Refs

Wiki:
https://en.wikipedia.org/wiki/Mean_shift

Visualization:
https://twitter.com/gabrielpeyre/status/1268044743771959298