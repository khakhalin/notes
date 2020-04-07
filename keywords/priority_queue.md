# Priority Queue (and Binary Heap)
#algo

See also:
* [[algos_trees]] - tree algorithms

Classic data structure, helps to move max (or min) operation from O(N) to O(log N).

The idea is to arrange data points in a tree-like structure, with left and right branches equivalent, and the only logic of parent node always being "better" than the children. It means that finding the "best" value is always trivial (it's always in the root), and all work is moved to keeing the structure properly arranged.

Two interfaces: **add value** and **remove best** (like popping from a stack).

Two possible internal processes: float up (usually called **swim**), and **sink** down.

* **Swim**: if an element is better than its parent, swap it with its parent. Repeat until the current parent is better, or the element becomes a root.
* **Sink**: if both kids are better than the element, pick the better of the kids, and swap them. Repeat until either both kids are worse, or you become a leaf (no kids).

In practice, instead of keeping this data as a linked tree, it is usually kept as a **Binary Heap**: a list with a dummy unused value in position 0. That way, for any element at position k its kids can be found at positions 2k and 2k+1.

For this to work, the left branch should always be full before the right branch is increased, so that there is no gap in presentation of the last layer (aka **complete tree**). Or you'll have to pad empty leaves with NaNs, instead of just making sure that the address of a potential child is ≤ n, where n is the current size of a heap. For a priority queue it is achievable, as we can make sure that we always modify the last element in the list, and the integrity of the leaf layer is never violated:

* To **add**, always add as the last element, then swim it up.
* To **pop**, swap the last element with the root, remove current last (former root), sink the current root (former last).

As both swimming and sinking don't change the shape of the tree, but only the sequence of elements, it solves the issue!

**Deletion** of elements from the middle is very similar to popping, just instead of exchanging with the root, we exchange the last element with this mid-element, shorten (trim) the heap, then **heapify** the impostor: either swim it up (if it is larger than its parent), or sink it (if it's heavier than kids). In practice it will usually have to sink, as it had come from the bottom, but it's not guaranteed, as it could have been a bottom of a different branch, and branches never promised to have similar distributions.

It is also possible to have **multi-way heaps** (not binary, but, say, tertiary).

It is also possible to use a priority queue as a sorting algo, called **Heap Sort**: just add all elements to a priority queue one by one, then pop them all. ~O(n log n), or more accurately, O(2n log n + 2n).

# Refs

* http://www.mathcs.emory.edu/~cheung/Courses/171/Syllabus/9-BinTree/heap-delete.html