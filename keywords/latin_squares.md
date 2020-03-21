# Latin Squares

#combinatorics

**Latin Square**: an n×n squre will with numbers, so that every row and every column has every number exactly once. 
**Mutually Orthogonal Latin Squares** or MOLS: two LS in which, if you superimpose them and consider them as tuples, every pair is met only once.

For example, the square $\begin{matrix}0&1&2\\1&2&0\\2&0&1\end{matrix}$  is Latin, as is $\begin{matrix}0&2&1\\1&0&2\\2&1&0\end{matrix}$ . If we combine them, we'll get a square $\begin{matrix}00&12&21\\11&20&02\\22&01&10\end{matrix}$, in which every two-digits is repeated only once, which means that the original two Latin squares were orthogonal. 

MOLS are related to magic squares, as if we look at this "Kronecker product" square as a bunch of numbers in modulo 3, it is equivalent to $\begin{matrix}0&5&7\\4&6&2\\8&1&3\end{matrix}$, which is a magic square (all numbers are used only once, and each row and column sum to 12).

The number of MOLS for different n is weird. No solutions for n=2 and 6, but between 4 and n-1 solutions for others. Cannot find a table anywhere. Hard to find for even n, but easy for odd (just do $a = (i+j) \mod n$ in one case, and $a=(2i+j) \mod n$ in another.

# Splitting students in groups

_I could find no clear solution, so just some musings here_

This is connected to the task of splitting students into groups so that every student was with another student only once, and supposedly this answer explains it, but somehow I cannot get it:
* https://math.stackexchange.com/questions/1240718/riddle-assigning-students-into-groups

For the beginning of the story, the the logic is not that bad. If we have 2n students, then we can create a pair of MOLS, rank n and stack them one on top of another. Now rows = students (2n), cols = days (n), and values in stacked LS show the groups (from 1 to n). As these are MOLS, if we Kronecker them, we'll get indices ij where i and j correspond to students from 1st and 2nd halfs. Similarly, you can do 3n for groups of 3, etc.

But what happens after n days? If k (the number of students in each group) is small, we may hope to find more orthogonal columns. But generally, it's intuitively obvious that we'd get more luck in finding new combinations if we part with original split of students into "supergroup1" and "supergroup2". But what to do? IDK.

A similar puzzle, but with the task to minimize, rather than prevent, repetitions:
* https://math.stackexchange.com/questions/1632815/splitting-a-set-into-two-disjoint-sets-five-times-minimizing-pairs-in-the-same

**Graphs?** Another approach: every pairing of students is an edge in a Fn graph. What if we go through the graph k nodes in a row, thus generating the splits. It's a salesman problem on a line graph of the original graph (**Line graph**: each edge of the original graph becomes a vertex of a new graph, and those verices of the new graph are connected that met in the original graph).

Now, if you manage to solve it, you get a set of n(n-1)/k tuples, k length each, with no shared pairs. We now need to arrange these k-tuples within a Latin Square, so that every row (assignment) had every number (student) only once, and every column (day) also had it only once. k-tuples are unordered, so they are not quite numbers in a certaion base...

# Refs
* https://en.wikipedia.org/wiki/Latin_square
* https://en.wikipedia.org/wiki/Orthogonal_array
* https://www.cut-the-knot.org/arithmetic/latin3.shtml
* https://www.mscs.dal.ca/~janssen/4370/Orthogonal_Latin_Squares_text.pdf

