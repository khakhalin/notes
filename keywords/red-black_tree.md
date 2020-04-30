# Red-Black Tree

#algo

Hierarchy: [[algos]] / [[algos_trees]] /
See also: [[bst]], [[avl_tree]]

Like a BST tree, but balanced.

# 2-3 search tree

A conceptual prototype for a **red-black tree** is a **2-3 search tree**; in it every node can have either 2 or 3 children, and this flexibility is used for auto-balancing the tree during insertion and deletion. And then red-black trees are a way to implement a 2-3 tree without sacrificing the simplicity of binary trees. So let's look at 2-3 trees first.

There are two types of nodes in a TTST: 2-nodes (same in a binary tree), and 3-nodes (that have 3 kids). A 2-node has one value and 2 kids (left and right), with left having keys < self.key, and right having keys > self.key. A 3-node has 2 keys (and so two values), and its kids respectively have kids below the left key (left branch), between the left and the right (middle branch), and above the right key (right branch).
