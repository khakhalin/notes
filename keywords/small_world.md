# Small-world networks

Parent: [[graphs]]
See also: [[erdos_graph]], [[scale-free]]

#graph #generative #networks


The problem: IRL networks have rather high clustering coeff, as "a friend of my friend is usually my friend" (aka **Triadic closure**). Everything is very local. But unlike in a regular grid (that is local), the diameter is very small (grows logarithmically, and not as a power, as in a grid). We need to satisfy 2 requirements and be local and global at the same time. Intuition: let's go in-between a regular grid-like graph and a random graph.

## Watts-Strogatz Model

Start with a low-dim **regular lattice**, where nodes are arranged in a circle, and each node is connected to k nodes on the left and on the right. (This achieves a high clustering coefficient). But then have a **rewiring** phase, where we we pick random pairs of nodes, and rewire one of the ends of this edge to a different node. pE nodes total.

* No rewiring: h=N/2k, C = 1/2
* Full rewire: h = log N / log α, C = k/N
* Intermediate rewire (p~0.1 to 0.01 share of all edges): something in-between, where C is still high, but h is quite low.

> Obvious homework: model that, build a plot of C and h as a function of p.

Original citation: Watts, D. J., & Strogatz, S. H. (1998). Collective dynamics of ‘small-world’networks. nature, 393(6684), 440-442. (43k citations, huh!)