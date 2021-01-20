# Embedding layers

#embedding #features

Parents: [[08_Unsupervised]], [[04_Features]]

See also:
* [[word2vec]]
* [[triplet_loss]] - a type of negative sampling
* [[graph_embedding]] - random walks, node2vec

**Embedding layer** solves the problem of data sparsity (to avoid direct training on long one-hot encoded vectors). A bottleneck layer from an N-hot layer to learn an embedding. Number of units in the bottleneck = number dimensions in the enbedding. Empirical rule of thumb: N dim ~ (n possible values)^(1/4). #todo:  _Where does this formula come from?_ With too few dimensions you won't capture the complexity; with too many - risk of overfitting, and longer to train. N dimensions should be treated as a hyperparameter, and optimized.

Classical training: get hold of some similarity or co-occurance measure; use it to produce groups, then "withhold some", predicting missing members of a group based on those that are visible.