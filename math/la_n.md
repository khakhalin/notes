# Linear algebra and networks

Laplacian matrix: L = D-A where A = adjacency, D = degree matrix (diagonal, with 0 at i≠j and d_ii = degree of i_th node)

For oriented graphs: apparently, 2 different Laplacians, depending on whether you choose in-degree or out-degree? TBC

Incidence matrix: `M(e,i)` = 1 if edge e goes i->j, -1 if edge e goes  j->i exists, 0 for all other cases. Not square: E rows (number of edges), N columns (number of vertices).

L = M'M

Proof: L_ij = sum M'(ie)M(ej) = sum M(ei)M(ej) . If i≠j, M(ei)M(ej) will give 0 for all edges that don't connect i and j, and -1 for the only one connecting them. If i=j, it will give +1 for all edges that either start or end at j, so it will give a degree of D. That's assuming no loops. Because M cannot represent loops, but L cannot represent loops either (if diagA≠0, it will get subtracted from D). So QED.

### Other weird laplacians

### Spectra of A and L