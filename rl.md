# Reinforcement learning

Parents: [[dl]]
See also: [[credit]] (rl-like thing in the brain), [[modeling]]

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

## RL methdos

* Three most common ones:
    * [[ppo]] - the most common method as of 2025.
    * DQN - discrete actions.
    * SAC (Soft Actor-Critic) - continuous control, robotics
* [[reinforce]] - aka Policy Gradient Methods, simplified methods that don't rely on V or Q
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

# Top reading

Reinforcement learning: An introduction. Richard Sutton. 2021. SIAM Rev, 6(2), 423. - simpler book, a good start

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

# Random links reading

Curriculum for Reinforcement Learning. Lilian Weng. Jan 29 2020.
https://lilianweng.github.io/lil-log/2020/01/29/curriculum-for-reinforcement-learning.html

Matusch, B., Ba, J., & Hafner, D. (2020). Evaluating Agents without Rewards. arXiv preprint arXiv:2012.11538.
https://arxiv.org/pdf/2012.11538.pdf
Exploration, curiosity, self-driven learning (including in Minecraft)

Reinforcement Learning for Language Models. Yoav Goldberg, April 2023.
https://gist.github.com/yoavg/6bff0fecd65950898eba1bb321cfbd81

Simple Reinforcement Learning with Tensorflow Part 0: Q-Learning with Tables and Neural Networks. Arthur Juliani
https://medium.com/emergent-future/simple-reinforcement-learning-with-tensorflow-part-0-q-learning-with-tables-and-neural-networks-d195264329d0

The present in terms of the future: Successor representations in Reinforcement learning
Arthur Juliani
https://medium.com/@awjuliani/the-present-in-terms-of-the-future-successor-representations-in-reinforcement-learning-316b78c5fa3

Mnih, V., Kavukcuoglu, K., Silver, D., Rusu, A. A., Veness, J., Bellemare, M. G., ... & Petersen, S. (2015). Human-level control through deep reinforcement learning. Nature, 518(7540), 529-533.
https://daiwk.github.io/assets/dqn.pdf
A popular RL paper about Atari games. Other papers (PCGRL: Khalifa 2020) reference it as "fractal networks", even though the term isn't used in this paper itself.

Reward-Conditioned Policies. Aviral Kumar, Xue Bin Peng, Sergey Levine. 2019.
https://arxiv.org/abs/1912.13465
Some sort of mix between RL, learning by imitation, and trying to infer better policies from observed examples. Kinda like self-supervised, in a sense of introducing an inner supervised loop that feeds from the outer RL loop. The intro sounds as if they were claiming that learning bad policies together with the rewards they yield can be helpful for extrapolating to better policies? Not sure.

Alexander Tschantz, Beren Millidge, Anil K. Seth, Christopher L. Buckley (2020). Reinforcement Learning through Active Inference
https://arxiv.org/abs/2002.12636
Instead of maximizing rewards, they maximize evidence for an internal generative model. Also use _free energy of the expected future_. Hmm. [[freeenergy]]? Looks quite mathy.

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
