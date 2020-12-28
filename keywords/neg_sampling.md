# Negative Sampling

#embedding

Parents: [[04_Features]]

The idea is to avoid sampling trough all possible pairs of elements, but sample only through all positives, and some random negatives. Critically important for sparse sets, where most elements aren't engaging with each other.

Subtypes, uses, implementations, and alt-names:
* [[word2vec]] - perhaps the most famous use of negative sampling
* [[umap]] - uses negative sampling for graph embedding
* [[TransE]] - knowledge graphs and **contrastive loss**
* [[triplet_loss]] - **triplet loss** on images, for face recognition