# UMAP: Uniform Manifold Approximation and Projection

#clustering #dim #features

Parents: [[04_Features]], [[algebraic_topology]]
Related: [[curse_dim]], [[tsne]], [[neg_sampling]]


**Simplicial complexes**: a way to replace continuous geometry (hard) with simple combinatorial algebra; a much more formulaic way to describe structures, which yields power! (See [[algebraic_topology]])

**Čech cohomology** or **Čech complex** - a way to represent a bunch of sets with a siimplicial complex. If you have a bunch of sets, represent each with a node. Connect 2 nodes if they intersect (crucially, not necessarily on a node; if they intersect anywhere in the space); connect 3 nodes with a face if all 3 sets have an intersection etc. How can it help us to represent the data? Surround each data point with a neighborhood area, then look at which neighborhoods had intersected. This operation would turn the dataset into a simplicial complex (but not necessarily connected at this point).

**Vietoris–Rips complex** - a relaxed generalization of Cech complex. A set of nodes S is made a simplex (of an appropriate dimension, matching the size of S) iif the distance between any two nodes in S is smaller than a certain threshold δ. So it's like the Chech complex, but looking only at intersecting neighborhoods on our data (neighborhoods including nodes); not just intersecting "somewhere in the void". Easier to build; easier to work with; doesn't depend on the actual embedding of the dataset in a metric space; it's enough that a local metric (node-to-node distances) is available.

UMAP works by finding a low-D embedding that has the same topological representation as the original data. Apparently, this original reliance on simplicial complexes, and not just on a neighborhood graph, is what makes UMAP so powerful, and with theoretical guarantees. Even though in practice, we usually work with graphs (0 and 1 simplices), and not higher structures.

But how to find a good δ, and how to make sure our data doesn't break into a bunch of unconnected points? Let's **expand each neighborhood** until it covers at least **k neighboring points**. This k is a parameter of the method. It does not guarantee that the manifold as a whole is connected (it can still consist of several unconnected pieces), just that no single point will be unconnected.

There's a fast algorithm for that (and UMAP uses it) called the **Nearest Neighbor Descent algorithm**, by Dong et al.

Then we use **fuzzy distances** and **fuzzy sets**. Two intuitions here; first, we are trying to capture the fact that some points are close, and some points are far from each other. But another, because of the dimensionality curse ([[curse_dim]]), all pairwise distances in a large-D space are pretty similar, so while the closest point may be far away, the next closest point will be right behind it. So we rescale all distances (_I think?_) to look not at their absolute values, but on distance differences between the k closest points. This also means that fuzzy distances aren't symmetric at first (ij ≠ ji), but then we treat them as 2 estimations of a probability that this edge exists, and end up with one distance: $p = (ij + ji) - ij\cdot ji$.

**Actual embedding**: draw a graph, calculate same thing on it, then optimize the position of points to reduce the cross-entropy ([[cross-entropy]]) between the probabilities of edges existence inferred from the original high-dim data $h_e$, and from this new embedding graph $l$: $L = \sum_e \big( h \log(\frac{h}{l}) + (1-h)\log(\frac{1-h}{1-l}) \big)$.

> I don't understand this, as this formula seems more similar to [[kl]] divergence, rather than cross-entropy! I've checked the paper. Is it really cross-entropy? Or do they somehow use the term loosely?

Except that to avoid sampling through all possible edges (most of which don't exist anyways!), we use the **negative sampling** trick, similar to how word2vec does it ([[neg_sampling]]). And we use Stochastic Gradient Descent to optimize this L (see [[06_DL]]).

# Practical consequences 

* Stochastic, random starting point, followed by SGD. No guarantee of replicability.

# My musings

A practical consequence to how the k neighbors is used, is that tightly placed points (cliques) will find these k neighbors for cheap (they are all close to each other), and so won't feel any pull or push from other points in the set. On the other hand, lonely points will reach out until they reach k neighbors, which means that points that will be "close" to them are likely to be pretty far from each other. At the embedding stage, it will tuck these "loners" in the middle of the picture, instead of repelling them on the periphery.

Now, for tightly place points, it means that the organization of points inside a tight cluster doesn't necessarily make much sense. Unless there's a continuous transition from one cluster to another, similar "axes" within clusters may be oriented differently in a projection, as clusters won't interact. Lonely points may actually introduce some organization to this, as after distance symmetrization, they will be placed "in-between" those points that happened to be "closest" to them. But if you think of it, it will only rotate clusters "towards" each other, not "along" each other. So if "ABC" and "XYZ" are clusters, there's no guarantee that you'll get them both aligned like that, and in fact more likely, you'll get "ABC" and "ZYX",  or maybe "CBA" and "XYZ", or maybe random.

# Refs

* [https://pair-code.github.io/understanding-umap/](<https://pair-code.github.io/understanding-umap/>)
* [https://towardsdatascience.com/how-exactly-umap-works-13e3040e1668](<https://towardsdatascience.com/how-exactly-umap-works-13e3040e1668>)
* [https://towardsdatascience.com/how-to-program-umap-from-scratch-e6eff67f55fe](<https://towardsdatascience.com/how-to-program-umap-from-scratch-e6eff67f55fe>)
* Category theory behind UMAP: https://johncarlosbaez.wordpress.com/2020/02/10/the-category-theory-behind-umap/

https://umap-learn.readthedocs.io/en/latest/how_umap_works.html
Very readable non-mathy explanation of how UMAP works.
2018, Leland McInnes

Pretty interactive simulation, with use case of UMAP on the ImageNet dataset:
https://tiga1231.github.io/umap-tour/
(by Mingwei Li, Carlos Scheidegger)
Here they project from 150K-dim original dataset to 15 dimentions, and then interpret / visualize / rotate through these dimensions. Then they use UMAP to interpret unit activations in a deep image classifying network,

Main paper:
McInnes, L., Healy, J., & Melville, J. (2018). Umap: Uniform manifold approximation and projection for dimension reduction. arXiv preprint arXiv:1802.03426.
https://arxiv.org/abs/1802.03426
(2k citations in 2 years heh)

Narayan, A., Berger, B., & Cho, H. (2020). Density-Preserving Data Visualization Unveils Dynamic Patterns of Single-Cell Transcriptomic Variability. bioRxiv.
https://www.biorxiv.org/content/10.1101/2020.05.12.077776v1?ct=
Like UMAP but also doesn't equalize density of points, but tries to preserve it. Important for good visualizations of real data. Trains a small deep network in the background (TF + Keras) to learn the transformation.
https://umap-learn.readthedocs.io/en/latest/densmap_demo.html
As of Dec 2020, the `densmap=True` version of IMAP wasn't yet in the default conda install, even tho the documentation totally describes it.

Related Wiki pages:
* https://en.wikipedia.org/wiki/%C4%8Cech_complex
* https://en.wikipedia.org/wiki/Vietoris%E2%80%93Rips_complex
* https://en.wikipedia.org/wiki/Nerve_of_a_covering