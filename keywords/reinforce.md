# REINFORCE and Policy gradient RL

Parents: [[rl]]
See also: [[sgd]]


Tries to optimize policy $π$ directly, without V or Q functions. 

REINFORCE (like that, in all caps) is the oldest method of this type, from 1992. It uses a stochastic policy, outputting probabilities of every possible action, as a vector, parameterized by a vector θ. For discrete actions, probably a softmax distribution.

We then generate a bunch of trajectories. Stochasticity becomes critical here, as it's the only guarantee that different actions and different states will be visited. And for each trajectory we calculate the total return (sum of rewards) J.

Then we want to shift θ in the direction of higher returns. Before we do it, let's perform a trick called "likelihood-ratio identity".

The trick: let's take a derivative by x of log(p(x)). By chain rule, it will be equal to $\frac{1}{p(x)}p'(x)$. Re-arranging this, we get p'(x) = p(x)(log(p(x)))`. In vectorized form, and with derivatives by parameters, it looks even fancier: $∇_θp_θ(x) = p_θ(x)∇_θ\log p_θ(x)$. The gradient of log-probability on the right is called **score function**, and it's decomposable (products of probabilities turn into a sum of log-probabilities), which is super-important for trajectories, as a probability of following a prajectory is a product of probabilities of each transition, but if we're working with logs of probabilities, then we suddently have a sum of log-probabilities, which is a much saner situation.

Also its expectation is zero, which is allegedly also cool somehow (probably also improves some convergence or whatnot?) Why is E of score function zero? Coz to calculate the expectation E we need to multiply each d/dx log(p(x)) by its p_θ and then sum them up (aka sum by x, that is, sum or integrate across all the possible actions probability of which we are calculating). But once we multiplied nabla-logs by p, we get nabla-p, by the equation above. Integrating this nabla-p is the same as calculating gradient of an integral, but the integral across all p is equal to one, by definition of probability, and so nabla of it is equal to 0. So E score function = 0.

Now back to RL. We want to shift θ based on dJ/dθ. But J (the total return across a lot of trajectories produced by a policy) is essentially E of sum(R) over all trajectories. Now differentiate by θ. Using the trick above, d/dθ of p(trajectory)R(trajectory) becomes R(trajectory) d/dθ log(p(trajectory)), and each of this logs immediately breaks apart into a sum of logs of probabilities of individual transitions (actions A taken at state S) across the trajectory. So now we have a sum across trajectories, and inside, it, a sum over time along the trajectory. Then on top of that, we may want to break rewards per trajectory R_n into discounted end-rewards per transition, using a discounting term $γ$, leading to this:

$\displaystyle \nabla_θ J = 1/N \sum_{n=1}^{N} \sum_{t=0}^{T_n} \left( \nabla_θ \log p_θ(A|S) \sum_{t'=t}^{T_n} γ^{t-t} R_n \right)$

And now suddenly this gradient can be computed explicitly, as we know the formulas for p(A|S), and so can calculate the gradient of a log of these formulas, and we can compute the discounted rewards as well, and then multiply them, and sum it up.

Why doesn't everyone use REINFORCE then, if it's so simple? For one, it uses a probabilistic policy. You may say - well, at least it samples well, so there's no exploitation / exploration problem, right? Well, that's the thing, essentially, it heavily preferences exploration, and so converges very slowly, is noisy, and requires lots and lots of trajectories. For very simple tasks it works, but for complex environments it becomes intractable, and we need something smarter, like [[ppo]] for example.

# Refs

https://en.wikipedia.org/wiki/Policy_gradient_method