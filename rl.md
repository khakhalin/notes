# Reinforcement learning

Parents: [[dl]]
See also: [[credit]] (rl-like thing in the brain), [[modeling]], [[job_shop]]

#rl #bib


RL is about **agents** interacting in the **environment** by performing **actions** that push them from one **state** to another, and collecting rare **rewards** (often very long-term, summative signals at the end of a whole trajectory of actions). In a way, RL is placed in-between supervised and unsupervised learning, as there is some feedback (so you are not doomed to only draw from the patterns of the data itself), but this feedback is so scarce and rarefied that you cannot immediately know the quality of each of your elementary steps: you have to fight for getting this info.

Subtopics:
* [[mdp]] - Markov Decision Process (the main abstraction underlying RL)
* [[multi_armed_bandit]] - the statistical abstraction behind exlporation / exploitation dilemma
* [[q-learning]] - one of the simplest methods of RL
* [[ppo]] - the most popular RL method ever

Terminology:
* **Policy**: A strategy / rule used by the agent to choose actions, based on its state.
* **Value Function**: Estimated expected cumulative discounted reward to be collected from a state
* **Q-Function**: Value function defined for actions rather than states
* **Advantage**: The difference between the value of taking a specific action and the average value of actions in a state.​
* **Episode**: A sequence of states, actions, and rewards that ends in a terminal state.​
* **Discount Factor**: When calculating V and Q, we discount future rewards. Usually represented by $γ$
* **Off-Policy learning**: when we can update the policy based on traces not produced by this policy
* **Actor-Critic** - a collection of methods, including A2C, [[q-learning]], SARSA etc, with 2 processes: actor acting on a policy, and updating it, and critic estimating either value functions V of states, action-value functions Q for actions, or advantage A, or their combination.

Open questions: 🔥
* What is model-based vs model-free RL?
* Bellman optimality equation

## RL methods

* Most popular approaches:
    * [[reinforce]] - aka Policy Gradient Methods, a group of simple methods that don't rely on V or Q. Work well when episodes are short enough and are easy to generate, so you don't have to invest too much into mapping "secretly good" and "secretly bad" states (using V); you can just take trajectories as a whole and calculate the **return**: a cumulative sum of all rewards.
    * [[ppo]] - the most common method as of 2025.
    * DQN - discrete actions.
    * SAC (Soft Actor-Critic) - continuous control, robotics
* A2C (Advantage Actor-Critic) - simpler than PPO, but less stable and less sample-efficient. 
* SARSA (State–action–reward–state–action). on-policy.
* TD (Temporal Difference) - model free.
* Q-Mix - cooperative multi-agent settings, discrete only. Assumes lots of agents, each contributing a bit to the goal, and not competing with each other. Doesn't work that well when agent-level greedy strategies are not expected to synergize.
* DDPG (Deep Deterministic Policy Gradient) - continuous spaces, robotics
* TD3 (Twin Delayed DDPG) - continous control
* MADDPG - continuos actions.
* Hierarchical RL - long-term, subpolicies
* MBRL - Model-based RL. It feels to lie roughly in-between deterministic methods (mixed-integer modeling, dynamic programming) and model-free RL. It's way more sample-efficient, as it knows how the model works, and can extrapolate a lot, but more computationally expensive. In practice, people don't seem to like MBLR too much, as it's too hard to set up, with its precise recapitulation of the process. No separate Wikipedia page, which is telling.

Summary table. Year when is according to Perplexity, and may be off, but at least the decade is probably true?

