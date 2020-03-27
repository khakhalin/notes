# AVL trees
#algo

Hierarchy: [[algos]] / [[algos_trees]] /
See also: [[bst]], [[red-black_tree]]

**AVL trees**, aka **self-balancing BST** is always kept to be height-balanced, guaranteeing O(H) = O(log n) operations for search, insert, delete, max and min (as opposed to O(n) for the worst unbalanced tree).

The key to maintaining this balancing is to use two extra operations: left and right **rotations** that tranform BST between the following two states:
`y(x(1,2) , 3) ↔ x(1 , y(2,3))`, where x and y are nodes, and 1, 2, and 3 are whole sub-trees starting at a node. These rotations are possible because 1<x<2<y<3 can be resolved (structured as a binary search tree) in two different ways, with either x or y as a root.

# Insertion

With rotations, **insertion** in an AVL tree is followed by **balancing**. Insert a node as usual, then travel up until you find the **first unbalanced node**. Do it by comparing heights of the children. Each node remembers its own hight, and it is also recursively updated as you climb up during insertion. Then perform an appropriate rotation, to shorten the path that starts from this newly added node. In practice, you have to hard-code 4 cases, when the path from the unbalanced node towards the newly added node goes either LL, LR, RL, and RR. You can draw all 4 and figure which of the nodes should be rotated (it's either the unbalanced node itself, or its kid, and either left or right rotation). [ref with Python code](https://www.geeksforgeeks.org/avl-tree-set-1-insertion/). The operation has some overhead, but is strictly O(log n).

# Deletion

The basic idea is that we first delete the node as in any other BST tree. Then we travel up until we find the first unbalanced node Z. And then we balance it. There are 4 cases, called left-left, left-right, right-right, and right-left, depending on whether it is the left or the right subtree of Z that is longer, and depending on whether it is the left of right subtree of the longest-child-of-Z that is longer.

* Left-Left case: Z has Y as its left kid, and Y has X as its left kid. We have 1<X<2<Y<3<Z<4. So we make it with Y on top, and X and Y as its kids.
* Left-Right case: Z has Y as its left kid, and Y has X as its right kid. We have 1<Y<2<X<3<Z<4. We make X the new top.
* Right-Right and Right-Left are identical, just mirror-flipped.

https://www.geeksforgeeks.org/avl-tree-set-2-deletion/

# Comparison to other trees

Compared to a **red-black tree** [[red-black_tree]], AVL trees are more balanced, but may require more rotations, so they are good if you are mostly reading, but don't insert and delete that much.

# Refs

* https://www.geeksforgeeks.org/avl-tree-set-1-insertion/