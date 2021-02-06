# Do Reservoir Computers Work Best at the Edge of Chaos?

Carroll, T. L. (2020). Do Reservoir Computers Work Best at the Edge of Chaos?. arXiv preprint arXiv:2012.01409.
https://arxiv.org/pdf/2012.01409.pdf

#chaos #echo #complexity #networks

Parents: [[echo]], [[chaos]]
Related: [[Carroll2019structure]] (previous paper), [[criticality]]


Cellular Automata ([[cellular_automata]]) have highest computational complexity at the edge of chaos, but as this paper shows, it's not quite true for reservoir computing, as at the edge of stability performance drops.

He uses some special types of echo networks: a **polynomial ODE**, and a **polynomial map** reservoirs (described in one of the earlier works). In both cases for each node you take it's previous state, calculate a cubic polynomial of it, add inputs, and either call it a derivative of a value for this node, or just the new value of this node. He claims it is, like, super-flexible, and diverse. 100 nodes, random edges. Fed the system with x signal from the Lorenz system [[lorenz]], but trained on z signal.

So not test self-generation (!), but rather a transformation of one weird signal into another one. What's the logic here? As the Lorenz system is 3D, there's always a latent variable in play. So it seems that they're saying that the echo state network is supposed to gradually infer the state of this "other" variable from the values of x and z variables that are "given" to it. (Even though they are "given" in two different ways). They speak of "embedding" the system in the network (or failing to do so), which suggests that in a way the network does indeed "figure out" the missing parameter from the activity. 

For that reason, the other signal that they use (a map) is also 3D (one dimension more than what the network can observe). We don't want network to just "fit" some fixed function; we want it to have to learn the hidden state. The 3D system they have:

$\displaystyle x ← \begin{pmatrix} 1.1 & 0 & 1 \\ 1 & 0 & 0 \\ 0 & 1 & 0 \end{pmatrix}\cdot(x \; \text{mod} \; 1)$

Calculated the Lyapunov exponent [[lyapunov-exp]] using a smart method:  T. S. Parker and L. O. Chua, Practical Numerical Algorithms for Chaotic Systems. (Springer-Verlag, New York, 1989).

Also looked at fractal dimension and entropy of the system.

> How do they calculate the entropy of a network?

**Their main answer** is actually not like in the title. Overall, they tested a continuous reservoir (ODE) and a map, and both were used to predict a continuous signal (Lorenz) and a discrete map. So they found that in 3 out of 4 cases (ODE fitting both signals, and map fitting Lorez) best performance is actually exactly at criticality (immediately before model divergence). And only for the fourth option (a map fitting a map), best performance (lowest error, the difference between the prediction and the actual behavior) is optimal somewhere half-way between underperformance and criticality.

# Potentially relevant Refs

Lu, Z., Hunt, B. R., & Ott, E. (2018). Attractor reconstruction by machine learning. Chaos: An Interdisciplinary Journal of Nonlinear Science, 28(6), 061104.
https://arxiv.org/pdf/1805.03362.pdf

Lymburn, T., Walker, D. M., Small, M., & Jüngling, T. (2019). The reservoir’s perspective on generalized synchronization. Chaos: An Interdisciplinary Journal of Nonlinear Science, 29(9), 093133.
https://people.bath.ac.uk/jhpd20/publications/HartHookDawes_preprint_JAN2020.pdf
Nice intro, then lots of math.

Hart, A., Hook, J., & Dawes, J. (2020). Embedding and approximation theorems for echo state networks. Neural Networks.

# Refs that are too hard to read

Grigoryeva, L., Hart, A., & Ortega, J. P. (2020). Chaos on compact manifolds: Differentiable synchronizations beyond Takens. arXiv preprint arXiv:2010.03218.
