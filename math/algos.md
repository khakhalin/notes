# Algorithms

#algo

## Big O notation
Different meaning in textbooks and in practical questions. In textbooks: upper bound, so if something is O(N), it is also O(N^2) by definition. In practice, just say the truth. 

* O(log N) appears when you have a tree, and always process one branch of this tree. Then the total number of steps = the number of branches = $\log_2 n$ . 
* If you have to visit every branch of a tree, the total number of operations is n (for leaves) + n/2 + n/4 + ... = 2n, so we have O(N).
* If we have to through all n elements at each recursion before going further, and if the recursion is good (we split in two equal sizes), then we have a recursion: T(n) = n + 2T(n/2) . Unraveling: T(n) = n + 2(n/2 + 2(n/4 + ... 2(1))) = n + n +  ... + n . This sum contains k ns where k = ceil(log_2 n), so the total O = nlogn. 
    * Quick sort has O(n log n) on average, but still goes to O(n^2) for the worst case. 
    * Mergesort is O(n log n) even for worst case, but takes more space.
    * Heapsort is mathematically the best, but 1) is real hart to explain, because of the heap data structure (sort of a tree), in practice is slower than quicksort.
* Selection sort (repeatedly find min element) is O(N^2) as it does the triangle thing (loop in a loop) through the array.