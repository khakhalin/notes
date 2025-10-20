# Embedding layers

Parents: [[unsupervised]], [[features]] (all dim reduction is there)

See also:
* [[word2vec]] - famous embedding of words from Google (2013)
* [[glove]] - embedding of words from Stanford (2014)
* [[triplet_loss]] - a type of negative sampling
* [[graph_embedding]] - random walks, node2vec

#embedding #features


# Embedding layers

Embedding layers solve the problem of data sparsity for [[dl]], so you don't have to train directly on long one-hot encoded vectors. Empirical rule of thumb: dimensionaltiy N dim ~ (n possible words)^(1/4). (🔥 no idea where this formula is coming from) With too few dimensions you won't capture the complexity; with too many - you overfit, and training takes longer. N dimensions can be treated as a hyperparameter, and optimized.

One possible approach for an unsupervised pre-training: get hold of some similarity or co-occurance measure; use it to produce groups, then "withhold some information", predicting missing members of a group based on those that are visible.

# Refs

A [[python]] package (Embetter) that aims to unite several popular (?) text and image embeddings in one interface.
https://koaning.github.io/embetter/