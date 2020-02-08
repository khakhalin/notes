# Algorithms
#algo #bib

Subtopics:
* [[graph_algos]] - everything about graphs

Data structures:
* [[priority_queue]]

Other curious algorithms
* [[lis]] - Longest Increasing Subsequence

Todo: #todo
* https://en.wikipedia.org/wiki/Maximum_subarray_problem

# Big O notation
Limiting behavior at ∞. Different meaning in textbooks and in practical questions. In textbooks: upper bound, so if something is O(N), it is also O(N²) by definition. In practice, just say the truth. 

* O(log N) appears when you have a tree, and a typical engagement with this tree is processing a branch of it. Then the total number of steps = the depth of the tree = $\log_2 n$ . 
* If however you have to visit every branch of a tree, the total number of operations is n (for leaves) + n/2 + n/4 + ... = 2n, so we have O(N).
* If we go through n elements and then use a divide-and-conquer approach, splitting in two equal sizes with recursion, then we have the complexity: T(n) = n + 2T(n/2). Unraveling this: T(n) = n + 2(n/2 + 2(n/4 + ... 2(1))) = n + n +  ... + n. This sum contains k ns where k = ceil(log_2 n), so the total T= O(N logN)

# Sorting
* **QuickSort** has O(n log n) on average, but still goes to O(n^2) for the worst case. 
* **Mergesort** is O(n log n) even for the worst case, but takes more space: O(n) instead of O(1) as most types of sort.
* **Heapsort** is mathematically the best (also O(n log n) for time, and O(1) for space), but heap is a slow data structure, making it slower than quicksort in practice. (See [[priority_queue]])
* **Selection sort** (repeatedly find min element) is O(N^2) as it does the full triangle thing (loop in a loop) through the array.

# Classic algorithms

**Median of medians**: Recursive way to find a median in linear time (median through sorting is O(n lg n) ). Imagine we have a linear agorithm for finding i-th element (i=N/2 for a median). Split the array into groups of 5. Find median of each; then find the median of medians. It is guaranteed to be between 30th and 70th percentiles (just draw it for a proof). Set this value as a pivot. Binary split all elements into those lower than the pivot, and those higher than it. If upper=lower, pivot is the median. If not, look either for the k-th elements of the upper part (if upper is larger), or i-k-1 -th elements of the lower part (if lower is larger.)  Because the size of the array is guaranteed to shrink fast (not slower than 0.7 for main recursion step, and 0.2 for the median of median), by induction, the entire thing is O(n), just multiplied by some weird factor. Refs: [wiki](https://en.wikipedia.org/wiki/Median_of_medians), [better explanation](https://www.austinrochford.com/posts/2013-10-28-median-of-medians.html)

**Stable matching algorithm**: A elements rank B elements by preference; B elements rank A elements by preference; find a matching such that no cross-wiring (swapping) of matches can improve the match (for all those 4 that would be cross-wired). The logic: each A proposes to their B. Each B picks the best A of those that proposed. All A that aren't matched propose to their second choice. If B can upgrade, they upgrade. Repeat until everyone are matched. Proof that all matched at the end: the least lucky A will propose to all B, so there cannot be a free B. Prove that the match is stable: if KL MN can be improved to KN ML, then K prefers N, but then K proposed ot N earlier; as N is with M, N must have traded K for a better match after that, so MN→KN isn't an improvement for N, contradiction. Reference: [wiki for the problem](https://en.wikipedia.org/wiki/Stable_marriage_problem), [wiki for the algorithm](https://en.wikipedia.org/wiki/Gale%E2%80%93Shapley_algorithm)