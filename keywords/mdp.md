# Markov Decision Process (mdp)

#rl #monte-carlo

Parents: [[13_MC]]
See also: [[11_RL]], [[q-learning]], [[09_Graphs]]

Partly random decision making; extension of **Markov Chains**. Imagine a Markov Chain with states; we are currently in state $s_i$, and states $\{s_j\}$ are available to be moved to. Then the decider takes one of the **actions** $a_k$. The system then moves to one of the states s_j randomly, but the probability of doing so p_ij depends on the action a that was taken. Once the transition to state $s_j$ happened, the decider is rewarded with a certain **reward** R_ij that is also a parameter.

Essentially, it is equivalent to adding a bunch of additional states $s_i a_k$ for every base state $s_i$, according to the number of actions that can be taken from state $s_i$. A transition from $s_i$ to $s_i a_k$ is not probabilistic, but algorithmic, while transitions from $s_i a_k$ to all available $s_j$ are probabilistic. The transition probabilities are parameters of the system and are not updated, but the decision-making process (aka **policy**) is updated. 

There are several ways to update policies. In the most common one, for each state we keep a prediction of how "good" it is, called its **value**.  We start with equal values, and then at each step propagate these values backwards, while injecting rewards, so that $V_i = \sum_j P_{iπj}(γV_j + R_{iπj})$ where $P_{iπj}$ is the probability of transition from i to j given policy π (that is, assuming that we take the current best action a while at s_i), and $R_{iπj}$ is a reward assuming policy π.

> Tbh it looks a bit like a network centrality calculation, or a stable flow calculation, or an eigenvalue calculation.

> Did I get it right that the reward propagates _backwards_? And so it can be injected at the end, and gradually shape the entire network?

But also, at each step, the policy π is updated, by picking the action that leads to the best current $V_j$ of all available: for each i we do $π_i = \argmax_a(\sum_j P_{iaj}(γV_j + R_{iaj}))$, where P_iaj and R_iaj are transition probabilities and rewards assuming each of the possible actions. And you do both updates again and again, until convergence, which maximizes the γ-discounted sum of V on a most probable path.

If R_ij are known, it becomes a supervised learning task. If they are unknown, it can be turned into a **reinforcement learning situation**, called [[q-learning]].

# Refs

Wiki: https://en.wikipedia.org/wiki/Markov_decision_process