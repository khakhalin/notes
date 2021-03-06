# Algorithms - General
#algo #bib

Subtopics:
* [[algos_graph]] - everything graphs
* [[algos_trees]] - everything trees
* [[sorting]] algorithms
* [[np-complete]] - P, NP, and NP-complete

Popular data structures:
* [[hash]] - hashmaps, hashtables, and hashing functions
* [[priority_queue]] - priority queue, heap, heap sort

Misc curious algorithms
* [[gcd]] - aka "Euclidean algorithm"
* [[nimbers]] - on effectively solving stone games
* [[lis]] - longest Increasing Subsequence
* [[bridson]] - sampling on a 2D plane

Todo: #todo
* https://en.wikipedia.org/wiki/Maximum_subarray_problem

# Big O notation
Limiting behavior at ∞. Different meaning in textbooks and in practical questions. In textbooks: upper bound, so if something is O(N), it is also O(N²) by definition (as N²>N). In practice, during interviews just say the truth (the closest approximation, aka lowest possible upper bound). 

* O(log N) appears when you have a tree, and a typical engagement with this tree is processing a branch of it. Then the total number of steps = the depth of the tree = $\log_2 n$ . 
* If however you have to visit every branch of a tree, the total number of operations is n (for leaves) + n/2 + n/4 + ... = 2n, so we have O(N).
* If we go through n elements and use a divide-and-conquer approach, splitting the list in two equal sizes with recursion, then we have the complexity: T(n) = n + 2T(n/2). Unraveling this: T(n) = n + 2(n/2 + 2(n/4 + ... 2(1))) = n + n +  ... + n. This sum contains k ns where k = ceil(log_2 n), so the total T= O(N logN)

It seems that for dynamic programming, the time complexity is the number of subproblems considered. Which in most cases matches the size of the memoization structure, as memoization is based on the structure of problem space. So O for time = O for space = max size of the grid used for memoization. Exceptions are possible, but only when memoization doesn't quite fit the structure of the problem, which is should be fairly easy to spot / feel.

Refs:
* https://www.bigocheatsheet.com/ - Big "O" cheat sheet
* [That for dynamic programming time and space are usually the same](http://www.fairlynerdy.com/dynamic-programming-time-complexity/)

# Misc classic algorithms

**Median of medians**: Recursive way to find a median in linear time (median through sorting is O(n lg n) ). Imagine we have a linear agorithm for finding i-th element (i=N/2 for a median). Split the array into groups of 5. Find median of each; then find the median of medians. It is guaranteed to be between 30th and 70th percentiles (just draw it for a proof). Set this value as a pivot. Binary split all elements into those lower than the pivot, and those higher than it. If upper=lower, pivot is the median. If not, look either for the k-th elements of the upper part (if upper is larger), or i-k-1 -th elements of the lower part (if lower is larger.)  Because the size of the array is guaranteed to shrink fast (not slower than 0.7 for main recursion step, and 0.2 for the median of median), by induction, the entire thing is O(n), just multiplied by some weird factor. Refs: [wiki](https://en.wikipedia.org/wiki/Median_of_medians), [better explanation](https://www.austinrochford.com/posts/2013-10-28-median-of-medians.html)

**Stable matching algorithm**: A elements rank B elements by preference; B elements rank A elements by preference; find a matching such that no cross-wiring (swapping) of matches can improve the match (for all those 4 that would be cross-wired). The logic: each A proposes to their preferred B. Each B picks the best A of those that proposed. All A that aren't matched propose to their next choice. If B can upgrade, they upgrade. Repeat until everyone are matched. Proof that all matched at the end: the least lucky A will propose to all B, so there cannot be a free B. Prove that the match is stable: if KL MN can be improved to KN ML, then K prefers N, but then K proposed ot N earlier; as N is with M, N must have traded K for a better match at some opint, so MN→KN isn't an improvement for N, which gives a contradiction. Reference: [wiki for the problem](https://en.wikipedia.org/wiki/Stable_marriage_problem), [wiki for the algorithm](https://en.wikipedia.org/wiki/Gale%E2%80%93Shapley_algorithm)