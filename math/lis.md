# Longest Increasing Subsequence
#algo

**Simple solution** is O(n²): assume that for every point we processed, we'll remember the longest sequence that ended at it (maybe a reference to the previous point in the sequence, and the total length of a sequence?). No, go through all points left to right. For each new point Xi, go through all points to the left from it (j in 0:i), and of all points that are Xj<Xi, that is, for all sequnces that Xi can extend, pick the one of highest length. Store this info for Xi; if necessary, update the current estimation of the longest found sequence, and the reference to it (the value of i). Do it for all points. At the end, you'll know the last element of the longest sequence, and for each element Xi in this sequence we'll have a pointer at the previous element of the sequence Xj, so we can construct the full winner sequence and return it.

**Fancy solution** is O(n log n), and relies on the fact that looking for the longest incomplete sequence among all sequences that end at a small enough value (Xj < Xi) is the same as looking for a small enough value (highest Xj that is still < Xi) among all longest incomplete sequences. And the benefit here is that these "incomplete sequences" can be arranged in an orderly way, allowing us to perform binary search (log N steps) instead of a complete look-through (N steps). Resulting in a final O(n log n) complexity.

The intuition here is that some points of {X} are just not useful, and will never be a part of the longest sequence. As you go left to right, if a new point is high, it may be added to a growing sequence. If a point is low, it may start a new promising sequence, or improve a sequence that was already going. But each improvement renders some previously used points obsolete. For example, in a sequence `0,4,2` the point 4 will never be a part of a sequence, as any sequence that could contain 4 (such as `0,4,5` for example), can also contain 2 (as in `0,2,5`). Moreover, 2 is strictly better than 4, as if the next point in the sequence happenes to be 3, it can extend `0,2`, but it cannot extend `0,4`. So by the time we observed `0,4,2`, we know that `0,2` is the best length-2 sequence currently available. While `0` is the best length-1 sequence currently available (it can get useful in the future if we stumble upon a subsequence `1,2,3` that can extend `0`). 

In other words, at any point, it is enough if we keep track of the best observed sequence of length k, for every available k. Indeed, two sequences length k would be both uniquely "worthy" only if the end at different values (coz otherwise they are interchangeable). But then one of them ends at a higher value, and this value can extend the other, forming a sequence length k+1. Which means that the original sequence length-k with a higher value at the end is not worth remembering. For each k, at each moment, we remember the sequence lengh k that ends at a lowest possible value!

But this means that finding the best sequence to extend, for each new Xi, is really easy, as they are all ordered, and we can use binary search! We can just binary search through sequences, looking for a highest ending value that is still below Xi. This would be the best sequence (length k) to extend with Xi, forming a new sequence length k+1. This new sequence length k+1 would be better (or at least not worse) than the old  sequence length k+1, as the old sequence clearly ended at something higher than Xi (just by virtue of not being found at the previous step).

So we keep doing that, and at the end, the longest sequence is the winning sequence. 

And in practice, of course, we don't have to remember entire sequences, but instead we can remember references from the last element of each sequence to the penultimate element, and so on. Once everything is analyzed, we can later reconstruct the best sequence by jumping through the references, from the end to the beginning.

# References:
* https://en.wikipedia.org/wiki/Longest_increasing_subsequence
* https://www.geeksforgeeks.org/construction-of-longest-monotonically-increasing-subsequence-n-log-n/
* http://manzzup.blogspot.com/2015/08/explained-finding-longest-increasing.html