# Floyd Warshall algorithm

#algo #graphs

Parents: [[algos]] / [[algos_graph]]
See also: [[dijkstra]], [[a-star]]

Find shortest distances from every node to every other node, using dynamic programming. The main idea: let's consider paths that use only first k nodes of the graph as intermediate nodes. We start with smallest possible k=0 (direct edge, no intermediate nodes). This gives the base case of `d(i,j,0)=w_ij`. Then for each k>0, we get `d(i,j,k) = min(d(i,j,k-1) , d(i,k,k-1)+d(k,j,k-1))`: in this formula k in the 3d position stands for "paths that only go through {nodes 1 to k}", while in the 1st and 2nd positions it is just a node (kth node of the graph). So we can iterate through all k from 0 to V (the number of edges), and calculate all distances.

Note: it's not about k steps or less. It's really about "only going directly or via first k nodes of the graph", in some arbitrary graph ordering. So if there's no direct edge from i to j, early on min distances d(i,j,k) for many (i,j) will be inf. That's by design.

Works on any weighted graph, including with cycles and negative weights, as long as there are no negative cycles (no cheating!).

The complexity is O(V^3). For sparse graphs [[dijkstra]] is better, as it gives O(EV + V^2 lg V), but for dense graphs Floyd-Warshall apparently often wins in practice (unclear, but ref: wiki).

Again, the algorithm:
```
for k in range(n):
  for i in range(n):
    for j in range(n):
      d(i,j,k) = min( d(i,j,k-1), d(i,k,k-1) + d(k,j,k-1) )
```