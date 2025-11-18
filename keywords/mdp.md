# Markov Decision Process (mdp)

Parents: [[modeling]]
See also: [[rl]], [[q-learning]], [[graphs]]

#rl #monte-carlo


Partly random decision-making; extension of **Markov Chains**. Imagine a Markov Chain with states; we are currently in state $s_i$, and can move to states $\{s_j\}$ from it. There's a decider that takes one of the **actions** $\{a_k\}$. The system then moves to one of the states $s_j$ randomly, and the probability of doing so $p_{kij}$ depends on the action $a_k$ that was taken (these probabilities are not something we choose, but are parameters of the system). Once the transition to state $s_j$ happened, the decider may be rewarded with a certain **reward** $R_{ij}$ that is also a parameter of the system.

From the graph traversal POV this process is equivalent to adding a bunch of additional states $s_i a_k$ for every base state $s_i$, according to the number of actions that can be taken from state $s_i$. A transition from $s_i$ to $s_i a_k$ is algorithmic, while transitions from $s_i a_k$ to $s_j$ are probabilistic. The transition probabilities and the rewards are out of our control and don't change over time (are a part of the problem, not the solution), but the decision-making process (aka **policy**) is updated. Policies (choosing an action $a_k$) may be either fully deterministic (e.g. picking the best policy), or probabilistic.

There are several ways to update policies. In the most common ones, to aid the policy search, we introduce an estimation of how "good" every state is, called the **value** of the state $V_i$.  We start with equal values, and then at each step propagate these values backwards, while injecting rewards, by formula: $V_i = \sum_j P_{iπj}(γV_j + R_{iπj})$ where $P_{iπj}$ is the probability of transition from i to j given policy π (that is, assuming that we take the current best action $a$ while at $s_i$), and $R_{iπj}$ is a reward assuming policy π. The value $γ$ here is a discounting factor ($γ<1$) that makes you discount future rewards and values, because who knows if you will ever get there.

Also, at each step, the policy π is updated, by picking the action that maximizes the probability of transition to the most useful state, that is, the state with the highest current $V_j$ available: $π_i = \argmax_a(\sum_j P_{iaj}(γV_j + R_{iaj}))$, where P_iaj and R_iaj are transition probabilities and rewards assuming each of the possible actions. Now all we need is to repeat both updates again and again, until convergence that maximizes the γ-discounted sum of V on a most probable path.

If R_ij are known, it becomes a supervised learning task. If they are unknown, it can be turned into a **reinforcement learning situation**, called [[q-learning]].

# Refs

Wiki: https://en.wikipedia.org/wiki/Markov_decision_process