# Self-Supervised Learning

#self-supervised #bib

Subtopics:
* [[vae]]
* [[gan]]

Related:
* [[triplet_loss]] - aka contrastive loss or contrastive learning, aka **negative sampling** (which is also a more popular term)

# Embedding layers
Solves the problem of data sparsity (to avoid direct training on long one-hot encoded vectors). A bottleneck layer from an N-hot layer to learn an embedding. Number of units in the bottleneck = number dimensions in the enbedding. Empirical rule of thumb: n dim ~ (n possible values)^(1/4). _Where does it come from?_ With too few dimensions you won't capture the complexity; with too many - risk of overfitting, and longer to train. N dimensions should be treated as a hyperparameter, and optimized.

Classical training: get hold of some similarity or co-occurance measure; use to produce groups, then "withhold some", predicting missing members of a group based on those that are visible. But look also: [[Weng2019self-supervised.md]]

Examples:
* [[word2vec]]