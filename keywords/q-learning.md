# Q-Learning

Parents: [[rl]]
See also: [[mdp]]

#stub #rl


A type of RL on [[mdp]]. Compared to the description introduced in [[mdp]] card (with states S_i, rewards R and discount factor $γ$) we also introduce Q, which is a table of "qualities" (thus the name?) of State-Action combinations (a single value for each action after each state), and the learning rate of α. Then we let agents do something and update this table by a formula:

$\displaystyle Q(S_i,A) = (1- α)Q(S_i,A) + α\left(R + γ \max_a Q(S_j,a)\right)$ 

So basically we are slowly (α) evolving from starting values to observed values, where observed values incorporate observed rewards, but also assume that the next decision will be optimal (according to current estimations). Unlike what was described in the [[mdp]] note, we don't store our policy π and state values $V_i$ separately, but kinda combine them into Q(S,A) values.

Because Q(S,A) are a "table", the method works particularly well when the space of (S,A) is really a rectangular table; that is, when the same set of actions is available from every step, as in the case of an agent moving on a grid. Moreover, in this case, instead of storing Q(S,A) as a table of values, we can learn them DL-way, allowing "similar" S states to learn from each other. This would be called "Deep Q-learning", and it was SOTA RL back in 2014.

TODO: 🔥 
* Finish https://en.wikipedia.org/wiki/Q-learning
* What is Bellmann Equation? Why wiki has it named so deliberately?