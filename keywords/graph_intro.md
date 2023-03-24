# Graphs Intro

Parents: [[09_Graphs]]
See also: [[graph_properties]], [[algos_graph]], [[la_net]]

#graph


**Motivation**: everything is complex systems (agents), and information (exchange of it between some… nodes?), so graphs and networks are everywhere. Knowledge itself is best to be represented as a graph (relations of items, elements). Human society is a graph, and our brains are a graph, and everything is a graph :) Universal language.

Four typical tasks on graph data:
* Node classification (some property assigned to each node)
* Predict edges (whether 2 nodes are linked)
* Identify communities (clusters, modules)
* Compare networks to each other (network similarity)

### Basic definitions

**Graph**: Nodes N, connected by edges E, form a graph G(N,E)

Often **Network** is used for real system, and **Graph** - for an abstraction that describes it.

Examples: friendships, collaborations, sexual relationships, internet, scientific collaborations, neurons, interacting proteins, sales, product shipments, movie and music preferences, movies and actors, food and ingridients.

**Degree**: number of adjacent edges, k. For directed graphs, we differentiate between in-degrees and out-degrees. $\sum k^{in} = \sum k^{out}$ (as every edge has two ends). Average degree $\bar k = \frac{1}{N} \sum k_i = \frac{2E}{N}$ (for undirected graphs; for directed we'll have half that for both in- and out-degrees).

**Complete graph**: all possible edges, N(N-1)/2 edges, average degree of N-1. Small complete graphs within a large network are called **cliques**.

Real networks are very sparse (average degree is really low).

**Bipartite graphs**: Two types of nodes, and only nodes of opposite types may be connected. It's possible to go from a biparatite graph to a "simple" graph by **folding** of **projecting** them: say, for a scientific collaboration graph (Authors, Papers), get a graph that all authors who ever collaborated, or all papers that share at least one author.

### Ways to represent a graph

* **Set of edges**: elementary, abstract, but hard to use in practice
* **Adjacenty list**: a dictionary (hash table) of lists: for each node, a list of nodes it is connected to. Ideal for most algorithmic tasks. Flexible and memory efficient.
* **Incidence matrix**: if the graph is not oriented, for each edge e, connecting nodes i and j, we have $M(e,i) = M(e,j)=1$. For all other elements of this row $M(e,k)$ with k≠i,j we have $M(e,k)=0$. If the graph is oriented, and the edge e runs i→j, then $M(e,i)=-1$, while $M(e,j)=1$. This matrix is obviously not square: it has $N_E$ rows (number of edges), and $N_V$ columns (number of vertices). And if there are bidirected edges in a directed graph, we have to dedicate 2 rows to it (there, and back). It also means that at least in a directed graph, M cannot represent loops, as something cannot be equal to -1 and 1 at the same time.
* **Adjacency matrix**: a_ij = 1 means that nodes i and j are connected. Symmetric for undirected graphs. Sums of columns and rows are node degrees (in and out respectively). Power of linear algebra! But as networks are sparse, typically matrices are sparse as well, which means that working with them may be memory inefficient.

### More terminology

* **Weighted graphs**: each edge now carries a number, encoding something like weak and strong connections, or distances of some sort etc.
* **Multigraphs**: may have more than one edge between any given 2 nodes. Can be modeled with weighted edges.
* **Loops**: nodes linking to themselves (non-zero elements on the diagonal in A). Are sometimes prohibited.
* **Path**: a sequence of nodes, so that each next node is linked to a prev one with an edge. Or a sequence of edges leading to each other.

**Connectedness** or connectivity of a graph: if there's a path from one node to another.
* For undirected graphs - simple, just see whether you can reach one node from another. For an algo, see [[union-find]]
* For directed graphs: weak and strong connectivity. **Weak** ignores edge directions, as if the graph was undirected. **Strong** requires that you only go along the edges, and you can reach _both_ from node v1 to v2, and back. For a practical algorithm, see [[kosaraju-sharir]] algorithm.

* [[bridge]]: if you remove this edge, it breaks the graph into two unconnected components.
* **Articulation node**: similar, but for a node. If you delete it, the graph breaks into unconnected parts.

If a graph is not connected (can be broken into several disconnected graphs), the adjacency matrix can be represented as a block matrix $\begin{pmatrix} B & 0\\ 0 & C \end{pmatrix}$, but we may have to re-arrange lines and columns. A "barely connected" graph will have "almost zeros" in these blocks, and this logic is used for community detection.
* **Cycle**: a path that leads back.
* **Acyclic graph**: a graph without cycles. 

# Refs

https://snap-stanford.github.io/cs224w-notes/preliminaries/introduction-graph-structure