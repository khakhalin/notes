# Embedding layers

#embedding #features

Parents: [[08_Unsupervised]], [[04_Features]] (all dim reduction is there)

See also:
* [[word2vec]] - famous embedding of words from Google
* [[triplet_loss]] - a type of negative sampling
* [[graph_embedding]] - random walks, node2vec

# Embedding layer

In DL ([[06_DL]]), solves the problem of data sparsity (to avoid direct training on long one-hot encoded vectors). A bottleneck layer from an N-hot layer to learn an embedding. Number of units in the bottleneck = number dimensions in the enbedding. Empirical rule of thumb: N dim ~ (n possible values)^(1/4). #todo:  _Where does this formula come from?_ With too few dimensions you won't capture the complexity; with too many - risk of overfitting, and longer to train. N dimensions should be treated as a hyperparameter, and optimized.

One possible approach: get hold of some similarity or co-occurance measure; use it to produce groups, then "withhold some information", predicting missing members of a group based on those that are visible.
> Where does it come from? Does it really work? Is it like a "noisy student" approach? ([[Xie2020noisy_student]]) Is it a modern technique, an experimental one, or an older one?

> Is it where autoencoders ([[vae]]) can be helpful, for pre-training the embedding? Or is it not a thing now, and everything is trained as one giant network?