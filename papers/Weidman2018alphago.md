# The 3 Tricks That Made AlphaGo Zero Work

by Seth Weidman, January 1st 2018
https://hackernoon.com/the-3-tricks-that-made-alphago-zero-work-f3d47b6686ef

#blog #rl #game

Parents: [[boardgame]]

A summary of how **AlphaGo** works

Actually surprisingly similar to how humans supposedly play: a combination of algorithmic lookahead (a tree of possible decisions) and ANN-based "untuition" (two, actually). They showed that this combo is allways better than straight network.

**Inputs**: current position + whose turn + last few turns (to avoid illegal moves by repetition). 

**Outputs**: 1) which moves to make (aka **policy**), 2) how likely is the current player to win (aka **value**)

For AlphaGo they trained policy on human games (30 million moves), but for AlphaGo Zero they did everything through self-play.

This second output (value) allowed algorythmic exploration (Monte Carlo Tree Search) using ANN assessment of each game state as a heuristics. So for every position, it new now only its ANN estimation of how good it is, but also whether it is likely to lead to even better positions.

## Architecture
* 20 layers
* **two heads** to produce two outputs (policy and value). Trained simultaneously (body + head 1, then body + head 2), so othey shared a representation, but produced different outputs. 
* Layers are **residual**: it's like convolutional (mostly 3x3 by 128 deep, then some 3x3x256, finished with 3x3x512). Residual are strange: x→ W1→ relu()→ W2→ +x (the original x, added back at the end, for real). See He 2015.
* Average pool at the very end.

## Training
1. Alternate between self-play (on current level), and training. 
2. Self-play: 1600 explorations into the future (MCTS) per turn. 
3. Each cycle: 500 000 games, 2048 randomly chosen positions + their assessments at the moment based on MCTS + whether the game was actually won. 
4. Use it for training. 
5. Once the trained network beats the previousn network in 55% of cases, switch to it for traiing.

# See also

* [[Swiechowski2018metagame]] - similar tree search, but for HeartStone

# Refs

On multi-headed (multitask) learning:
https://ruder.io/multi-task/index.html#introduction

On residual networks:
Deep Residual Learning for Image Recognition, 2015, from Microsoft Research
https://arxiv.org/pdf/1512.03385.pdf