# Erdős–Rényi model

#graphs

Parent: [[09_Graphs]] / [[graph_generate]]
Related [[small_world]]

Random graph, aka Poisson random graph. Introduced in 1960. Two variants:
* **G_np**: undirected graph of n node, with edges between all pairs of nodes equally probable, and probability p. More popular.
* **G_nm**: similar, but n nodes and m edges, and all edges (all pairs of nodes) equally probable.

**Properties** (for G_np):

**Degree distribution**: binomial. $P(k) = \begin{pmatrix} n-1 \\ k \end{pmatrix}p^k(1-p)^{n-1-k}$, where $\begin{pmatrix} n-1 \\ k \end{pmatrix}=\frac{n!}{k!(n-k)!}$. Bell-shaped. Not like IRL networks. Mean: k=p(n-1), variance σ² = p(1-p)(n-1). If we look at $σ/\bar k$, we'll get $1/\sqrt{n-1}$, which means that the distribution narrows down as the network becomes larger.

**Clustering coeff**: $C_i = \frac{2e}{k(k-1)} = \frac{2}{k(k-1)} \cdot \frac{pk(k-1)}{2}=p≈ \bar k / n$. As $\bar k$ is ~ const, C_i gets smaller as the network becomes bigger. (Which incidentally means that Erdos graphs aren't good for representing IRL large networks, as typically in real networks clustering coefficients are rather large; say the order of ~0.1)

## Graph Expansion

A graph G(V,E) has **expansion α** if ∀S⊆V (for every subset of nodes), N(edges leaving S) ≥ α∙min(|S|, |V-S|). Alternatively, we can define α as: $α=\min_S(\frac{N(\text{edges leaving }S)}{\min(|S|, |V-S|)})$. This cumbersome min(|S|, |V-S|) is avoidable if we only consider small subsets of nodes S (half of all nodes in G, or fewer). In other words, we are considering all possible (small) subsets of V, and looking at the number of edges conecting S and its complement. And looking at this number relative to the number of nodes involved.  If everything is very connected (no bottlenecks), α is large. If the graph has a narrow bridge connecting two large components, α is small.

> For example, in a full graph of N nodes, where N is even N = 2M, every node is connected to N-1 other nodes, we an extreme E/S of N-1 for it. If we consider larger S, the value goes down, until we split the graph in halves, having M nodes in each half, and M² edges connecting them, leading to α of M²/M = M. On the contrary, a graph consisting of two complete graphs, M nodes each, connected with a single one-edge-wide bridge, has α of 1/M.

Erdos graphs has large expansion values α. So in some way they are closer to complete graphs than to modular graphs (those with communities and bottlenecks)

Interesting property: in a graph of N nodes with expansion α, for every pair of nodes ij there's a path length O((log n)/α) connecting them. The intuition here relates to the name "expansion": α shows how many more nodes you are guaranteed ot have at each next step of BFS. From 1 node you go to at least α, to at least α², and so forth. It means that in ~log(n) steps we'll visit all nodes, and α regulates the base of this log.

> Homework: prove this equation (or at least justify it a bit better).

For an Erdos random graph, if average degree np is between log n and some constant c (log n > np > c), then graph diameter d(G) = O(log n / log np). So the diameter of a graph grows quite slowly (logarithmically), and increasing p decreases it further.

> What is this constant here? Why? Is it ∃c, or is it ∀c?

> Potential simulation homework: simulate how diameter grows in Erdos grpahs, with incrase in N, for a fixed p.

## Giant component

When average degree of an Erdos graph gets > p(n-1), suddely a **giant component** emerges: the majority of nodes get to be connected together. Already at k=1.5 ~60% of all nodes are connected; at k=2 80%; at k=3, 95%. Looks like a phase transition.

> An obvious topic for a modeling homework.