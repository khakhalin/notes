# Binomial distribution and related formulas

Parents: [[stats]], combinatorics

#stats

**Binomial distribution:** toss a coin n times, count how many time you got H. A probability that you get k successes out of n attempts, given probability of H (success) of p is: $\displaystyle P(k,n,p) = \begin{pmatrix}n \\ k \end{pmatrix}p^k (1-p)^{n-k}$, where this thing in the beginning is a binomial coefficient: $\displaystyle \begin{pmatrix}n \\ k \end{pmatrix} = \frac{n!}{k!(n-k)!}$ . 

E(k) is obvious and = pn.

Var(k) is fun and = $np(1-p)$.