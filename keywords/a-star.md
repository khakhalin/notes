# A-star (A*) search algorithm

Parent: [[algos_graph]]
Related:  [[dijkstra]]

#graph #algo


Graph traversal, aka path search, aka travel routing algorithm. Very similar to [[dijkstra]]'s, but improved for this specific purpose by using a heuristics for the expected _remaining_ cost. As with Dijkstra, we have a weighted graph, and we are building a tree, trying to reach from a source node to the target node.

Also as with dijkstra we always try to extend the "best node" (by keeping track of all extendabe nodes in a [[priority_queue]]), but instead of only relying on the distance from the root g(x), we'll use a sum g(x) + h(x), where h(x) is a heuristic "guess" about how helpful node x could be, in terms of reaching the ultimate target ($h(x)$ is the expected distance from n to the target).

The choice of heuristic h(n) is problem-specific: for example, on a map, it can be a straight-line distance to the target, while on hard terrain it can be something closer to the manhattan distance. A good h(x) (monotone or consistent) is one with a property of h(i)-h(j) ≤ d_ij, where d_ij is the actual (post-factum) length of the edge from i to j. With this additional requirement, A* always finds a solution, and no node has to be processed twice (we never have to go back). That's because with this requirement you can never "cheat" by getting "negative distances" after you have already invested in some part of a graph; your "current best route monotonously increases".

But that said, apparently, sometimes you want to use suboptimal h(i), trading optimality for speed.
Ref: https://en.wikipedia.org/wiki/A*_search_algorithm#Bounded_relaxation