# Self-Supervised Learning

#bib #self-supervised #embedding #representation

Subtopics:
* [[vae]] - autoencoders
* [[gan]] - GANs :)

Related:
* [[triplet_loss]] - aka contrastive loss or contrastive learning, aka **negative sampling** (which is also a more popular term)
* [[word2vec]] - an example from text

Papers:
* [[Weng2019self-supervised.md]] - a review of sorts
* [[Yu2020lookback]] - graph networks and label propagation for few-shots learning, using embedding from both late and early layers.
* [[Zoph2020pretraining]] - pre-training on images hurts more often than it helps	

# Embedding layers

Solves the problem of data sparsity (to avoid direct training on long one-hot encoded vectors). A bottleneck layer from an N-hot layer to learn an embedding. Number of units in the bottleneck = number dimensions in the enbedding. Empirical rule of thumb: N dim ~ (n possible values)^(1/4). _Where does it come from?_ With too few dimensions you won't capture the complexity; with too many - risk of overfitting, and longer to train. N dimensions should be treated as a hyperparameter, and optimized.

Classical training: get hold of some similarity or co-occurance measure; use to produce groups, then "withhold some", predicting missing members of a group based on those that are visible..