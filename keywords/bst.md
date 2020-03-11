# Binary Search Tree

#algo

See also:
* [[algos_trees]]
* [[algos_graph]]

In a BST, each node has a value, and each node has 2 children: left and right, with  `left.value < self.value`, and `right.value > self.value`. **Search** is trivial: just repeat checking if value matches, then go left or right depending on value. Same for **insertion**: you either meet same value (and as values are usually understood as keys, you just overwrite the key), or you find a gap between previously used keys. Both search and insertion are best to be implemented using **recursion**.

**Deletion** is a bit more tricky: #todo
https://www.geeksforgeeks.org/binary-search-tree-set-2-delete/

A BST is called **height-balanced** if the depth (aka height) of L and R branches in the tree, and every of its subtrees, is no larger than 1. A tree is called **weight-balanced** if the number of subnodes on the L and R of every node differs only by 1 (and it is a strictly stronger demand). **Perfectly balanced**: every node has either 2 or 0 children (for leafs), and all branches are of same size.

# Balanced implementations

* [[avl_tree]]
* red-black tree

# Refs

* https://www.geeksforgeeks.org/binary-search-tree-data-structure/