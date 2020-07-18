# Generating graphs and networks

#graph

Parent: [[09_Graphs]]
Siblings: [[graph_properties]] (prev)
See also: 

# Erdős–Rényi model

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

# Small-world networks

The problem: IRL networks have rather high clustering coeff, as "a friend of my friend is usually my friend" (aka **Triadic closure**). Everything is very local. But unlike in a regular grid (that is local), the diameter is very small (grows logarithmically, and not as a power, as in a grid). We need to satisfy 2 requirements and be local and global at the same time. Intuition: let's go in-between a regular grid-like graph and a random graph.

## Watts-Strogatz Model

Start with a low-dim **regular lattice**, where nodes are arranged in a circle, and each node is connected to k nodes on the left and on the right. (This achieves a high clustering coefficient). But then have a **rewiring** phase, where we we pick random pairs of nodes, and rewire one of the ends of this edge to a different node. pE nodes total.

* No rewiring: h=N/2k, C = 1/2
* Full rewire: h = log N / log α, C = k/N
* Intermediate rewire (p~0.1 to 0.01 share of all edges): something in-between, where C is still high, but h is quite low.

> Obvious homework: model that, build a plot of C and h as a function of p.

Original citation: Watts, D. J., & Strogatz, S. H. (1998). Collective dynamics of ‘small-world’networks. nature, 393(6684), 440-442. (43k citations, huh!)

## Kronecker Graph Model

Recursive (self-similar) generation of networks. Start with a small seed graph, and expand recursively. Take an adjacenty matrix K_1, then replace every 1 with K_1 as a block-matrix, and every 0 with a similarly sized block of 0. You get K_2. Repeat recursively. Very parallelizable, so good for creating very large graphs.

May be written using Kronecker product: C = A⨂B if we replace every a_ij with a block matrix a_ij∙B .

Kronecker graph: $\displaystyle K^{[m]} = \bigotimes^m K_1$ . Can also do various matrices at each step: $\displaystyle \bigotimes^m_{i=1} K_i$.

But isn't it too regular? Yes, so let's make it **stochastic**: instead of doing all 0/1, let's work with probabilities, calculate the final probabilities matrix $Θ_k$, and then at the very end replace each element p_ij with either 1 or 0 with probability p_ij, producing K_k. 

This is computationally expensive though (~$(n_1^m)^2$ weighted coin flips, where n_1 is the number of elements in K_1)! A way to fix: use the fact that IRL graphs are very sparse, and instead of flipping a coin for every possible edge, just decide how many edges we need, and find where  they would be. To do so, remember all K_i, and drop a fixed number of edges from the first graph K_1 down, at each step deciding to what quadrant the edge will go (like a funneling Galton board). This way we only need to generate random numbers $En_1 m$ times, where E is the total number of edges, n_1 is the size of the seed graph K_1, and m is the number of recursive expanions we performed.

> Obvious homework: generate this graph for some matrix of your choice; measure its properties.

By playing with generating matrices, it is possible to bring Kronecker graphs very close to large IRL networks. In practice, for a vanilla stochastic Kronecker graph the **degree distribution is very spiky**, so in practice people use special tricks to smooth it out.

> Homework: figure out how to smooth it out, and do it.

Original publication: Leskovec, J., Chakrabarti, D., Kleinberg, J., Faloutsos, C., & Ghahramani, Z. (2010). Kronecker graphs: an approach to modeling networks. Journal of Machine Learning Research, 11(2).
https://arxiv.org/abs/0812.4905
(~800 citations; first variant in 2008, so spent 2 years in review, huh)