# Kronecker Graph Model

Parent: [[09_Graphs]]
Related: [[small_world]]

#graph #fractal #generative


Recursive (self-similar) generation of networks. Start with a small seed graph, and expand recursively. Take an adjacenty matrix K_1, then replace every 1 with K_1 as a block-matrix, and every 0 with a similarly sized block of 0. You get K_2. Repeat recursively. Very parallelizable, so good for creating very large graphs.

May be written using Kronecker product: C = A⨂B if we replace every a_ij with a block matrix a_ij∙B .

Kronecker graph: $\displaystyle K^{[m]} = \bigotimes^m K_1$ . Can also do various matrices at each step: $\displaystyle \bigotimes^m_{i=1} K_i$.

But isn't it too regular? Yes, so let's make it **stochastic**: instead of doing all 0/1, let's work with probabilities, calculate the final probabilities matrix $Θ_k$, and then at the very end replace each element p_ij with either 1 or 0 with probability p_ij, producing K_k. 

This is computationally expensive though (~$(n_1^m)^2$ weighted coin flips, where n_1 is the number of elements in K_1)! A way to fix: use the fact that IRL graphs are very sparse, and instead of flipping a coin for every possible edge, just decide how many edges we need, and find where  they would be. To do so, remember all K_i, and drop a fixed number of edges from the first graph K_1 down, at each step deciding to what quadrant the edge will go (like a funneling Galton board). This way we only need to generate random numbers $En_1 m$ times, where E is the total number of edges, n_1 is the size of the seed graph K_1, and m is the number of recursive expanions we performed.

> Obvious homework: generate this graph for some matrix of your choice; measure its properties.

By playing with generating matrices, it is possible to bring Kronecker graphs very close to large IRL networks. In practice, for a vanilla stochastic Kronecker graph the **degree distribution is very spiky**, so in practice people use special tricks to smooth it out.

> Homework: figure out how to smooth it out, and do it.

Original publication: Leskovec, J., Chakrabarti, D., Kleinberg, J., Faloutsos, C., & Ghahramani, Z. (2010). Kronecker graphs: an approach to modeling networks. Journal of Machine Learning Research, 11(2).
https://arxiv.org/abs/0812.4905
(~800 citations; first variant in 2008, so spent 2 years in review, huh)