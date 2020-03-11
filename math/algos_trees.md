# General tree algorithms
#algo

See also: 
* [[algos_graph]] - general chapter for algorithms
* [[bst]] - Binary Search Tree

# Terminology
* **Binary tree** - two children for each node.
* **Complete tree** - without gaps in its right branches; can be written as a vector with child = 2k logic, as we used in [[priority_queue]]. In the last layer, all leaves are on the left, without gaps.
* **Full tree** - not only complete, but also the last layer is finished. I'm guessing it means that a full tree always has 2^k-1 elements, and is stored in a 2^k vector (with dummy head in 0). Also known as **proper** or **plane** binary tree.

As for graphs, can have **pre-order** and **post-order** traversals (kinda when the node is accessed during a traversal). But as a tree is layered and ordered, one can also define **in-order traversal**: imagine drawing a tree so that chidren would also fell in-between parent and grandparent (not under grandparent), then going left to right. The algorithm for **in-order**:
1. Try to go to an unprocessed node down-left. If successful, continue the loop (return to 1)
2. If not, and if current node is unprocessed, process it (and mark it as processed)
3. Try to go to an unprocessed node down-right. If successful, continue (return to  1)
4. If not, try to backtrack (go up). If successful, continue (return to  1)
5. If not (nothing can be done), we're done with this tree.

**Recursively**, this is super-easy to implement: recurse on left, then process yourself, then recurse on right (left-self-right). Similarly, for pre-order it would be "self-left-right", and for post-order, "left-right-self".

**Level-order traversal**: first all 1st order elements (in practice, only one: root), then all 2nd order ones (all 2 of them), then all 3d etc. It is easier to do it with a queue, without recursion, as in BFS. And for **Reverse order traversal** (where you start with the lowest layer), we do almost the same thing, just 1) go right-left at each fork, and 2) don't process anything, but just populate the queue; then, once it is fully built, process it backwards, from the end to the beginning. As each layer was added there right-to-left, with backwards processing it would produce the correct order.