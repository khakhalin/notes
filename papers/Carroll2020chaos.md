# Do Reservoir Computers Work Best at the Edge of Chaos?

Carroll, T. L. (2020). Do Reservoir Computers Work Best at the Edge of Chaos?. arXiv preprint arXiv:2012.01409.
https://arxiv.org/pdf/2012.01409.pdf

#chaos #echo #complexity #networks

Parents: [[echo_state]], [[chaos]]
Related: [[Carroll2019structure]] (previous)


Cellular Automata ([[cellular_automata]]) have highest computational complexity at the edge of chaos, but as this paper shows, it's not quite true for reservoir computing, as at the edge of stability performance drops.

He uses some special types of echo networks: a **polynomial ODE**, and a **polynomial map** reservoirs (described in one of the earlier works). In both cases for each node you take it's previous state, calculate a cubic polynomial of it, add inputs, and either call it a derivative of a value for this node, or just the new value of this node. He claims it is, like, super-flexible, and diverse. 100 nodes, random edges. Fed the system with x signal from the Lorenz system [[lorenz]], but trained on z signal.

> So not self-generation!! But rather a transformation of one weird signal into another one. Is it easier to test the quality like that?

> I still don't really understand it, as 

Calculated the Lyapunov exponent [[lyapunov-exp]] using a smart method:  T. S. Parker and L. O. Chua, Practical Numerical Algorithms for Chaotic Systems. (Springer-Verlag, New York,
1989).