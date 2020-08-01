# Pagerank and link analysis

#graph #centrality

Parents: [[09_Graphs]] / centrality
Related: Katz centrality, Kosaraju-Sharir, PCA, eigenvectors, spectral techniques on graphs

## Bowtie with tendrils

IRL motivation: Web as a graph; pages = nodes, links = edges. (Footnotes for the modern age: what about dynamically generated pages? What about firewalls and walls, aka "dark matter"? We ignore them both in this discussion) We also ignore transactional links (that make something happen), and consider only oldschool navigational links. Other similar networks: citation papers, articles on Wikipedia.

These are obviously graphs, and they are **directed**. For every node $v$, let's define two sets: a set of nodes that can be reached from this node out(v), and a set of nodes from which you can arrive at this node in(v).
* In **Strongly connected graphs** in(v) = out(v) = entire graph. 
* Sort of an opposite, from this pov, in a **Directed Acyclic Graph** (DAG): in(v) ∩ out(v) = v. If w ∈ in(v) ⇒ w ∉ out(v), for w≠v. Trees for example are DAGs.

Any directed graph can be expressed as a sort of a superposition of these two types of graphs via **graph condensation**: all strongly connected components may be merged into a **super-node**, or meta-node, and then these meta-nodes, together with the nodes of the original graph that didn't belong to super-connected components, will form a DAG. (Proof by design / contradiction: if a→b and b→a, then they belong to a strongly connected component, and so may be condensed; once it is no longer possible, we have to deal with a DAG)

To keep it easier, when people talk about **strongly connected components**, they refer to largest possible components connecting every pair of mutually connected nodes. So a SCC is always a single one, that becomes a single meta-node during condensation, and this node is final (cannot be merged with any other node). This way, the statement above may be rephrased as: **every graph is a DAG on its SSCs**.

12:50

