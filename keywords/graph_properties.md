# Measuring network properties

#graph

Parent: [[09_Graphs]]
See also: [[graph_intro]], [[graph_generate]], [[algos_graph]], [[graph_th]]

Subtopics and papers:
* Clustering coeff
    * [[Bojanek2020cyclic]] - triplet motifs in oriented graphs



**Four main ways** to provide a large-scale characteristic of a graph:
* Degree distribution P(k)
* Something about path lengths h
* Clustering coefficient C
* Connected component sizes s

**Degree distribution**: Prob that a node has a degree of k. P(k) = N(k)/N . (For a directed graph, you have to work with 2 distributions: P_in and P_out.)

IRL degrees are often extreme: power distributions (most nodes are barely connected, very few nodes are hugely overconnected). Need to be plotted in double-log coordinates.

**Path**: a sequence of nodes, where each 2 conecutive nodes are linked with an edge. May interset each other, pass through every node more than once. We can now define a **distance**: a length of a shortedst path connecting a pair of nodes (sometimes called geodesic distance). If not connected, set to infinity. In directed graphs, distances are not symmetric, as we can only go along edges. For a weighted graph, minimal sum of edges connecting 2 nodes (note: that's only true for distance-like graphs, not connectivity-like graphs).

**Graph diameter**: Maximum shortest path distances between any 2 points in the graph. 
$\max(h_{ij}) = \max(\min(length(path: i → j)))$. 
Aka the distance between two farthest points in the graph. Upper bound for distances. Not very useful for giant graphs, as it is fragile: in scale-free networks, the bulk of the graph is actually rather compact, and a single long chain-like tentacle can throw the diameter off a lot.

**Average shortest path length**, or average distance: $\bar h=\frac{2}{N(1-N)}\sum_{ i<j} h_{ij}$. If graph is not connected (∃ij: h_ij = ∞), we may have to ignore unconnected pairs of nodes. Or only do the calculation for the strongly connected component of the graph.

> Potential homeworks: for a given graph, calculate the distribution of distances between the points. How does the distribution of distances characterize the shape of the graph? What graph of a given N and E (and thus a fixed average k) would have maximal average distance? Which graph will have it minimal? If we generate a bunch of random graphs with fixed N and E, can we somehow correlate average h to degree distributions we got?

**Clustering**: connectedness of 1-neighbors of each node. First calculate it for every node: what's the probability that 2 neighbors of this node will be connected? (C_i ∈ 0, 1) The total number of neighbors is the degree k_i, so the total possible number of inter-friend connections is k(k-1)/2. If we divide the actual number of inter-friend connections on this max possible value, we get the probability. If the node has degree 1, C_i is undefined (0/0), so just skip it.

> Todo: there's a good formula for clustering coefficient; can we derive it?

> It may be sometimes interesting to compute clustering coeficients for nodes of different degree. Potential homework: generate some large random network, compute (and plot) not just one C, but C(k).

Not very well defined for undirected graphs. (It becomes more of a motif thing, although see [[Bojanek2020cyclic]] - "motif-bound clustring coefficients" or "triplet clustering coefficients").

**Average clustering coefficient** for the entire network: just the average of clustering coeff for all nodes. Typically, very high in social networks.

**Graph connectedness**: the relative size of the **largest connected component** (aka **giant component**). s = N(giant component)/N . For undirected graphs, found with BFS, labeling nodes as you go. For directed graphs, it's typically about largests **strongly connected component**, which can be found using Kosaraju-Sharir algorithm [[algos_graph]].

For very large, and not fully connected networks, it may also make sense to build a histogram of component sizes. There will be a giant component, most probably, but it may be interesting how quickly tiny components taper off.

> Potential homework: model these properties for Erdos graphs, or preferential networks. Compare the distributions.

An IRL example (typical numbers): some large social network. N=10^8, ⟨k⟩ =15, ⟨h⟩  =7, C=0.11, 99.9 in a giant component. Protein interactions network: N=2000, k=3, h=6, C=0.12, s=81%.

# Refs

Lecture by Jure Leskovec
https://www.youtube.com/watch?v=dD6LRgw_2mQ&list=PL1OaWjIc3zJ4xhom40qFY5jkZfyO5EDOZ&index=1