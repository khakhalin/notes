# AVL trees

#algo #trees

Hierarchy: [[algos]] / [[algos_trees]] /
See also: [[bst]], [[red-black_tree]]

**AVL tree**, aka **self-balancing BST** is always kept to be height-balanced, guaranteeing O(H) = O(log n) operations for search, insert, delete, max and min (as opposed to O(n) for the worst unbalanced tree). The  tree is named AVL for the first letters in names of 2 inventors (both Russian).

The key to maintaining this balancing is to use two extra operations: left and right **rotations** that tranform BST between the following two states:
`y(x(1,2) , 3) ↔ x(1 , y(2,3))`, where x and y are nodes, and 1, 2, and 3 are whole sub-trees starting at a node. These rotations are possible because 1<x<2<y<3 can be resolved (structured as a binary search tree) in two different ways, with either x or y as a root.

# Insertion

With rotations, **insertion** in an AVL tree is followed by **balancing**. Insert a node as usual, then travel up until you find the **first unbalanced node**. Do it by comparing heights of the children. Each node remembers its own height, and it is also recursively updated as you climb up during insertion. 

Then perform the appropriate rotations (either 1 or 2), to shorten the path that starts from this newly added node. We have to hard-code 4 cases, depending on whether the path from the unbalanced node towards the newly added node goes LL, LR, RL, and RR. If necessary, you can draw all 4 cases (see below), and figure which of the nodes should be rotated (it's either the unbalanced node itsef, or its kid, and either left or right rotation). The operation has some overhead, but is strictly O(log n).

In practice, if we coded insertion method recurrently, we don't have to code this "travel up" explicitly, but just do something to the effect:

```
def self.add(val):
    self.branch = either self.left or self.right (depending on val)
    self.branch = self.branch.add(val)
    return balance(self)
```

# Deletion

The basic idea is that we first delete the node, as in any other BST tree. Then we travel up until we find the first unbalanced node Z, and balance it. There are again 4 cases, called left-left, left-right, right-right, and right-left, depending on whether it is the left or the right subtree of Z that is longer, and depending on whether it is the left of right subtree of the longest-child-of-Z that is longer.

* Left-Left case: Z has Y as its left kid, and Y has X as its left kid. We have 1<X<2<Y<3<Z<4, and currently Z is on top. So we make Y a new top (the former left child of Z), with X and Z as its kids.
* Left-Right case: Z has X as its left kid, and X has Y as its right kid. We have 1<X<2<Y<3<Z<4, with Z on top. We make Y (former LR grand-kid) the new top, and X and Z its kids.
* Right-Right and Right-Left are identical, but mirror-flipped.

Another way to write the same thing:
* LL: `Z(Y[X(1,2),3],4)` → `Y(X[1,2],Z[3,4])`
* LR: `Z(X[1,Y(2,3)],4)` → `Y(X[1,2],Z[3,4])`

In theory, in Python, with its tuple assignments, both rotations can be coded one-liners (but horrible one-liners, so it's a bad idea):

```
temp = z.left
z.left, z.left.right = z.left.right, z
return temp

temp = z.left.right
z.left, z.left.right, z.left.right.left, z.left.right.right
=
z.left.right.right, z.left.right.left, z.left.left, z
return temp
```

# Comparison to other trees

Compared to a [[red-black_tree]], AVL trees are more balanced, but may require more rotations, so they are good if you are mostly reading, but don't insert and delete that much.

# Refs

* https://www.geeksforgeeks.org/avl-tree-set-1-insertion/
* https://www.geeksforgeeks.org/avl-tree-set-2-deletion/