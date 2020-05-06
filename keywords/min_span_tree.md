# Minimum Spanning Tree

#algo #graphs

Parents: [[algos]] / [[algos_graph]]
Related: [[algos_trees]]

**MST**: for an undirected weighted graph, a spanning tree (connected subgraph with no cycles) that also happenes to have a minimal sum of weights across all possible spanning trees. (May not be the ony tree with this sum, but at least there's no better one).

Assumptions:
* The graph is connected (if not, just apply in a loop to each unreached component)
* Edges can have 0 and negative weights
* All weights are different. This is not critical, as it turns out, but simplifies the reasoning.

Definitions:
* **Cut**: separation of vertices into two disjoined sets.
* **Crossing edge**: For a given cut, connects i from one half to j from another

Lemma (aka **Cut property**): for ∀ cut, a crossing edge with min w_ij ∈ MST. (Kinda obvious, as all nodes in one half will be connected, and all nodes in the other one as well, so it doesn't matter which crossing edge to use to link them, so obviously we need to use the minimal one.)

**Greedy MST algorithm**:
1. Pick any cut of a graph, making sure that no existing edges (those already added to a growing MST) are crossing edges. I.e., the entire growing MST should lie in one half of the cut.
2. Find min crossing edge for this cut, add it to the growing MST.
3. Repeat until we have V-1 edges, and it's impossible to continue.

Typically, working with weighted graphs is easier not in vertex representation (`{v:[vertices]`, as we had it for unweighted graphs), but in edge representation `(i, j, w)`. Makes it easy to iterate through edges. A good graph object for this representation would contain a list of bags (lists? linked lists?) of edges (in practice: pointers, references to edge objects) adjacent to every vertex `{v:[edges]}`.

Two main practical algorithms:

**Prim's algorithm**: add one edge at a time to a growing connected tree, looping through all untaken vertices. Some candidate edges will become redundant with every iteration, and can be either cleared up eagerly (go through all edges, remove all those that connect the new node to existing nodes), or ignored and cleared up lazily (when looking for a min edge, skip it if it doesn't help). Time: O(E log V) for eager Prim, O(E log E) for lazy prim. Acceleration from O(EV) is achieved by using a priority queue for candidate edges. Space: O(V).

In practice, for **Lazy Prim**:
* Ideally, have a list of edges for every node
* Visit first node. Visiting implies: 1) marking it as "visited" (keep an array for that), and 2) enqueuing all edges leading from it in a min-queue, using edge weight as a key.
* Now in a loop: 1) pop an edge; 2) if both points are visited, skip; 3) if not, visit the unvisited, and add the edge to the tree.

Note that if V is at least somewhat proportional to V squared, then total complexity is O(V² log V)

**Kruskal's algorithm**: include one edge at a time, in the order of w_ij, but don't bother about keeping the tree connected until it's ready. Time: O(E log E), space: O(E). To accelerate edge sorting, use priority queue. To make sure that no cycles are added, use "union find" (each connected component maintains its identity via upwards-linked tree terminating at a root that serves as a component id. If two vertices have different terminal roots, they belong to different components. And linking components is also easy). Is typically slower than Prim's. 

Curious fact: an almost-linear O(E + ...) solution for this task exists (Chazelle 1997), but it is so complcated that cannot be used in practice.