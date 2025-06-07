# Priority Queue (aka Binary Heap)

Parents: [[algos_trees]]
See also: [[dijkstra]], [[a-star]]

#algo #trees


A classic data structure that helps to move either `max` or `min` operation from O(N) to O(log N).

The idea is to arrange data points in a tree-like structure, with left and right branches being equivalent, the only logic of value placement being that the parent node is always "better" than both children. It means that finding the "best" value is always trivial (it's always in the root), and all work is shifted towards keeing the structure properly arranged.

A Heap has two tricky interfaces: **add value** and **remove best** (like popping from a stack). And also one trivial interface of "what is the current best value" (it's the root, duh!)

The two tricky interfaces are built on two internal processes: float up (often called **swim**), and **sink** down. People may also refer to them as "**sifting**": sifting up, or sifting down (or, in linear representations, sifting left and right; see below for details)

* **Swim** (or sift up): pick an element, and if it is better than its parent, swap it with the parent. Repeat until it either meets a parent that is not worse than it, or until it reaches the top and becomes a root.
* **Sink** (or sift down): if both kids are better than the element, pick the better of the kids, and swap them. Repeat this process until either both kids are worse (or rather, not better), or the element become a leaf (has no  kids).

# Binary Heap

In practice, instead of keeping this data as a linked tree, it is usually kept in a linear array, as a **Binary Heap**: a list with a dummy unused value in position 0. That way, for any element at position k, its kids can be found at positions 2k and 2k+1.

One little complication is that for a binary heap to work the left branch should always be filled before the right branch, so that there is no gap in presentation of the last layer (aka **complete tree**). Or we'll have to pad empty leaves with NaNs, instead of just making sure that the address of a potential child is ≤ n, where n is the current size of a heap. For a priority queue it is easily achievable, as we can make sure that we always modify the last element in the list (literally, the last element of a linear array), in which case the integrity of the leaf layer (the shape of the tree) will never be violated. We'll then just move elements within a fixed shape!

* To **add**, always add a new element as the last element, then swim it up.
* To **pop**, swap the last element with the root, remove the current last (former root); then sink the current root (former last).

**Deletion** of elements from the middle is similar to popping, just instead of exchanging with the root, we exchange the last element with this mid-element, shorten (trim) the heap, then **heapify** the impostor: either swim it up (if it is larger than its parent), or sink it (if it's heavier than kids). In practice it will almost always have to sink, as it had come from the bottom, but it's not guaranteed, as it could have been a bottom of a different branch, and branches can have very different distributions.

It is also possible to have **multi-way heaps** (not binary, but, say, tertiary).

It is also possible to use a priority queue as a sorting algo, called **Heap Sort**: just add all elements to a priority queue one by one, then pop them all. ~O(n log n), or more accurately, O(2n log n + 2n).

# Refs

* http://www.mathcs.emory.edu/~cheung/Courses/171/Syllabus/9-BinTree/heap-delete.html