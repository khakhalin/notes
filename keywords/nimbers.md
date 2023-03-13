# Nimbers, Nim Game, Grundy numbers

#game

Parents: [[algos]]
See also: [[boardgame]]

# Nim game

N piles of stones, each witn p_n stones. Two players take turns, taking 0 < s < current pile size stones from some pile. The goal is either to be the last to take a stone (direct, or **normal** game), or not to be the last who takes the last stone (reversed, or **misère** game). **Winning state**: if for a given state there's a winning strategy for it (current player is guaranteed to win if they play correctly).

# Nimbers

**Nim-values** (or **Nimbers**): a number G to classify each situation. Should be additive in respect to piles, but not in a normal arithmetic way (⊕, another type of addition). G=0 corresponds to a losing game state; G>0 means winning state. As combining piles is distributive and commutative, ⊕ also has to be. Also A⊕0 = A (no change).

Obvious example:
* One pile direct play: p=0 (no stones) means losing; any other p>0 means winning.

A more interesting property: A⊕A = 0 (duplicating all piles makes game unwinnable). Proof: it's like playing two games in parallel. If your opponent always mirrors you, they will always finish second, and take the last stone, thus winning the game. ⇒ A⁻¹ = A, which means that we have a groups isomorphic to a group of binary rows with bitwise XOR as an operation. Therefore we can define nimbers as numerical values of these binary sequences, and use bitwise XOR for ⊕. Note that this statement doesn't depend on whether we play a direct or reversed game.

If G=0 (losing state), then any legal move changes it to a winning state for the oppoinent, and so any state achievable from G=0 with a legal move has G>0 (winning state). Conversely, If G>0 (there's a winning strategy), then ∃ turn that would make it a G=0 state: that would be the turn that belongs to a winning strategy (or one of them), as the state achieved via this step would be no-win, and thus G=0.

As far as nimbers go, as they are additive, every legal move (taking s stones from pile i) can be represented as removing p stones from pile i, and then adding (p-s) stones to pile i. Moreover, as A⁻¹=A, removing all p_i stones from a pile is equivalent to adding another pile of p_i stones. Therefore A' = A⊕(p_i)⊕(p_i - s). Where, again, ⊕ is an elementwhise XOR on binary, and all these values are some sorts of numbers.

# Variations and Grundy numbers

In a more general game, states form a directed graph. All terminal nodes are loss states (either no stones, or a number of stones that cannot be taken because of some restrictions). Assume no cycles (cycles are either useless if you're winning, or you can stall forever if you're losing, so they are always prohibited by the rules anyways). **Nimbers** (or **Grundy numbers**) for this game are a generalization of Nim-numbers. For terminal states are defined as 0; for all other states, as **minimal excludant** (mex) of nimbers of states achievable from this state: $\text{mex}(S) = \min(i \in ℕ, i \notin S)$ (where 0 ∈ ℕ of course).

So all those that lead to terminal states only have G=1. Those who lead to terminal states (G=0) and G=1 only (and maybe some high-ranking, that's ok) are G=2 etc. 

G=1 is a win state (you can go to G=0 and leave it to your opponent). G=2 is a win state as well (as G=0 is among its kids), as does any G>0. All G=0 are a loss state, because from them is impossible to get to another G=0 (coz mex), which means that any state achievable from them is G>0, and from any G>0 is possible to go to G=0. So "strategy-optimal" (for your opponent) evolution from G=0 is made of a alternating sequence of G=0 and G>0 that sooner or later will reach a terminal G=0 (as game is finite), yet because of alternation it will happen on your turn, and you'll lose.

# Refs

Good explanation that explains the proofs:
https://codeforces.com/blog/entry/66040?f0a28=1

A bunch of circularly referential (and thus not too helpful) Wikipedia pages:
* https://en.wikipedia.org/wiki/Nim
* https://en.wikipedia.org/wiki/Genus_theory
* https://en.wikipedia.org/wiki/Nimber

Some code that I found quite unhelpful, but linking here for completeness sake:
https://www.geeksforgeeks.org/combinatorial-game-theory-set-3-grundy-numbersnimbers-and-mex/