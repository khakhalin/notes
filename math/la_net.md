# Linear algebra and networks

#networks #linalg #bib #math

Parents and siblings:
* [[linalg]] - linear algebra
* [[graph_intro]] - introduction to graphs
* [[graph_th]] - graph theory of discrete kind
* [[algos_graph]] - algorithms on graphs
* [[09_Graphs]] - ML for graphs

# Adjacency

For a directed graph with **Adjacency matrix** A, the k-th power of this matrix ($A^k$) gives the number of paths length k that lead from each node i to each j.

# Flows and incidence matrix

If $f$ is $N_E$-long vector of total flow in each edge, and $M$ is **incidence matrix** (see [[graph_intro]]),  then $Mf$ = total flow into each vertex (as after multiplication, f will be subtracted from i, and added to j). If the flow is balanced (no accumulation of stuff in nodes), Mf = 0. If it isn't balanced (there are sources that form an $N_V$-long vector $s$), then $Mf = -s$, or $Mf + s = 0$. This obviously turns any flow (diffusion) task into a matrix equation.

And the other way around, if nodes have potentials {v}, then $M'v$ is a vector of differences in potentials for each edge. The measure of how high these differences are is called **Dirichlet energy**: $D(v) = \sum M'v ^2  = \sum_{i→j} (v_i - v_j)^2$. That is, the sum of squared differences in v across every edge. (Wikipedia calls it "Dirichlet sum", which is less fancy)

# Laplacian

**Laplacian matrix**: L = D-A where A = adjacency, D = degree matrix (with degrees on the diagonal and zeros everywhere else: d_ij=0 ∀j≠i; d_ii = degree of i_th node). For oriented graphs, we have two different Laplacians, depending on whether you choose to work with in-degree or out-degree.

> What does a laplacian of an unoriented graph mean in practice?

> What do 2 oriented grpah laplacians mean in practice?

**Theorem:** L = M'M . **Proof:** $L_{ij} = ∑_k M'(ie)M(ej) = ∑_k M(ei)M(ej)$. Here we use e as a running variable, as a foreshadowing, as M(ei) indicates whether an edge e really goes out of node i (-1), or towards node i. Therefore, if i≠j, M(ei)M(ej) will give 0 for all edges that don't connect i and j, and -1 for the only one edge connecting them (as we'll multiply -1 for i by 1 for j). If i=j, it will give +1 for every edge that either starts (-1 ∙ -1) or ends (1 ∙ 1) at j, so we'll get the total degree of D. That's assuming no loops. But M cannot represent loops anyway, and L cannot represent loops either (if diagA≠0 because of a loop, it gets subtracted from D, so we have d_ii-1 on the diagonal of L instead of a d_ii). QED.

Dirichlet sum seems to be related to Laplacian quadratic form, but for now I'm not sure it's the same Laplacian; may be one of its normalized cousins: https://en.wikipedia.org/wiki/Laplacian_matrix

### Other weird Laplacians

> According to Wikipedia, there are like 6 alterenative formulations for a Laplacian, but let's park it for now.

# Graph Spectra

#todo