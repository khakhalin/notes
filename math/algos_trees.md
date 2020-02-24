# Algorithms about trees
#algo

See also: 
* [[algos_graph]]

# Terminology
* **Binary tree** - two children for each node.
* **Complete tree** - without gaps in its right branches; can be written as a vector with child = 2k logic, as we used in [[priority_queue]]. In the last layer, all leaves are on the left, without gaps.
* **Full tree** - not only complete, but also the last layer is finished. I'm guessing it means that a full tree always has 2^k-1 elements, and is stored in a 2^k vector (with dummy head in 0). Also known as **proper** or **plane** binary tree.

As for graphs, can have **pre-order** and **post-order** traversals (kinda when the node is accessed during a traversal). But as a tree is layered and ordered, one can also define **in-order traversal**: imagine drawing a tree so that chidren would also fell in-between parent and grandparent (not under grandparent), then going left to right. The algorithm for **in-order**:
1. Go down-left until there are unvisited nodes down-left
2. Once stuck, if the node is unprocessed, process it, mark it as visited
3. Try to go down-right. If successful, continue the loop (return to statement 1)
4. Try to backtrack (go up). If successful, continue the loop (return to statement 1)
5. If nothing can be done, we're done with this tree.

Recursively, this is super-easy to implement: recurse on left, then process yourself, then recurse on right. While for pre-order it is self-left-right, and for post-order it is left-right-self.

**Level-order traversal**: first 1st order element (root), then all 2nd order ones (all 2 of them), then all 3d etc. It is probably easier to do it with a queue, without recursion. And for **Reverse order traversal** (where you start with the lowest layer), we do almost the same thing, just 1) go right-left at each fork, and 2) don't destroy the queue at the end; instead, once it is fully built, we read it (process the nodes) from the end to the beginning, which gives the correct order.