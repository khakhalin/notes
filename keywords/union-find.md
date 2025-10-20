# Union-find, aka Disjoint-Set

Parents: [[algos]] / [[algos_graph]]
See also: [[graphs]], [[min_span_tree]]

#graph #algo


To find each all connected components in a graph. 

Naive approach: for each node, remember its "boss" (a link to another node). This "boss" node will serve as a component id. At first, link all nodes to themselves (be their own boss). Then start looking at edges connecting two nodes:
* If both nodes have the same überboss (boss of the boss of the boss etc.), they are already in the same component.
* If they have different bosses, go upwards (boss of the boss of the boss etc.) until you reach the Überboss (that is a boss of itself). Then set überboss(A) to überboss(B), or the other way around, it doesn't matter. Now they are marked as belonging the same component.

Fancier approach:
* In step (2), always link shorter chain to an überboss of a longer chain, as it would make chain lenghts grow uniformly, and will keep everything fast.

It is also possible to keep track of a size of each subtree, and merge with this in mind.

**Complexity:** O(n log n): n because we consider n elements, and log n because that's how the hight of the tree grows.

# Refs

https://en.wikipedia.org/wiki/Disjoint-set_data_structure