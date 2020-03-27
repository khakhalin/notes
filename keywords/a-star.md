# A-star (A*) search algorithm

#graphs

Placement: [[algos_graph]] / [[dijkstra]] /

Graph traversal, aka path search, aka travel routing algorithm. Similar to Dijkstra's, but improved by using a heuristics. As with Dijkstra, we have a weighted graph, and we are building a tree, trying to reach from a source node to the target node.

It also tries to extend the "best node", but instead of only relying on the distance from the root g(n), we'll use a sum g(n) + f(n), where is a heuristic "guess" about how helpful this node could be, in terms of reaching the target (expected distance from n to the target). To find the node with minimum g+h we use a priority queue, as always.

The h(n) is problem-specific: for example, on a map, it can be a straight-line distance to the target.

A good h(n) (aka monotone or consistent) has the property of h(i)-h(j) ≤ d_ij, where d_ij is the length of edge from i to j. With this additional requirement, A* always finds a solution, and no node has to be processed twice (we never have to go back).

But then apparently sometimes you want to use a suboptimal h(i) as it trades optimality for speed (?). Ref: https://en.wikipedia.org/wiki/A*_search_algorithm#Bounded_relaxation