# Latin Squares

#combinatorics

**Latin Square**: an n×n squre will with numbers, so that every row and every column has every number exactly once. 
**Mutually Orthogonal Latin Squares** or MOLS: two LS in which, if you superimpose them and consider them as tuples, every pair is met only once.

For example, the square $\begin{matrix}0&1&2\\1&2&0\\2&0&1\end{matrix}$  is Latin, as is $\begin{matrix}0&2&1\\1&0&2\\2&1&0\end{matrix}$ . If we combine them, we'll get a square $\begin{matrix}00&12&21\\11&20&02\\22&01&10\end{matrix}$, in which every two-digits is repeated only once, which means that the original two Latin squares were orthogonal. 

MOLS are related to **magic squares**, as if we look at this "Kronecker product" square as a bunch of numbers in modulo 3, it is equivalent to $\begin{matrix}0&5&7\\4&6&2\\8&1&3\end{matrix}$, which is a magic square (all numbers are used only once, and each row and column sum to 12).

The number of MOLS for different n is weird. No solutions for n=2 and 6, but between 4 and n-1 solutions for others. Cannot find a table anywhere. Hard to find for even n, but easy for odd (just do $a = (i+j) \mod n$ in one case, and $a=(2i+j) \mod n$ in another.

# Splitting students into groups

_I could find no clear solution, so just some musings here_

This is connected to the task of splitting students into groups so that every student was with another student only once, and supposedly this answer explains it, but somehow I cannot get it:
* https://math.stackexchange.com/questions/1240718/riddle-assigning-students-into-groups

If we have only 1 student in each group, then of course assigning them to different tasks each day is just one latin square (every student can be present in only 1 place every day, and every student should eventually do every assignment once).

If we have 2 students in each group, it's almost like a pair of mutually orthogonal latin squares, except students cannot work with a copy of themselves, so values like 00 and 11 are prohibited. It means that for n students, perfect distribution is impossible, as there are n^2 cells to fit (days by assignments), but only n(n-1)/2 possible pairs.

We can however try to minimize, rather than prevent, repetitions:
* https://math.stackexchange.com/questions/1632815/splitting-a-set-into-two-disjoint-sets-five-times-minimizing-pairs-in-the-same

# Refs
* https://en.wikipedia.org/wiki/Latin_square
* https://en.wikipedia.org/wiki/Orthogonal_array
* https://www.cut-the-knot.org/arithmetic/latin3.shtml
* https://www.mscs.dal.ca/~janssen/4370/Orthogonal_Latin_Squares_text.pdf

