# Back to Basics: Benchmarking Canonical Evolution Strategies for Playing Atari

Chrabaszcz, P., Loshchilov, I., & Hutter, F. (2018). Back to basics: Benchmarking canonical evolution strategies for playing atari. arXiv preprint arXiv:1802.08842.
https://arxiv.org/abs/1802.08842

#game #evolution #atari #hack

Coverage:
* https://twitter.com/Miles_Brundage/status/968322848724004864
* https://www.theverge.com/tldr/2018/2/28/17062338/ai-agent-atari-q-bert-cracked-bug-cheat

One interesting finding in the paper (that put it on everyone's radar) is that found a hack (like a bug: an unintended behavior) that allowed it to aquire very high scores very quickly. The Verge article above has a Youtube video.

They used a canonical Evolutionary Strategy (ES) algorithm. The architecture: Pre-process of the screen (from **OpenAI Gym**) to reduce flickering → grayscale → recise and crop to 84x84 → stack together last 4 frames to get a tensor. To speed up, only consider every 4th or 3d tensor. Then use **Evolutionary Strategy** algorithm for **Policy networks** (following Salimans 2017). At each iteration, consider a population of networks (~800 of them), centered around the current best policy θ, but noisified using **mirrored sampling** (Brockhoff 2010). Mirrored sampling refers to trying each noise vector twice, with opposite signs, as policy ± noise instead of just policy + noise. Evaluate results for each policy, rank them (apparently it's better than looking at actual scores, as it helps to avoid local minima). From these ranks, approximate the gradient fr the "centroid" policy θ, then use ADAM with weight decay to move the centroid towads the minimum.

**The network: **

All transitions use batch norm and ELU activation:
* State (84×84×4) →
* 8×8×32 conv2d with stride 4
* 4×4×64 conv2d stride 2
* 3×3×64 conv2d stride 1
* 3136×512 dense
* 512×n dense
* n outputs (5 "buttons" that can be pressed?)

Training budget: 10 hours on 400 CPUs

# Key Refs

Dimo Brockhoff, Anne Auger, Nikolaus
Hansen, Dirk V Arnold, and Tim Hohm. Mirrored sampling
and sequential selection for evolution strategies. In International
Conference on Parallel Problem Solving from Nature, pages 11–21.
Springer, 2010.

Tim Salimans, Jonathan Ho, Xi Chen, and
Ilya Sutskever. Evolution strategies as a scalable alternative to
reinforcement learning. arXiv preprint arXiv:1703.03864, 2017.