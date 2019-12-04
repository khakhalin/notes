# Linear algebra and networks

**Adjacency matrix** (obv.). For a directed graph powers of A (A^k) give the number of paths length k that lead from node i to j.

**Laplacian matrix**: L = D-A where A = adjacency, D = degree matrix (diagonal, with 0 at i≠j and d_ii = degree of i_th node)

For oriented graphs: apparently, 2 different Laplacians, depending on whether you choose in-degree or out-degree? TBC

**Incidence matrix**: M(e,i) = 1 if edge e goes i->j, -1 if edge e goes  j->i exists, 0 for all other cases. Not square: E rows (number of edges), N columns (number of vertices).

If f is E-long vector of total flow in each edge, then Mf = total flow into each vertex. If the flow is balanced (no accumulation of stuff in nodes), Mf = 0. If it's not balanced (there are sources: a V-long vector s), then Mf = -s, or Mf + s = 0.

This obviously turns any flow (diffusion) task into a matrix equation.

And the other way around, if nodes have potential (v), then M'v is a vector of differences in potential. The measure of how high these differences are is called **Dirichlet energy**: D(v) = sum M'v ^2  = sum_e (v_i - v_j)^2 . That is, sum of squared differences in v across every exiting edge. (At least it's claled that way according to VLMS book;  Wikipedia seems to disagree and calls it 'Dirichlet sum', which is less fancy)

**Th:** L = M'M . 
Proof: L_ij = sum M'(ie)M(ej) = sum M(ei)M(ej) . If i≠j, M(ei)M(ej) will give 0 for all edges that don't connect i and j, and -1 for the only one connecting them. If i=j, it will give +1 for all edges that either start or end at j, so it will give a degree of D. That's assuming no loops. Because M cannot represent loops, but L cannot represent loops either (if diagA≠0, it will get subtracted from D). So QED.

Dirichlet sum seems to be related to Laplacian quadratic form, but for now I'm not sure it's the same Laplacian; may be one of its normalized cousins: https://en.wikipedia.org/wiki/Laplacian_matrix

### Other weird Laplacians

According to Wikipedia, there are like 6 alterenative formulations for a Laplasian, but let's park it for now.

### Spectra of A and L