# Fast deck recommendation system for collectible card games

Chen, Z., Amato, C., Nguyen, T. H. D., Cooper, S., Sun, Y., & El-Nasr, M. S. (2018, August). Q-deckrec: A fast deck recommendation system for collectible card games. In 2018 IEEE Conference on Computational Intelligence and Games (CIG) (pp. 1-8). IEEE.
http://web.cs.ucla.edu/~yzsun/papers/2018_cig_deckrec.pdf

#game

Parent: [[boardgame]]

Intro: they describe how games work, and some very broad archetypes, as examples (aggro and not-aggro). Try to pitch that deck-building is important for game design (not too convincingly). **Combinatorial optimization problem**. Cite a big number (10^25). Previous algorithms: heursitic (popularity and in-game resources) and meta-heuristic (some type of iterative optimization, where deck quality is assessed via simulated matches). Claim that it is expensive (not that it is currently impossible). Instead, they are saying, let's pick cards one by one (even though they are NOT explicitly modeling a draft situation), just because computationally picking a card is easier than optimizing the full set. So their player always has acess to all cards, and just optimize cards one by one.

Use a fake (I think?) Heartstone (as it's simpler). Cite some **Cross-entropy method** as a way to optmize combinatorial tasks, as alternative to genetic algorithms ([[gen_alg]]). Genetic algorithms are slow (they claim - days of optimization for one deck). They are trying to frame it as a lerning problem, using **Markov Decision Process** (see [[mdp]]) **Reinforcement Learning** (aka [[q-learning]]). Give some examples of how this can also apply to Shuttle launches scheduling :)

What they actually do: they still rely on a game-playing AI, but somehow divide different AI games into different game-strategies (e.g. aggressive, conservative: in this paper they actually only use 2 strategies, claiming that that's enough to illustrate the point). It seems that gaming strategies are actually prescribed (not derived from cards), so in a way they work with adjusting cards for the strategy, not the other way around. From these virtual games they collect info about which cards work well for each of the strategies.

Then they take a random deck, and start replacing cards one by one, to a "better card" for a given strategy. Their reward function (to train the Markov Decision Process) is a sum of win-rate ratios after each deck improvement, each win-ratio exponentially amplified (the formula looks messy, but in the text they claim to just take an exp() of win-ratio).

Instead of a look-up value for policy Q(s,a) for every state and action, they encode them as a 2-layer FF network (that they call Multi-Layer-Perceptron for some reason). 2N+1 inputs (why?) → hidden layer (1000 ReLUs) → one output (linear), to give the Q value, that is later argmax-ed to find the best policy. And they use gradient descent to train this network.

Show that their method is an order of magnitude faster than a genetic algorithm (that they implemented with open-source DEAP library), and reaches higher win rates **against random decks** (their main test, and objective).

# Refs

Cross-entropy method:
Rubinstein, R. (1999). The cross-entropy method for combinatorial and continuous optimization. Methodology and computing in applied probability, 1(2), 127-190.
800 citations, but no free pdf online.

Genetic algorithm to build decks (refs 13-14-15):
* J. H. Holland, Adaptation in natural and artificial systems: an introductory analysis with applications to biology, control, and artificial intelligence. MIT press, 1992.
* P. Garc´ıa-Sanchez, A. Tonda, G. Squillero, A. Mora, and J. J. Merelo, “Evolutionary deckbuilding in hearthstone,” in Computational Intelligence and Games (CIG), 2016 IEEE Conference on. IEEE, 2016, pp. 1–8.
* S. J. Bjørke and K. A. Fludal, “Deckbuilding in magic: The gathering using a genetic algorithm,” Master’s thesis, NTNU, 2017.