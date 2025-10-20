# Metrics

Parents: [[features]], [[clustering]]
Related: [[typical_sample]], [[curse_dim]]


# Metric Zoo

## Minskovsky-like

**euclidean**

**manhattan**

**chebyshev**
**minkowski**

## Other spatial
canberra
braycurtis
haversine

## Normalized spatial
mahalanobis
wminkowski
seuclidean

## Angular

**cosine**
For very high dimensional data (say, word counts in a document) simple Euclidean distance doesn't work that well, as it becomes impossible to compare long documents to short documents. One way would be to normalize vectors before calculating Eucledian distance, but there's an easier way: use **cosine similarity**. It sorta auto-normalizes lengths, is easier to calculate, and ordering of proximity is the same as for normalized Eucledian.

correlation

# Binary data
hamming
jaccard
dice
russelrao
kulsinski
ll_dirichlet
hellinger
rogerstanimoto
sokalmichener
sokalsneath
yule

# Refs

Origin of this list:
https://umap-learn.readthedocs.io/en/latest/parameters.html