# Dijkstra's Shortest Path First algorithm

Parent: [[algos_graph]]
Related: 
* [[a-star]] - a heuristic improvement upon Dijsktra's
* [[floyd_warshall]] - similar, but calcualting all-to-all shortest paths for a dense graph
* [[priority_queue]] - a data structure used in Dijsktra

#algo #graph


Finding a shortest path between two points (or to all points from a starting point) on a weighted graph. The idea is simple: do something like Breadth-First-Search, but instead of marking how far you are from the starting point in terms of the number of transitions, keep track of the current min() distance to each node.

The algo:
1. Mark all nodes as unvisited, and set their distances from the root to +inf; set root to 0.
2. Pick a node with minimal distance from the root. (Footnotes 1-2 here). Mark it as visited.
3. For all unvisited nodes j connected to the current node i, update their distances: `d_j = min(d_j , d_i + e_ij)`, where e_ij is the weight of the edge connecting them. (Footnote 3 here)
4. Repeat.

Now the footnotes:
1. If the footnote with minimal distance is Inf, it means that we covered the entire connected part of the graph, and all that remains is disconnected. We can stop.
2. What is the best (fastest) way to find the minimum? The best way of course is to create a priority queue ([[heap]]), and store all values d_i there, so that we always have the minimal value on top of the heap.
3. When looking for a distance to some particular node, once we have found this node, we can stop. And in fact, we can stop when we update the distance to this node, not when we select it as current, so at step 3 of the algorithm above, not at step 2.

**Complexity:** If the queue is just naively stored in a list, it's O(E+V²) = O(V²). With a really well-optimized well-balanced priority queue it's O(E+V log V). For a simple unbalanced priority queue it is a bit worse than that, but not by much. (ref: [wiki](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm#Running_time))

# Refs

* https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm