# Graph symmetry

#graph

Parent: [[09_Graphs]] / [[graph_properties]]
Related: [[groups]]


Relevant papers:
* [[Carroll2019structure]] - that good reservoirs for [[echo]] should be asymmetric


A graph is called **symmetric** if it has an **automorphism** (a self-**isomorphism**, or a permutation of vertices that preserves the graph).

Finding symmetries of a graph is generally an **NP problem**, as it's literally a graph isomorphism problem. But some **heuristics** exist. For example, if nodes have different degrees, they cannot be twins. Following this logic consistently, one can greatly reduce the number of of nodes to tested for interchangeability.

Most large random ([[erdos_graph]]) graphs are asymmetric (probability of symmetry goes to 0 as N increases). At the same time, infinite graphs are typically symmetric.

Another interesting paper (Ball 2018) shows that most real-life graphs (from some graph repository) have at least one symmetry (70%, out of about 300 considered). But for most graphs, the redundancy is low, so their  symmetries shouldn't be that consequential. Most real-world graphs are pretty similar to an asymmetric graph, which if of course not that surprising.

# Packages

The citation in [[Carroll2019structure]] (Sage 2013) is not a real publication, and leads towards the SageMath software piece, which is a standalone python-like language. In Sage, I couldn't quite figure out what function would calculate graph symmetries for directed graphs. So maybe it's possible, but I'm not sure how, and it's not Python anyways.
* https://www.sagemath.org/
* https://doc.sagemath.org/html/en/reference/graphs/sage/graphs/graph.html
* https://doc.sagemath.org/html/en/reference/graphs/sage/graphs/digraph.html

The tool used in (Ball 2018) is called `saucy`, and it just returns the symmetries. Seems promising:
* https://github.com/KIT-IISM-EM/pysaucy2
* https://kit-iism-em.github.io/pysaucy2/html/

# Curiosities

**Frucht graph**: one of the five smallest cubic graphs (all nodes have degree=3) that has no symmetries (automorphisms) except identity. Pancyclic (contains cycles of all possible lengths), planar, Hamiltonian (a cycle to visit every node). [wiki](https://en.wikipedia.org/wiki/Frucht_graph) _May be interested for complexity estimations?_

# Refs

Definition:
https://en.wikipedia.org/wiki/Symmetric_graph

Minimal asymmetric graphs:
https://en.wikipedia.org/wiki/Asymmetric_graph

De Fraysseix, H. (1999, September). An heuristic for graph symmetry detection. In International Symposium on Graph Drawing (pp. 276-285). Springer, Berlin, Heidelberg.

Ball, F., & Geyer-Schulz, A. (2018). How symmetric are real-world graphs? A large-scale study. Symmetry, 10(1), 29.

The reference for `saucy`:
Darga, P.T.; Sakallah, K.A.; Markov, I.L. Faster Symmetry Discovery Using Sparsity of Symmetries.
In Proceedings of the 2008 45th ACM/IEEE Design Automation Conference, Anaheim, CA, USA,
8–13 June 2008; pp. 149–154.

Southwell, R. (2006). Finding symmetries in graphs (Doctoral dissertation, Ph. D. thesis. University of York, UK).
https://richardsouthwell.files.wordpress.com/2010/08/msc.pdf