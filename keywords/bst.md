# Binary Search Tree

#algo

Hierarchy: [[algos]] / [[algos_trees]] /
See also: [[algos_graph]]

In a BST, each node has a value, and each node has 2 children: left and right, with left value < self value, and right value > self value. **Search** is trivial: just repeat checking if value matches, then go left or right depending on whether what you are looking for is smaller or larger than self value. Can be easily implemented using **recursion**:

```python
def find(root,key):
    if root is None: Search failed
    if key==root.key: Found it
    if key< root.key: find(root.left, key)
    else:             find(root.right, key)
```

**Insertion** is similar: you either find the key, and you can overwrite the value for this node, or you find a gap between previously used keys by reaching the end-level. So if using recursion and a base case of `root=None`, we have to remember the parent. Or we can check whether `left` and `right` are Nones before recurring on them. 

**Deletion** is a bit tricky: before deleting a node, we need to find a new place for its left and right branches. And for that, we need to find another node (aka "successor node") that would lie above everything on the left, but below everything on the right. The best candidate for that is the smallest of all numbers that are larger than old node. To find it, start from the "deletion node", step right once, then go left until you cannot go no more. That's the successor. Swap successor with the deletion node, then kill former successor.

While this approach works in principle, there are several special cases here:
* Maybe the deletion node has no children. - Then just delete it, and call it a day.
* Maybe the deletion node has only one subtree. - Then, just re-atatch this subtree to the stump where the doomed node used to be, and the tree is fixed.
* After stepping right (and we always can step right, as now we can assume that the deletion node has 2 subtrees), we may not be able to go left. But then the right child is already a good successor, and we put it at the place of former deletion node. Moreover, technically we don't have to consider it a special case, as we still "went left until we cannot go no more", it's just that we did 0 steps to the left haha.

Finally,  when we "kill the former successor node" (after either copying or swapping it with the doomed node), we need to re-attach the right subtree of this successor node, grafting it to the place where the successor node itself used to grow. So we set `successor.parent.left = successor.right`. Which means that once we started looking for a successor, we need to keep track of the parent of every "current node" we consider, as we'll need it once we find a successor (successor.parent is not really a thing in trees: links are not bidirectional).

Together, all of this is apparently called **Hibbard node deletion algorithm**.

Ref: http://www.mathcs.emory.edu/~cheung/Courses/171/Syllabus/9-BinTree/BST-delete2.html

# Balanced implementations

* A BST is called **height-balanced** if the difference in depth (aka height) of L and R branches in the tree, as well as every one of their subtrees, is no larger than 1. 
* A tree is called **weight-balanced** if the number of subnodes on the L and R of every node differs no more than by 1. Weight-balanced is a strictly stronger demand than height-balance. 
* **Perfectly balanced**: every node has either 2 or 0 children (leafs), and all branches are of the same size.

Implementations:
* [[avl_tree]]
* [[red-black_tree]]

# Refs

* https://www.geeksforgeeks.org/binary-search-tree-data-structure/