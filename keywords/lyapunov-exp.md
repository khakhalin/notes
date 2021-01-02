# Lyapunov exponent

#chaos

Parent: [[chaos]]
Related: [[fractal]], [[echo_state]]

We take a **dynamic system** with chaotic instability, and consider two sets of initial conditions that differ by a very small value δ. Then we track how this difference evolves over time. If trajectories diverge, it will increase, and in fact it will (at least at first) diverge exponentially. That is:

$\displaystyle |y_δ(t)-y_0(t)| \approx e^{-λt}\cdot |δ|$ , where y_0 started from some initial conditions, and y_δ started from same conditions + δ. The exponential constant λ is the **Lyapunov's exponent**.

As different trajectories may demonstrate different divergence, typically we look at a max of all possible λ for a given system.

The easiest way to find: build a bunch of coordinates, then calculate a ratio of Δ(t)/δ, take the log() of it, and fit it with a linear slope. Although if λ are very different for different trajectories, finding the one that yields the highest λ may be tricky.

See Wiki for:
* A better (more informed, mathy, vector) way to find λ numerically
* A connection with fractal dimension for the attractor (in this case apparently called **Kaplan-Yorke dimension**)

# Refs

Smart way to calculate it:
 T. S. Parker and L. O. Chua, Practical Numerical Algorithms for Chaotic Systems. (Springer-Verlag, New York,
1989).

https://en.wikipedia.org/wiki/Kaplan%E2%80%93Yorke_conjecture

https://en.wikipedia.org/wiki/Lyapunov_exponent