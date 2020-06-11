# Evolutionary Approach to Collectible Card Game Arena Deckbuilding using Active Genes

Kowalski, J., & Miernik, R. (2020). Evolutionary Approach to Collectible Card Game Arena Deckbuilding using Active Genes. arXiv preprint arXiv:2001.01326.
https://arxiv.org/abs/2001.01326v2

#game #evolution #gen_alg

Parents: [[gen_alg]], [[boardgame]]

They use some self-invented "programming game" **Legends of Code and Magic** (Kowalski 2018), inspired by MTG and Heartstone, specifically designed to be AI research -friendly, and further simplified since 2018. Their "Arena" mode is similar to MTG drafting: they have a set of cards (160), and players take turns picking one card from three that are offered to them. Players don't know the choices of of the other player.

To optimize the gentic algorithm, they introduce a concept of a **gene**, which seems to be a subset of cards that travel together.

Interesting paragraph about how if you solve the task of drafting, you also solve the task of game balancing. _Ryan's Cube Predictor would be another example here._

Ah, they don't even look at synergies! They just try to learn which card is better! (Page 3, left column, middle). _I stopped reading at this point, as it makes this paper not quite relevant for our case.

# Refs

J. Kowalski and R. Miernik, “Legends of Code and Magic,” http://legendsofcodeandmagic.com, 2018.