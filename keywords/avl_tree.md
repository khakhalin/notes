# AVL trees
#algo

See also: 
* [[bst]] - (Binary Search Tree)
* [[algos_trees]] - general tree algorithms

**AVL trees**, aka **self-balancing BST** is always kept to be height-balanced, guaranteeing O(H) = O(log n) operations for search, insert, delete, max and min (as opposed to O(n) for the worst unbalanced tree).

The key to maintaining this balancing is to use two extra operations: left and right **rotations** that tranform BST between the following two states:
`y(x(1,2) , 3) ↔ x(1 , y(2,3))`, where x and y are nodes, and 1, 2, and 3 are whole sub-trees starting at a node. These rotations are possible because 1<x<2<y<3 can be resolved (structured as a binary search tree) in two different ways, with either x or y as a root.

# Insertion

With rotations, **insertion** in an AVL tree is followed by **balancing**. Insert a node as usual, then travel up until you find the **first unbalanced node**. Do it by comparing heights of the children. Each node remembers its own hight, and it is also recursively updated as you climb up during insertion. Then perform an appropriate rotation, to shorten the path that starts from this newly added node. In practice, you have to hard-code 4 cases, when the path from the unbalanced node towards the newly added node goes either LL, LR, RL, and RR. You can draw all 4 and figure which of the nodes should be rotated (it's either the unbalanced node itself, or its kid, and either left or right rotation). [ref with Python code](https://www.geeksforgeeks.org/avl-tree-set-1-insertion/). The operation has some overhead, but is strictly O(log n).

# Deletion

#todo

https://www.geeksforgeeks.org/avl-tree-set-2-deletion/

# Comparison to other trees

Compared to **red-black trees**, AVL trees are more balanced, but may require more rotations, so they are good if you are mostly reading, but don't insert and delete that much.

# Refs

* https://www.geeksforgeeks.org/avl-tree-set-1-insertion/