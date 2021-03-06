# Bridson's algo for Poisson-disc sampling

Parents: [[algos]]

#algo

To create a nice uniform-looking coverage of points on a 2D plane. Uniformly distributed random samplings are really clumpy and have a weird distribution of inter-points distances. While we want a distribution of distances with a sharp peak (not quite regular, but not that far from a regular grid actually).

Algo:
0. Pick a point, add it to a list of active points
1. Loop until the list is empty:
    2. Pick a source point from the list
    3. Attempt 100 times:
        4. Sample a new point in a ring between R and 2R from the source point
        5. If the new point is further than R from any existing point, consider it a success and leave the loop. Otherwise, keep sampling.
    6. If the loop above failed (we couldn't find a good new point after 100 attempts), remove "source" point from the list. It is now "dead", ,and won't be considered for sampling again.
    7. Otherwise, add the newly found point to the list.

The point 5 above is a bit tricky, as we really don't want to compare our new point with every other point; to keep it fast we need to keep it local. One way: to keep a grid or $r/\sqrt{2}$ (in which case each grid square will contain either 0 or 1 points), and for a new point, only look at grid cells near it. So we'd have a grid of links to points. For a candidate, we'd take its `x,y`, round them to get `i,j` of the grid element, and only compare with points referenced in grids `i-1 : i+1, j-1 : j+1`.

Complexity: O(N). _Is it true? One of the fan pages mentions that, but I haven't seen the derivation_

Originally invented in the context of anti-aliasing.

# Refs

In Visualizing Algorithms by Mike Bostock: https://bost.ocks.org/mike/algorithms/

A shorter explanation: 
* Algo: https://bl.ocks.org/mbostock/dbb02448b0f93e4c82c3
* Demo (in js): https://bl.ocks.org/mbostock/19168c663618b7f07158

Just a little animation based on it: https://www.jasondavies.com/poisson-disc/

Wiki page on aliasing that mentions the algo: https://en.wikipedia.org/wiki/Supersampling#Poisson_disc