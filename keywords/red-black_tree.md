# Red-Black Tree

#algo

Hierarchy: [[algos]] / [[algos_trees]] / [[bst]]
Related: [[avl_tree]]

Like a BST tree, but balanced.

# 2-3 search tree

A conceptual prototype for a **red-black tree** is a **2-3 search tree**: in it every node can have either 2 or 3 children, and this flexibility is used for auto-balancing the tree during insertion and deletion. And then red-black trees are a way to implement a 2-3 tree without sacrificing the simplicity of binary trees. So let's look at 2-3 trees first.

There are two types of nodes in a TTST: **2-nodes** (same as in a binary tree), and **3-nodes** (that have 3 kids). A 2-node has one value and 2 kids (left and right), with left having keys < self.key, and right having keys > self.key. A 3-node has 2 keys (and so two values stored), and its kids are split in three branches: those below the left key (left branch), between the left and the right keys (middle branch), and above the right key (right branch).

The logic of inserting into the tree:
* If you reached a 2-node with 2 leafs, and it's time to insert, turn it into a 3-node
* If you reached a 3-node, and need to insert into it, there are 2 options:
    * If the 3-node is the root, replace it with 3 2-nodes
    * If it's not a lonely root:
        * compare the new key with 2 existing keys; 
        * of these 3 keys select the middle one, 
        * insert this key into the node upstream (recursively)
        * use the other 2 keys to create 2-nodes

It is clear from this definition that if we have a chain of 3 nodes, we can have a chain effect of them being turned into 2-nodes, with middle keys always squeezed upwards, until we either hit a 2-node (case 1 above), or a root (case 2b). Essentially it means that 2-3-trees grow from the bottom towards the top, just the top always remains connected to something (aka serves as a root).

# Red-Black Tree Proper

Now let's represent **3-nodes** from a 2-3-tree as pairs of 2-nodes, connected with a **red link**. To keep things simpler, let's agree that red links are always left links (aka **leaning left**). Technically it is edges that are red, but in practice it's easier to store "reddness" as a node property.

Fortunately for coding all this, we don't have to "translate" a 2-3-tree into a RB-tree, or implement it as an abstraction, but rather we can code the RB-tree directly, by adding just a few extra checks and adjustments to a standard BST, and then showing that the resultis equivalent to a 2-3-tree (as 2-3 tree is easier to undersand, and it's easier to build formal proofs about it). The adjustments in question are two **rotations** (left and right), and a color flip. (What's nice is that these rotations are far less involved than for an AVL tree)

* **Left rotation**: we start with a case when a node A has a normal (black) left kid, and a red (improper) right kid B. We make B the root of this system, and we make A its left, red child.
* **Right rotation**: the opposite: A is a left kid of B, but we make A the root, and B - its' right kid. The color of the edge is again preserved, making B a RED child (improper, but that's OK, it's temporary)
* In **Color flip** we make all three connections surrounding the node (L, R, and parent) flip their colors.

Here's how these operations are used in balancing:

* If right kid is red, rotate left.
* If left kid is left, but its kid is also red (two reds in a row, which is illegal), rotate right, then flip.

This operation should be performed after tree modification, recurrently (in post-order), so if it leads to a creation of a wrong configuration upstream, it will propagate upstream, until everything is fixed. The last touch to it all is that the very top, root node is always forced to be black, even if color-flipping tries to make it red.