| Method name | Continuous/ Discrete | On/Off-Policy | Complexity | Year when | Strength                                      | Areas                               | Common |
|-------------|----------------------|---------------|------------|-----------|-----------------------------------------------|-------------------------------------|--------|
| PPO         | Both                 | On            | Mid        | 2017      | Stable, simple, but inefficient               | Robotics, games, continuous control | 3      |
| A2C         | Both                 | On            | Easy       | 2016      | Simple, stable, but finicky                   | Games, simple control tasks         | 2      |
| SARSA       | Discrete             | On            | Easy       | 1994      | Safe, but slow                                | Grid worlds, simple MDPs            | 2      |
| TD          | Both                 | Both          | Easy       | 1988      | Foundation method, simple, never used alone   | Foundational (used within others)   | 2      |
| DQN         | Discrete             | Off           | Mid        | 2015      | Sample efficient, stable                      | Atari games, discrete control       | 3      |
| Q-Mix       | Discrete             | Off           | Hard       | 2018      | Decentralized execution, centralized training | Multi-agent games (StarCraft)       | 1      |
| MADDPG      | Continuous           | Off           | Hard       | 2017      | Continuous multi-agent control                | Multi-agent robotics, continuous    | 1      |
| DDPG        | Continuous           | Off           | Mid        | 2015      | Brittle                                       | Robotics, continuous control        | 2      |
| TD3         | Continuous           | Off           | Mid        | 2018      | Improves DDPG                                 | Robotics, locomotion, manipulation  | 2      |
| IMPALA      | Both                 | Off           | Hard       | 2018      | Massive scalability, throughput, but complex  | Large-scale multi-task learning     | 1      |
| SAC         | Continuous           | Off           | Mid        | 2018      | Sample efficient, robust                      | Real-world robotics, manipulation   | 3      |
| HDQN        | Discrete             | Off           | Hard       | 2016      | Long-horizon sparse rewards                   | Sparse reward games (Montezuma)     | 1      |

## Tricks

**Reward shaping**: on top of the final reward for the episode, or natural "big rewards" for task completion, introduce "encouragement" on the way, helping agents to get to the task. This can make learning much faster, but lead to suboptimal behaviors. One of the ideal situations is **Potential-Based Reward Shaping** - when we really can rank states in terms of how close they are to the goal. Other cool tricks - **Curriculum Reward Sharping** or **Reward Annealing**, when we start with a support structure, but then gradually deprioritize it.

**Curriculum learning**: start with simple tasks, then gradually add complexity. See [[curriculum]]

**Warm-starting**: (aka initializing with heuristics, or imitation learning). One way to achieve it is with **pre-training**, using supervised learning to learn some successful traces, and only then starting to improve from there. Or a very crude version of it called **heuristic initialization** when we don't even bother to imitate successful traces, but just start with very raw (but useful) biases.

**Epsilon-greedy exploration**: A similar technique where we use one policy with $1-ϵ$ probability, and a riskier policy with $ϵ$ probability. For warm-start, we can use heuristic in 90% of cases and the new yet untrained policy in 10% of cases, and only learn from them (and gradually increase their share, aka **decaying epsilon**). Or the other way around, we can use the main policy, the one that we are learning, in 90% of cases, and random actions in 10% of cases, to encourage exploration. It is important when the agents may get stuck, or when the rewards are very sparse.

**Intrinsic motivation**: we can add reward for visiting states that were never visited before, or that are rarely visited (aka **novelty-seeking**), or for taking actions that are rarely taken. We can also add reward proportional to prediction error (aka **curiosity**), encouraging agents to explore those action where they are most mistaken about the state to which they will lead. The cool thing about curiosity is that if we make reward proportional to prediction error, and our network is expressive enough, we will gradually learn the true consequences of our actions, prediction error will go down, and so the impact of curiosity on behavior will go down. As curiosity is about states, it's most effective in non-deterministic environments, but it is often used in deterministic environments as well, especially when we are not comparing states directly, but are comparing their representations, as in the case of [[ppo]]. Curiosity can however turn agents into gamblers, addicted to actions with 50/50 consequences. **Value curiosity** also exists (aka TD, or Temporal Difference intrinsic motivation, or learning from surprise), but it is less commonly used.

**Hybrid approaches**: for example, RL for high-level decision-making, and deterministic techniques (dynamic modeling, [[constraint_programming]]) for local actions.

## Papers

* [[Baker2019autocurricula]] - RL agents by OpenAI playing hide-and-seek, with pretty videos