(Broder 2000): analysis of internet structure (as it was in 1999) using this DAG/SCC paradigm. 2e8 nodes (back then); how to analyze it? SSC (that ∋ v) = out(v) ∩ in(v) = out(v, G) ∩ out(v, G'), where G' is a flipped graph (with the direction of all edges flipped). 

> See Kosaraju-Sharir algorithm, to run it efficiently.

What Broder et. al discovered (using a state-of-the art machine that was less powerful that a laptop today) is that if you start from random nodes, and do a BFS, you either end very early (~100 nodes), or you get almost the entire web. The graph is only plottable in log-y scale, and it grows faster than exponentially, and then suddently switches to a maxed-out plato. A **Giant Connected Component**. (In their case, saturated out(v) contained ~50% of the nodes, and so did saturated in(v), with their intersection - the giant strongly connected component proper - containing ~30% of all nodes). They called this structure a "**Bowtie with tendrils**" - there's a dense knot in the middle (the giant components), a fan-in (of pages with links that aren't referenced), a fan-out (of pages that are referenced, but don't contain links), and then some weird random tentacles and tendrils growing off it, as well as some disconnected components.

22:40

## Pagerank

Are some pages more "important" than the others? Yep. But if the "importance" of a page affects its position in the graph, we can use this graph info to reconstruct the implicit importance. The famous idea behind the original Google search algorithm: let's interpret links as "votes" for the quality (importance) of the page. This idea is also inherently **recursive**, if you assume that "votes" of important nodes are more important, which seems a logical assumption. But now you may potentially have some circular reasoning that will have to be resolved.

Their formulation: the importance (rank) of a page is evenly split betwee, and inherited by those pages it links to. Importance of page j $r_j = \sum_i f_i$, where $f_i$ is a vote coming from page $i$, and this vote $f_i = r_i / d_i$, where $d_i$ is the out-degree of page (node) $i$. Combining his into one formula:

$\displaystyle r_j = \sum_{i→j} \frac{r_i}{d_i}$

31:00

Can we solve it? Yes, probably, as it's essentially a matrix equation $\textbf{R} = \textbf{AD}^{-1}\textbf{R}$ (with a modified adjacency matrix that has 1s scaled down by $d_i$-s for existing edges). Except it's better not to it in a brute-force straightforward way (Gaussian elimination), as it won't scale to really large matrices. And btw, we'll also require ΣR =1 (normalized, in a statistical sense).

> I mean, as usual, if R is a column-vector, and A is used as an operator to be multiplied by R on the right, as above, then A is flipped compared to a "normal" graph matrix, in the sense of a_ji encoding the i→j edge. If you want a "classic", adjacency matrix, you'll have to flip it: R = MR, where M = (D⁻¹A)ᵀ.

**Random walk interpretation**: if there are random walks on this graph, $r_i$ will encode the probability of the process being at node $i$ over time (or, if we have lots of concurrent walks, it will reflect the expected number of random walkers staying at node $i$ at every moment of time), once the stationary distribution of these probabilities is reached.

40:40

It also means that pagerank = **principal eigenvector** of M = (D⁻¹A)ᵀ. And principal eigenvectors can be easily found via **power iteration** (exactly same as in PCA):

* Init: $r^{(0)} = \vec{1}/N$ 
* Iterate: $r^{(t+1)} = Mr^{(t)}$
* Stop when $Δr < ε$ (typically in L1 norm)

But does the solution even exist? **Will R=MR converge** to something reasonable? In this naive form, **no**, because there are nodes with no way out, so r will get trapped there, and completely leak out of the connected component. The "dead end" problem.

> For the matrix, the behavior is not defined as you cannot delete 0 by d_out. If you keep all these rows as 0, they'll just disappear completely; if you put a 1 on the diagonal, to reflect that random walkers stay there forever, they will stay there forever, with different (but uninteresting) r_i values.

And the same situation will happen in cycles (aka "Spider trap" problem) that will serve as similar "final endpoints" (just a cycle instead of a loop).

How to solve it? Make everything leak just a little bit by adding "random teleportation". With probability 1-β (typically ~0.1, aka $β=0.9$), a random walker disappears and gets "teleported" to a random node in the web. Or if you think in terms of flows, $(1-β)r_i$ of flow is consumed from every point, and a constant flow of $(1-β)/N$ is also injected injected in every point (where N is the number of nodes).

In a matrix form, it is equivalent to scaling all elements by β, but also adding a scalar:

$\displaystyle \textbf{R} = β\textbf{MR} + \frac{1-β}{N}$, where M either doesn't include dead-end nodes, or has them as 1/N flat rows (aka teleportation certainly happens, as it is the only choice now, and it is uniformly random).

The entire construction above may be represented as a magic "Google matrix" $\mathcal{Μ} = β\textbf{M} + \frac{1-β}{N}\textbf{E}$, where $\textbf{E}$ is a matrix of all ones (not an identity matrix, but 1s everywhere). It works like a scalar because we normalize $\textbf{R}$ to $\sum r_i=1$, and so $\textbf{ER}=\vec{1}$. It turns the iterative equation above to somple $\textbf{R} = \mathcal{M}\textbf{R}$. Power iteration still works, and typically converges in 10-100 iterations. 

> BTW, apparently map-reduce architecture was originally invented to run pageranks.

1:01:30

**How to calculate Pagerank in practice?** The adjacency matrix A is sparse, so it can exist in memory, but the "Google matrix" $\mathcal{M}$ is no longer sparse, and has N² dimensions that are impractical. So go back to a sparse matrix M with no dead-ends (zeroes for dead-ends), and add a scalar manually. And to deal with leaking dead-ends, just renormalize R at every step, which is equivalent to insta-teleportation.

Just in case, the algorithm once again:
1. Set $r=\vec{1}/N$
2. $\displaystyle \forall j: r' = 0+β\sum_{i→j}\frac{r_i}{d_i}$. That is, if no edges lead to j and $\{i: i→j\}=\varnothing$, this $r_j = 0$.
3. $\displaystyle \forall j: r' = r' + \frac{1-S}{N}$ where $S=\sum_j r'_j$. 
4. If $\sum_j |r_j' - r_j|<ε$, break the loop.
5. Else: $r = r'$, and loop back to point 2 (next iteration).

1:12:24

## Personalized PageRank and Random walk with restarts

**Recommender systems** often work with data that looks like a bipartite graph (something like user - product). So we want to implement **collaborative filtering** [[collab_filtering]] to find nodes (users , products) that are related. Number of common neighbors is important (should count towards similarity), but number of non-common neighbors should count against similarity. So it gives us a set of intuitions:
* The more common friends 2 nodes have, the closer they are
* The more different friends 2 nodes have, the further they are
* If there is no direct connections, but you can reach each other with several hops, they are still somewhat related, but the fewer hops, the better.

And random walks on graphs satisfy this intuition!

* **Personalized Page-Rank**: like normal page-rank, but for walks that start in only a subset of nodes (aka **query nodes**; thematically filtered). 
* **Random walk with restarts**: In the most extreme case we always restart in the same node. In this case it may be easier to explicitly simulate random walks, rather than work with probabilities (via power iteration). Helps to quickly create thematic recommending systems.

> What's the complexity of random walk with restart? For a single node, it's roughly constant, as we can decide apriori the depth of each random walk, and the number of walks. During the walk, we will have to decide where to go from each node, which is equivalent to randomly sampling from a list stored in a hashtable (a list of edges), and even this may be further optimized by duplicating some edges if necessary, giving all random samples the same length (as they did in [[word2vec]] for negative sampling - just a performance hack).

# Refs

Conspectus of lectures by Leskovec:
https://snap-stanford.github.io/cs224w-notes/network-methods/pagerank

Youtube lecture:
https://www.youtube.com/watch?v=_4rPZVOLzCs&list=PL1OaWjIc3zJ4xhom40qFY5jkZfyO5EDOZ&index=10

## Original papers

Broder, A., Kumar, R., Maghoul, F., Raghavan, P., Rajagopalan, S., Stata, R., ... & Wiener, J. (2000). Graph structure in the web. Computer networks, 33(1-6), 309-320.
4k citations. Published by Altavista people, based on 1999 internet structure.

Google Engine:
Brin, S., & Page, L. (1998). The anatomy of a large-scale hypertextual web search engine.
20k citations.

Pagerank:
Page, L., Brin, S., Motwani, R., & Winograd, T. (1999). The PageRank citation ranking: Bringing order to the web. Stanford InfoLab.
http://ilpubs.stanford.edu:8090/422/1/1999-66.pdf