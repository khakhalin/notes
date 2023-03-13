# Sorting
#algo

# Comparison of algorithms

* **Selection sort** (repeatedly find min element) is O(N^2) as it does the full triangle thing (loop in a loop) through the array. May be the fastest for up to a dosen of elements (depending on the overhead)
* **QuickSort** has O(n log n) on average, but still goes to O(n^2) for the worst case. 
* **Mergesort** is O(n log n) even for the worst case, but takes more space: O(n) instead of O(1) as most types of sort.
* **Heapsort** is mathematically the best (also O(n log n) for time, and O(1) for space), but heap is a slow data structure, making it slower than quicksort in practice. See [[priority_queue]] for implementation.
* Neither of these cool guys work well on nearly-sorted data though. For nearly-sorted, the best is **Insertion sort**: move through an array, and for each element, swim it up (via pairwise swaps with the upwards neighbor) while necessary. For one out-of-place element, it will only do ~n/3 swaps on average, and n in the worst case (n/3 as that's what the integral gives if you calculate the average expected length between 2 random points on a range	).
* **Timsort**: #todo Similar to MergeSort, but first collects runs (ordered sequences), then buffs them up with insertion sort if they are too small, then merges them. Default sorting algorithm in Python. Best complexity performance overall (guaranteed O(n log n) even for worst case + O(n) for best case (which is not true for most textbook sorts), and O(n) for space!) https://en.wikipedia.org/wiki/Timsort

Good properties a sorting algorithm may have:
* **Stability**: if several keys are identical, they remain in the same sequence after sorting.

Refs:
* https://www.bigocheatsheet.com/

# Quicksort

The logic:
1. Find a good element, called **pivot**. Ideally, a median. 
2. Move all elements < pivot to the left from it, all elements ≥  than pivot - to the right.
3. Repeat the same procedure recursively for left and right halves.

Fine-tuning:
* 3 may only have one half, if the pivot ended to be min or max (which is the worst pivot possible).
* How to do 2 in practice? First, hide pivot at the end of the array. Then have two pointers (i and j), set both to 0. Move the right pointer (j) to the right. If jth element is < pivot, swap it with i (early in the game - just leave it in place), and increase i by one. Once done, i would point at the first element ≥ pivot, so swap pivot with it (from its hiding place at the end of the array).
* There are some enlightned ways of choosing the pivot, to prevent worst-case scenario (either sorted array if you always pick last element, or anti-sorted array if you always pick the first element). The **median of three**: consider first, middle, and last element, and pick the median of them. While it is possible to generate an array that would give worst-case scenario on it, it is quite unlikely to happen in practice.

# Mergesort

1. Sort the left, and the right halves of the array (recursively).
2. Set two pointers, i an j, at the beginning of each half. Start picking the smaller of the two, then moving appropriate pointer. If one of the pointers reached the end of its segment, obviously, just take the entire tail of the other segment in.