# Bridge

Parents: [[09_Graphs]] / [[algos_graph]]
See also: [[union-find]]

#graph #algo


A **bridge** is an edge in a graph that, if you remove it, will disjoint the nodes that it connects (they will now belong to two different comonents). There seem to be no fast method to tell if an edge is a bridge or not, as ultimately it boils down to figuring out if this edge is a part of a cycle, and that's slow. You have to do full DFS (see [[algos_graph]]) to see if the other node is reachable by any other way than this one, which is an O(E + N) algorithm.

If we need to eliminate edges **iteratively**, one by one (as in building in [[min_span_tree]], but by removal rather than agglomeration) we can cheat a bit by keeping decent spanning tree, and just checking if the dge belongs to a tree. If no, we can drop it. If yes, we need to see if we can adjust the tree by "grafting" the "upstream" node to some other edge of a tree (check all its connections, and see if any of them is not downstream from the node). If yes, ajust the tree and continue. If impossible, it means that cutting the edge would break the tree, and thus the edge is a bridge. To check if a node is downstream, we can use linked-list labeling, as in [[union-find]].

The iterative problem above is called a "Reverse-delete algorith"; there's a wikipedia page, but it doesn't describe how ot check for "if still connected" quickly, in this particular case: https://en.wikipedia.org/wiki/Reverse-delete_algorithm

# Refs

https://stackoverflow.com/questions/2994681/how-to-detect-if-breaking-an-edge-will-make-a-graph-disjoint

Some more rather tricky bridge identification algorithms are described here: 
https://en.wikipedia.org/wiki/Bridge_%28graph_theory%29