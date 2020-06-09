# Alpha-Beta Pruning

#algo #game

See also:
* [[boardgame]] - AI in gaming
* [[beam_search]] - similar concept, but for text prediction
* [[Weidman2018alphago]] - on combining this with ANNs, to estimate the quality

A way to find best solution while looking forward in games. Improvement over **naive minimax**. With naive minimax, you look through the full tree of possible future moves, and pick a move that maximizes the minimal value of future possible position, assuming perfect play by the opponent. That is, if for any moves we make, the opponent plays optimally thus minimizing our gain, which of our moves would give us the best final outcome? Giving the adversity, which move would be best for us? ( [ref](https://en.wikipedia.org/wiki/Minimax#Minimax_algorithm_with_alternate_moves) ) This should, of course, rely on some heuristics, to estimate how good each state is, for eventually winning the game. The terminology is to call "us" the **maximizing player**, and them the **minimizing player**. The number of turns by either player that the algorithm is looking ahead is for some reason called **plies** (singular: ply). One full round (a "move") in a two-player game is typically worth 2 plies.

The idea of αβ-pruning is to not explore hopeless branches, and instead explore promising ones. α is a value of minimal guaranteed score that we (maximizing player) can achieve through our actions, and is init at -Inf (as if we were hopeless). β is a lowest score that our opponent (minimizing player) can bring us down (is if originally they were hopeless as well). 

As we go through the tree, we want to bring α up, while our fake opponent is trying to bring β down. Each node keeps track of αβ for the tree that starts from this node. The α for a node is a max of all αs achieved in its subtree (the best we can get if we follow this branch). The β for a node is a min of all βs in the tree (the lowest score they can inflicet upon us).

If a branch offers our opponent a better strategy than we can ever achieve (β<α), then there's no need to explore this branch, and it can be pruned.

# References

* https://en.wikipedia.org/wiki/Alpha%E2%80%93beta_pruning - good pseudocode, but bad explanation (it is not quite clear from the text what β means exactly)
* http://web.cs.ucla.edu/~rosen/161/notes/alphabeta.html - a better explanation