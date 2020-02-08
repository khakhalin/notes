# Priority Queue (and Binary Heap)
#algo

Classic data structure, helps to move max (or min) operation from O(N) to O(log N).

The idea is to arrange data points in a tree-like structure, with left and right branches equivalent, and the only logic of parent node always being "better" than the children. It means that finding the "best" value is always trivial (it's always in the root), and all work is moved to keeing the structure properly arranged.

Two interfaces: **add value** and **remove best** (like popping from a stack).

Two possible internal processes: float up (usually called **swim**), and **sink** down.

**Swim**: if child is better than the parent, swap child with the parent. Repeat until the parent is better, of former child becomes a root.

**Sink**: if a parent is worse than both kids, pick the better of kids, and swap with them. Repeat until either content, or become a leaf (lowest-level with no kids).

In practice, instead of keeping this data as a linked tree, it is usually kept as a **Binary Heap**: a list with a dummy unused value in position 0. That way, for any element at position k its kids can be found at positions 2k and 2k+1.

For this to work, the left branch should always be full before the right branch is increased, so that there is no gap in presentation of the last layer. Or you'll have to pad empty leaves with NaNs, instead of just making sure that the address of a potential child is ≤ n, where n is the current size of a heap. For a priority queue it is achievable, as we can make sure that we always modify the last element in the list, and the integrity of the leaf layer is never violated:

* To pop, place the last element as a new root, then sink.
* To add, always add as the last element, then swim up.

As both swimming and sinking don't change the shape of the tree, but only the sequence of elements, it solves the issue!

It is also possible to have **multi-way heaps** (not binary, but, say, tertiary).

It is also possible to use a priority queue as a sorting algo, called **Heap Sort**: just add all elements to a priority queue one by one, then pop them all. ~O(n log n), or more specifically O(2n log n + 2n).