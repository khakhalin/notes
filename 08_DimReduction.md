# Dimensionality reduction

## Subspace Methods
General idea: define subspaces and assign vectors to classes to minimize the distance between these vectors and these subspaces. Usually distance = acos cosine similarity = acos (v∙proj(v))/v^2 = acos vPv/v^2, where P is a projection matrix. PCA is one way to achieve it, but not the only one. But in all cases, the idea is that (Signals) = (Basis)∙(Scores), where Scores can be interpreted as representations.

A zoo of possible approaches:
**PCA** (principal component anlaysis) - optimal reconstruction (minimal reconstruction error, given a limit on basis size), maximal variance of projections. Eigenspace of XX' ( #todo : why again?)

**LDA** (Linear Discriminant Analysis) - optimal separation of classes. At each step, maximize separation (distance) between classes, aka **Fisher Linear Discriminance**. Obviously, label-driven, and these labels are nominal.
Links: [wiki](https://en.wikipedia.org/wiki/Linear_discriminant_analysis)

**CCA** (Canonical Correlation Analysis) - optimal correlation with a given numerical value (similar to labels, but in a continuous case). More generally, for two high-D datasets X and Y, finds directions (projections to 1D) in both X and Y such that the correlation between these projections is maximal. In even more general case, we can develop a sequence of these correspondences, or an "optimal rotation" from one space to another.
Links: [wiki](https://en.wikipedia.org/wiki/Canonical_correlation)

**ICA** (Independent Component Analysis) - makes the basis statistically independent. Can be considered an extension of PCA that goes beyound 2nd order statistics.

**NMF** (Non-negative Matrix Factorization) - keeps all elements in the basis non-negative, which often really aids interpretability.

**Kernel methods** - non-linear extension (Kernel tricks?)

Refs:
* [Some lecture with a list of approaches](https://www.cc.gatech.edu/~hic/8803-Fall-09/slides/SubSpace-Learning.pdf)	 - by Horst Bischof and Ales Leonardis?

## tSNE
"The art of using tSNE" from Nature:
[https://www.nature.com/articles/s41467-019-13056-x](<https://www.nature.com/articles/s41467-019-13056-x>)

## UMAP
ToRead:
* [https://pair-code.github.io/understanding-umap/](<https://pair-code.github.io/understanding-umap/>)
* [https://towardsdatascience.com/how-exactly-umap-works-13e3040e1668](<https://towardsdatascience.com/how-exactly-umap-works-13e3040e1668>)
* [https://towardsdatascience.com/how-to-program-umap-from-scratch-e6eff67f55fe](<https://towardsdatascience.com/how-to-program-umap-from-scratch-e6eff67f55fe>)

# Clustering

# Very High Dimensions
For high dimensions (say, word counts in a document) simple Eucledian distance doesn't work that well, as it becomes impossible to compare long documents to short documents. One way would be to normalize vectors before calculating Eucledian distance, but there's an easier way: use **cosine similarity** (described above). It sorta auto-normalizes lengths, is easier to calculate, and ordering of proximity is the same for normalized Eucledian and cosine similarity. (_unproved here, but apparently it's the case, hmm_)

**Specifics of working with very high dimensions:**
(Genomic data in this case)
* https://towardsdatascience.com/no-true-effects-in-high-dimensions-1f56360182cd
* https://towardsdatascience.com/pitfalls-of-data-normalization-bf05d65f1f4c
* https://towardsdatascience.com/how-to-cluster-in-high-dimensions-4ef693bacc6