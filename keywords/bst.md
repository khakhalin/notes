# Binary Search Tree

#algo

Hierarchy: [[algos]] / [[algos_trees]] /
See also: [[algos_graph]]

In a BST, each node has a value, and each node has 2 children: left and right, with left value < self value, and right value > self value. **Search** is trivial: just repeat checking if value matches, then go left or right depending on whether what you are looking for is smaller or larger than self value. Same for **insertion**: you either find this value (and as values are usually understood as keys, you just overwrite this key), or you find a gap between previously used keys by reaching the end-level, and attaching a new leaf. Both search and insertion can be easily implemented using **recursion**:

```python
def find(root,key):
    if root is None: Search failed
    if key==root.val: Found it
    if key< root.val: find(root.left, key)
    else:             find(root.right, key)
```

**Deletion** is a bit more tricky: before deleting a node, you need to find a new place for its left and right branches. And for that, we need too find another node (aka "successor node") that would lie above everything on the left, but below everything on the right. The best candidate for that would be the smallest of all numbers that are larger than old node. To find it, start from the "deletion node", step right once, then go left until you cannot go no more. That's the successor. Swap successor with the deletion node, then kill former successor.

While this approach would work in most cases, there are several special cases here:
* Maybe the deletion node has no children. - But then just delete it, and call it a day.
* Maybe the deletion node has only one subtree. - But then, just reatatch this subtree to the stump, and the tree is fixed.
* After stepping right (and we always can step right, as now we can assume that the deletion node has 2 subtrees), we may not be able to go left. But then the right child is already a good successor, and we put it at the place of former deletion node. And technically we don't have to consider it a special case, as we still "went left until we cannot go no more", it's just that we did 0 steps haha.

Finally,  when we "kill successor node", we need to re-attach its right subtree to a place where the successor node itself used to grow. So we set successors_parent.left = successor.right. Which also means that once we started looking for a successor, we need to keep track of the parent of current node, as we'll need it once we find a successor.

Together, all this is apparently called **Hibbard node deletion algorithm**.

Ref: http://www.mathcs.emory.edu/~cheung/Courses/171/Syllabus/9-BinTree/BST-delete2.html

A BST is called **height-balanced** if the depth (aka height) of L and R branches in the tree, and every of its subtrees, is no larger than 1. A tree is called **weight-balanced** if the number of subnodes on the L and R of every node differs only by 1 (and it is a strictly stronger demand). **Perfectly balanced**: every node has either 2 or 0 children (for leafs), and all branches are of same size.

# Balanced implementations

* [[avl_tree]]
* [[red-black_tree]]

# Refs

* https://www.geeksforgeeks.org/binary-search-tree-data-structure/