The Surprising Effectiveness of PPO in Cooperative, Multi-Agent Games
Chao Yu, Akash Velu, Eugene Vinitsky, Jiaxuan Gao, Yu Wang, Alexandre Bayen, Yi Wu
https://arxiv.org/abs/2103.01955

Adaptive Reward Penalty in Safe Reinforcement Learning
In this blog, we dive into the ICLR 2019 paper Reward Constrained Policy Optimization (RCPO) by Tessler et al. 
https://iclr-blogposts.github.io/staging/blog/2023/Adaptive-Reward-Penalty-in-Safe-Reinforcement-Learning/

https://www.argmin.net/p/defining-reinforcement-learning-down
https://www.argmin.net/p/theres-got-to-be-a-better-way
https://www.argmin.net/p/there-is-no-data-generating-distribution
https://www.argmin.net/p/benchmark-studies

# Top reading

Reinforcement learning: An introduction. Richard Sutton. 2021. SIAM Rev, 6(2), 423. - simpler book, a good start

Andrychowicz, M., Raichuk, A., Stańczyk, P., Orsini, M., Girgin, S., Marinier, R., ... & Bachem, O. (2020). What matters in on-policy reinforcement learning? a large-scale empirical study. arXiv preprint arXiv:2006.05990.
https://arxiv.org/pdf/2006.05990

🔥 Spinning Up in Deep RL: https://spinningup.openai.com/en/latest/
A deeper and more practical introduction to the subject

🔥 CleanRL library - practical examples

Since 2017 PPO is the commonly accepted SOTA for RL:
https://en.wikipedia.org/wiki/Proximal_policy_optimization

Exploration in RL (a sort of blog-review with math, 2020)
https://lilianweng.github.io/lil-log/2020/06/07/exploration-strategies-in-deep-reinforcement-learning.html)

Nice collection of advice on reinforcement learning (non-mathy, applicable to almost any experimental ML): https://rll.berkeley.edu/deeprlcourse/docs/nuts-and-bolts.pdf
(by John Schulman, 2016)

See also: [[rl_questions]] 🔥 - remove eventually

# Tools

* Stable Baselines3 - an almost plug-and-play set of tools for starting to do RL. Uses OpenAI's Gym.

# Random reading

Curriculum for Reinforcement Learning. Lilian Weng. Jan 29 2020.
https://lilianweng.github.io/lil-log/2020/01/29/curriculum-for-reinforcement-learning.html

Matusch, B., Ba, J., & Hafner, D. (2020). Evaluating Agents without Rewards. arXiv preprint arXiv:2012.11538.
https://arxiv.org/pdf/2012.11538.pdf
Exploration, curiosity, self-driven learning (including in Minecraft)

Reinforcement Learning for Language Models. Yoav Goldberg, April 2023.
https://gist.github.com/yoavg/6bff0fecd65950898eba1bb321cfbd81

Mnih, V., Kavukcuoglu, K., Silver, D., Rusu, A. A., Veness, J., Bellemare, M. G., ... & Petersen, S. (2015). Human-level control through deep reinforcement learning. Nature, 518(7540), 529-533.
https://daiwk.github.io/assets/dqn.pdf
A popular RL paper about Atari games. Other papers (PCGRL: Khalifa 2020) reference it as "fractal networks", even though the term isn't used in this paper itself.

Reward-Conditioned Policies. Aviral Kumar, Xue Bin Peng, Sergey Levine. 2019.
https://arxiv.org/abs/1912.13465
Some sort of mix between RL, learning by imitation, and trying to infer better policies from observed examples. Kinda like self-supervised, in a sense of introducing an inner supervised loop that feeds from the outer RL loop. The intro sounds as if they were claiming that learning bad policies together with the rewards they yield can be helpful for extrapolating to better policies? Not sure.

Alexander Tschantz, Beren Millidge, Anil K. Seth, Christopher L. Buckley (2020). Reinforcement Learning through Active Inference
https://arxiv.org/abs/2002.12636
Instead of maximizing rewards, they maximize evidence for an internal generative model. Also use _free energy of the expected future_. Hmm. [[freeenergy]]? Looks quite mathy.

