# Emergent Tool Use from Multi-Agent Interaction

Baker, B., Kanitscheider, I., Markov, T., Wu, Y., Powell, G., McGrew, B., & Mordatch, I. (2019). Emergent tool use from multi-agent autocurricula. arXiv preprint arXiv:1909.07528.
https://arxiv.org/abs/1909.07528

Also this blog summary with illustrations and videos.
[https://openai.com/blog/emergent-tool-use/](<https://openai.com/blog/emergent-tool-use/>)

#agents #curriculum #game #self-supervised #rl


See also: [[11_RL]], [[08_Unsupervised]], [[curriculum]]

That OpenAI hide-and-seek study. Agents progressively built 6 distinct strategies and counter-strategies (some of them glitch-based).

Hiders are rewarded for not being seen, seeekers - for seeing them (and opposite punishments). "Seeing" objects is directional, but there's also "sensing objects" and distance to them that is lidar-like (all directions, but with occlusion). Also a prep phase, and a penalty for going far from the "main arena". No direct reward for interaction with objects etc.

Emereging **strategies** create an **autocurriculum**: a game-like dynamics. Each next behavior negates the previous one (random > chasing > door blocking > ramp use > ramp defense). Coordination and coopearation emerges, including who will grab which object, and passing objects. Even more strategies in a more randomized environment, including "shelter construction", and finally glitch-based "box surfing" and a counter-locking it. This also counts as **tool use** (and they have some more refs for it).

Training infrastructure an algorithms were described earlier:
* [OpenAI Five](https://openai.com/blog/openai-five/)
* [Dactyl](https://openai.com/blog/learning-dexterity)

The compute is huge of course (described on p7)

"Each object is embedded and then passed through a masked residual self #attention block, similar to those used in transformers, where the attention is over objects instead of over time. Objects that are not in line-of-sight and in front of the agent are masked out such that the agent has no information of them."

> What does it mean? It feels that I don't understand attention well enough to even understand this sentence.

Policies are trained using [Proximal Policy Optimization](https://openai.com/blog/openai-baselines-ppo/)

They also compared a direct game with a game in which agents had "intrinsic #motivation" to visit as many states as possible (esp. infrequently visited states). It appears that states were expliticly coded (like, the position of each box etc.) It didn't work (less efficient, and looks weird: less purposeful). I'd say however that their implementation of motivation seems botched (overparameterized, lack of generalization and pruning).

**Evaluation** of different policies: mention "Metrics like ELO (Elo, 1978) or Trueskill (Herbrich et al., 2007)" (?), but then also say that they don't really work.

Then tested #transfer on novel tasks: count objects, lock and return, sequiential lock, blueprint construction, sheltere construction. For most, agents pre-trained in "Hide-and-Seek" had  a leg (but not for shelter construction, surprisingly!!) They also compared it with pre-training on a much simpler (I think?) policy (explicitly motivated counting of objects), and it performed similarly, or even better. So this wasn't that much of a success, which is actually cool.