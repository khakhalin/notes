# Tree algorithms

Parents: [[algos]], [[algos_graph]]
See also: [[graphs]]


Data structures:
* [[priority_queue]] - aka Heap
* [[bst]] - Binary Search Tree
    * [[avl_tree]], [[red-black_tree]] - two balanced implementations of BST
* [[trie]] - space-compact and fail-efficient string representation

Other:
* [[rrt]] - Rapidly exploring Random Tree (fractal-like growth algo)

# Terminology

* **Binary tree** - two children for each node.
* **Complete tree** - without gaps in its right branches; can be written as a vector with child = 2k logic, as we used in [[priority_queue]]. In the last layer, all leaves are on the left, without gaps.
* **Full tree** - not only complete, but also the last layer is finished. I'm guessing it means that a full tree always has 2^k-1 elements, and is stored in a 2^k vector (with dummy head in 0). Also known as **proper** or **plane** binary tree.

# Traversals

As for graphs, can have **pre-order** and **post-order** traversals (kinda when the node is accessed during a traversal). But as trees are  layered and ordered, one can also define **in-order traversal**: imagine drawing a tree in a way for chidren nodes to fall in-between parent and grandparent (not under grandparent), then going left to right.

The algorithm for **in-order**:
1. Try to shift to an unprocessed node down-left. If successful, return to 1 (aka continue)
2. If not, and if current node is unprocessed, process it (and mark it as processed)
3. Try to shift to an unprocessed node down-right. If successful, return to  1 (continue)
4. If not, try to backtrack (go up). If successful, return to 1 (continue)
5. If not (nothing can be done), we're done with this tree.

So it sets the priorities in this order: Left>Current>Right>Up. And also Left and Right only happen if respective notes are unprocesses, while "Up" does not have htis condition (when we go "Up" from the left branch we shift to an unprocessed node, but when we back-grack from a right branch, upawrds nodes have been processed already).

**Recursively**, this is super-easy to implement: recurse on left, then process yourself, then recurse on right (left-self-right). Similarly, for pre-order it would be "self-left-right", and for post-order, "left-right-self".

**Level-order traversal**: first all 1st order elements (in practice, only one: root), then all 2nd order ones (all 2 of them), then all 3d etc. This one is easier to do with a queue, without recursion, as in BFS. And for **Reverse order traversal** (where you start with the lowest layer), we do almost the same thing, just 1) go right-left at each fork, and 2) don't process anything, but just populate the queue; then, once it is fully built, process it backwards, from the end to the beginning. As each layer was added there right-to-left, with backwards processing it would produce the correct order.

# Other types of trees

**Quad Tree**: For data compression and searching on 2D imaging and graphics applications. Each node is a square, and can have either 0 or 4 children (for sub-squares). A matching concept in 3D is called an **Octree**.