# Trie

Parents: [[algos_trees]]

#algo #text #graph


Key idea: A search tree for a string that tells "not found" early on, not waiting for the rest of the string. And also that stores strings with matching beginnings efficiently, without double-storage. Pronounced as "try".

Consider a fixed alphabet R. Each node uses one character with a key, stores some value, and has links to R (at least potentially) children nodes, reflecting all possible continuations of a string.

**Seach** and **add** operations are trivial: loop through chars, every time go down. If cannot, or if once the string is empty, the node has no value (aka "terminates at a null link"), it's a **search miss**. Many search misses would happen before we go down the full trie, which is good. Because a trie represents an ordered set of keys, its creation is invariant to the sequence at which new keys are introduced (but it is not necessarily true for optimized versions of a trie).

Another really cool thing about tries is that they support **wildcard match** relatively easily (you'd only need to loop through options at a branch).

**Size** can be done recursively lazily (on request), but it's slow, so may be better to update it on addition. **Collecting keys** is similar, also recursive. **Deleting** can also benefit from recursion, in the style of  `this.next = this.next.delete()`. The logic here is funny: if you are getting deleted, and you have kids, just become a transit node. If you don't have kids, it's branch-trimming, so not only delete yourself, but return None. Then upon recursion (for nomal-not-deleting node), check if you are still used: a node without children and value is a member of a dead branch and should also delete itself, sending the deletion signal further upstream.

# Example

A phrase "shall she sell sea shells at a shallow sea shore" is encoded in the following basic trie (20 nodes overall):

```
shall
││││└ow
│└e
││└lls
│└ore
└ell
│└a
a
└t
```