## Meta-writing

Ben Recht, the main idea (and math) of RL in a single paragraph:
https://www.argmin.net/p/reformist-reinforcement-learning

## Robots

Ibarz, J., Tan, J., Finn, C., Kalakrishnan, M., Pastor, P., & Levine, S. (2021). How to train your robot with deep reinforcement learning: lessons we have learned. The International Journal of Robotics Research, 0278364920987859.
https://arxiv.org/abs/2102.02915

Sharma, A., Ahn, M., Levine, S., Kumar, V., Hausman, K., & Gu, S. (2020). Emergent Real-World Robotic Skills via Unsupervised Off-Policy Reinforcement Learning. arXiv preprint arXiv:2004.12974.
https://arxiv.org/abs/2004.12974
Sharma, A., Gu, S., Levine, S., Kumar, V., & Hausman, K. (2019). Dynamics-aware unsupervised discovery of skills. arXiv preprint arXiv:1907.01657.
https://arxiv.org/abs/1907.01657
A sequence of two papers that explore unsupervised exploration of possible behaviors in a virtual environment. SOTA?
Also this write-up:
https://ai.googleblog.com/2020/05/dads-unsupervised-reinforcement.html

## Curiocities

* [Minigrid](https://github.com/maximecb/gym-minigrid/) - minimalistic RL environment

# RL questions

## Part 1 - Basics

> Define reinforcement learning and explain how it differs from supervised and unsupervised learning

Listen for clarity on the feedback loop, agent-environment interaction, and long-term reward maximization. Weaker candidates conflate RL with other paradigms; strong ones explain the sequential decision-making aspect

> Define the core components: agent, environment, state, action, reward, and transition dynamics

Transition dynamics may be deterministic or non-deterministic. Also it may be model-based (we know transition rules) or model-free (we have to learn the value of policies from experience).

> For a job-shop schedulig task, define the state, the actions, and the reward

State: The current configuration of the shop, including the status of all jobs (which tasks are pending, in progress, or completed), the status of all machines (idle, busy, or broken), and the allocation of resources to jobs.​

Actions: The decision to assign a specific job to a specific machine, or to move a job from one machine to another, or to start or stop a job on a machine.​

Reward: A signal that reflects the quality of the scheduling decision, such as minimizing the makespan (total time to complete all jobs), reducing idle time, or penalizing constraint violations (e.g., missing deadlines or resource conflicts). Typically, episode-level reward at the end of the episode.

> "What is the Markov property, and why does it matter for RL?"

Strong candidates link this to state representation quality and explain the trade-off between state simplicity and information loss.

## Part 2 - Details

"Explain the value function V(s) and action-value function Q(s,a). What's the relationship between them?" Expect candidates to describe V as expected cumulative reward from a state under a policy, and Q as the same but for state-action pairs. Probe: "How would you estimate these in practice?"​

> "Derive or explain the Bellman expectation equation and Bellman optimality equation." This separates deep understanding from surface-level knowledge. You're not demanding a derivation from scratch, but they should explain the recursive structure and why it enables iterative algorithms.

The Bellman optimality equation expresses the value of a state (or state-action pair) under an optimal policy as the maximum expected return achievable from that state. Recursive Structure: Similar to the expectation equation, but now the value is the maximum over all possible actions, reflecting the optimal policy. Iterative Algorithms: This recursive structure enables algorithms like value iteration and Q-learning to iteratively improve value estimates and converge to the optimal policy.

"What's the relationship between the discount factor (γ) and the problem's time horizon?" Strong answers connect γ to planning depth—a candidate should mention that higher γ emphasizes distant rewards, which matters for long-horizon scheduling problems.

"Explain the exploration-exploitation dilemma. How do ε-greedy, UCB, and Thompson Sampling handle it?" Look for understanding of the risk-return trade-off and when each method is appropriate. A strong answer mentions the regret bound or efficiency considerations.​

"Compare model-free and model-based RL. What are the pros and cons?" Model-free (Q-learning, policy gradients) trades sample efficiency for simplicity. Model-based trades computational cost for sample efficiency. For an NP-hard scheduling problem, this trade-off is critical.​

"Explain Q-Learning and SARSA. Why is Q-learning off-policy and SARSA on-policy?" Weaker candidates memorize the algorithm steps; strong ones explain why off-policy learning enables more efficient use of exploration trajectories, and why that matters for sample-constrained environments.

## Part 3 - Specific questions

> "What is reward shaping, and how could you use it to guide learning?" This is especially relevant for your scheduling domain. Strong candidates discuss potential-based reward shaping and intrinsic motivation. Probe: "If your scheduling problem has very sparse rewards (only at episode end), how would you design intermediate rewards without biasing the optimal policy?"​

Small intermediate rewards, e.g. for job completion, making the queue shorter etc. Can make learning much faster, but lead to suboptimal solutions.

> "Describe a scenario where naive reward design fails. What went wrong?" Listen for understanding of reward specification challenges—candidates familiar with real projects often mention unintended behaviors or local optima.

"If you had 100,000 simulator samples to train on, would you choose model-free or model-based? Why?" This forces trade-off reasoning. A strong answer considers the problem's complexity, discount factor, and whether you can afford the computational overhead of planning.

"Mention policy gradients (REINFORCE, Actor-Critic) and compare them to value-based methods." Explain that policy gradients directly optimize the policy, work naturally with continuous action spaces, and have better convergence properties but higher variance. Actor-Critic combines both for reduced variance.

## Part 4 - Practical implementation

State and Action Spaces: "What information would the agent need in its state? How many actions are feasible at each step?" Strong candidates ask about observation dimensionality, temporal dependencies, and whether the state space is fully observable or partially observable.​

Reward Structure: "How would you define success? What are the business metrics?" Push on sparse vs. dense rewards, the impact of delayed feedback, and whether a single reward signal suffices or if multi-task learning is needed.​

Scale and Constraints: "How many facilities? How far into the future must we plan? What's the tolerance for computation time—real-time decisions or batch overnight?" Strong candidates use these constraints to inform algorithm choice immediately.

Feasibility vs. Optimality: "Is finding any feasible schedule valuable, or must we optimize? Is there a gap between feasible and optimal that matters for the business?" This tests understanding of NP-hardness and pragmatism.​

"Given the problem's characteristics, what RL algorithm family would you choose: value-based (Q-learning, DQN), policy-based (policy gradient, PPO), or actor-critic?" Strong candidates reason through: Is the action space discrete or continuous? Do we need off-policy learning for sample efficiency? Is the reward signal sparse?​

For scheduling, discrete action spaces and sparse rewards often favor policy gradients with reward shaping or actor-critic methods over Q-learning.

"Would you use model-based RL? Why or why not?" Model-based has sampling advantages but computational overhead. For an NP-hard problem where the transition dynamics are complex but deterministic (given current state), a learned model might be valuable—or it might be a waste if the real environment is fast to simulate.​

"What about hybrid approaches—using RL with other optimization techniques?" Strong candidates mention combining RL with constraint programming, mixed-integer programming, or local search heuristics. This is crucial for NP-hard problems where pure RL alone may struggle.​

"For NP-hard problems, should we aim for optimal solutions or good-enough heuristic solutions?" The pragmatic answer is context-dependent. Candidates who recognize that RL naturally provides anytime algorithms (can always use the current policy's output) are showing systems thinking.

Training Data: "Where do you get training data? Simulator, offline logs, or active interaction?" For scheduling, simulators are usually available. Strong candidates discuss sample efficiency and data augmentation strategies.​

Feature Engineering and State Representation: "How would you encode the scheduling state efficiently? What features matter most?" This tests domain intuition. For oil rig scheduling: production status, resource availability, demand forecast, equipment state, etc. Can this be represented as a fixed-size vector, or do you need more complex encodings (graphs, attention)?​

Model Training Cadence: "How often would you retrain? Offline nightly batch, or continuous online learning?" Strong candidates discuss distribution shift, model staleness, and business requirements. For scheduling, overnight batch training after a production cycle often makes sense.​

Inference Serving: "What's the latency requirement? Does the system need to recommend schedules in real-time, or is it okay to run overnight?" This drives infrastructure decisions (edge model vs. central service).​

Monitoring and Feedback Loops: "How would you detect if the learned policy is degrading in production? What metrics would you track?" Listen for understanding of reward shift, performance drift, and the importance of logging action-outcome pairs for offline evaluation and retraining.​

"How would you handle the complexity if the problem size is large (e.g., 1000 facilities)?" Options: decompose into subproblems, use hierarchical RL, combine RL with constraint relaxation, or sample-based approximation algorithms

"Would you use a learned heuristic (RL) as part of a broader solver?" For instance, RL could learn a heuristic policy that generates an initial feasible solution, which a local search or branch-and-bound then refines. This hybrid approach is pragmatic and shows systems maturity.

"How would you measure whether the RL solution is better than the current baseline?" They should discuss offline metrics (solution quality, feasibility), online metrics (impact on production KPIs), and A/B testing if applicable.

 "Implement a basic Actor-Critic agent for a discrete action scheduling environment. Assume state is a vector, action is an index, and you have a small simulator. Show me the training loop." They don't need TensorFlow—pseudo-code in Python with NumPy is fine. Look for:​ **Correct Bellman backup**,  Gradient computation (or at least the intent), Exploration strategy, Testing on a toy problem

"Suppose generating a valid schedule (trace) is computationally expensive or rare. How would you balance exploration and exploitation in this setting to avoid wasting samples on invalid or low-quality traces?" (Tests understanding of sample efficiency and smart exploration strategies. Strong answers mention guided exploration, curriculum learning, or injecting prior knowledge.)​

"How would you design an exploration strategy that leverages heuristics (e.g., greedy rules, local search) to guide the agent toward promising regions of the state-action space, while still allowing for the discovery of novel solutions?" (Probes hybrid approaches: combining heuristics with stochastic exploration. Look for candidates who mention epsilon-greedy with heuristic bias, or UCB with heuristic priors.)​

"If you had access to a set of pre-existing (suboptimal) schedules, how would you use them to warm-start your RL agent or pre-train its policy/value network? What risks might this introduce?" (Tests awareness of warm-starting and pre-training. Strong answers discuss imitation learning, behavioral cloning, or initializing with heuristic policies, and mention the risk of policy collapse or overfitting to suboptimal traces.)​

"In what ways could you use offline data (e.g., historical schedules) to improve the sample efficiency of your RL agent, especially if online interaction is limited or costly?" (Probes understanding of offline RL, batch RL, or hybrid offline-online learning. Listen for ideas like reward shaping, dataset aggregation, or offline policy evaluation.)​

"How would you design your reward function to encourage the agent to discover valid schedules early in training, while still incentivizing long-term optimality? Could you use intrinsic rewards or curriculum learning?" (Tests creativity in reward design and curriculum strategies. Strong answers mention shaping rewards for feasibility, or gradually increasing problem complexity.)​

"What are the trade-offs between using a completely random initialization for the policy and starting from a heuristic-based initialization (e.g., a rule-based scheduler)?" (Probes understanding of initialization strategies. Strong answers discuss faster convergence vs. risk of local optima, and the importance of escaping from heuristic biases.)​

"How would you handle the case where the environment is partially observable or the simulator occasionally produces invalid or infeasible traces? Could you adapt your exploration strategy or use robust RL methods?" (Tests robustness and adaptability. Look for answers that mention safe RL, constraint satisfaction, or filtering invalid traces.)​

"If you wanted to combine RL with constraint programming or other optimization techniques to generate traces, how would you structure this hybrid approach, and what role would RL play?" (Probes systems thinking and integration with other methods. Strong answers mention RL as a meta-heuristic, or using RL to guide search in a larger optimization framework.)​
