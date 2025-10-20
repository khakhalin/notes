# Reinforcement learning

See also: [[credit]] for RL in the brain, [[modeling]]

#rl #bib

Papers
* [[Baker2019autocurricula]] - RL agents by OpenAI playing hide-and-seek, with pretty videos

# Top reading

🔥 Barto, A. G. (2021). Reinforcement learning: An introduction. by richard’s sutton. _SIAM Rev_, _6_(2), 423.
 - simpler book, a good start

🔥 Spinning Up in Deep RL: https://spinningup.openai.com/en/latest/
A deeper and more practical introduction to the subject

🔥 CleanRL library - practical examples

After 2017 PPO is the commonly accepted SOTA for RL:
https://en.wikipedia.org/wiki/Proximal_policy_optimization

Exploration in RL (a sort of blog-review with math, 2020)
https://lilianweng.github.io/lil-log/2020/06/07/exploration-strategies-in-deep-reinforcement-learning.html)

Nice collection of advice on reinforcement learning (non-mathy, applicable to almost any experimental ML): https://rll.berkeley.edu/deeprlcourse/docs/nuts-and-bolts.pdf
(by John Schulman, 2016)

# Possible reading

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
Some sort of mix between RL, learning by imitation, and trying to infer better policies from observed examples. Kinda like self-supervised, in a sense of introducing an inner supervized loop that feeds from the outer RL loop. The intro sounds as if they were claiming that learning bad policies together with the rewards they yield can be helpful for extrapolating to better policies? Not sure.

Curriculum for Reinforcement Learning. Lilian Weng. Jan 29 2020.
https://lilianweng.github.io/lil-log/2020/01/29/curriculum-for-reinforcement-learning.html

Alexander Tschantz, Beren Millidge, Anil K. Seth, Christopher L. Buckley (2020). Reinforcement Learning through Active Inference
https://arxiv.org/abs/2002.12636
Instead of maximizing rewards, they maximize evidence for an internal generative model. Also use _free energy of the expected future_. Hmm. [[freeenergy]]? Looks quite mathy.

# Robots

Ibarz, J., Tan, J., Finn, C., Kalakrishnan, M., Pastor, P., & Levine, S. (2021). How to train your robot with deep reinforcement learning: lessons we have learned. The International Journal of Robotics Research, 0278364920987859.
https://arxiv.org/abs/2102.02915

Sharma, A., Ahn, M., Levine, S., Kumar, V., Hausman, K., & Gu, S. (2020). Emergent Real-World Robotic Skills via Unsupervised Off-Policy Reinforcement Learning. arXiv preprint arXiv:2004.12974.
https://arxiv.org/abs/2004.12974
Sharma, A., Gu, S., Levine, S., Kumar, V., & Hausman, K. (2019). Dynamics-aware unsupervised discovery of skills. arXiv preprint arXiv:1907.01657.
https://arxiv.org/abs/1907.01657
A sequence of two papers that explore unsupervised exploration of possible behaviors in a virtual environment. SOTA?
Also this write-up:
https://ai.googleblog.com/2020/05/dads-unsupervised-reinforcement.html

# Other links

* [Minigrid](https://github.com/maximecb/gym-minigrid/) - minimalistic RL environment
