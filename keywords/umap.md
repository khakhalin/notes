# UMAP: Uniform Manifold Approximation and Projection

#clustering #dim #features

Parents: [[04_Features]], [[algebraic_topology]]
Related: [[curse_dim]], [[tsne]], [[neg_sampling]]

Open questions ( #todo ):
* Practical advice on picking `n_neighbors`
* The meaning of the `local_connectivity` parameter (default = 1). I thought that every lonely point is forced to be connected to `n_neighbors` points, but apparently, according to this doc, no? https://umap-learn.readthedocs.io/en/latest/api.html
* Is it reproducible? How to use it in pipelines if it is not reproducible?
* Any advice on which distance measure to use, when they are not precomputed? (it can do like 20 different ones)
* This new develoipment  about parametric embedding
* Is there a trick to organizing elements within each cluster, or is it impossible / useless? If you do UMAP on a full set, then UMAP on a cluster, will the representation of this cluster in the "big picture" be similar to its local analysis? In any sense? Or are they absolutely incomparably different? Judging from the fact that q(d) approximates a step function, I would expect all clusters to be chaotic… But is it true, and is it always true?
* How does the supervised UMAP work? (Apparently it exists	)

# Background

**Simplicial complexes**: a way to replace continuous geometry (hard) with simple combinatorial algebra; a much more formulaic way to describe structures, which yields power! (See [[algebraic_topology]])

**Čech cohomology** or **Čech complex** - a way to represent a bunch of sets with a siimplicial complex. If you have a bunch of sets, represent each with a node. Connect 2 nodes if they intersect (crucially, not necessarily on a node; if they intersect anywhere in the space); connect 3 nodes with a face if all 3 sets have an intersection etc. How can it help us to represent the data? Surround each data point with a neighborhood area, then look at which neighborhoods had intersected. This operation would turn the dataset into a simplicial complex (but not necessarily connected at this point).

**Vietoris–Rips complex** - a relaxed generalization of Cech complex. A set of nodes S is made a simplex (of an appropriate dimension, matching the size of S) iif the distance between any two nodes in S is smaller than a certain threshold δ. So it's like the Chech complex, but looking only at intersecting neighborhoods on our data (neighborhoods including nodes); not just intersecting "somewhere in the void". Easier to build; easier to work with; doesn't depend on the actual embedding of the dataset in a metric space; it's enough that a local metric (node-to-node distances) is available.

UMAP works by finding a low-D embedding that has the same topological representation as the original data. Apparently, this original reliance on simplicial complexes, and not just on **neighborhood graphs**, is what makes UMAP so powerful, and with theoretical guarantees. Even though in practice we usually work with graphs (so only 0 and 1 simplices), and not higher structures. So in practice this difference seems to be moot.

# Algorithm

The plan:
1. For each point, find some neighbors (the question is which ones to pic)
2. Create edges to these neighbors, and give them weights, the further, the weaker (the question is, again, which formula to use here)
3. Now, for the low-D embedding, define a a certain function to optimize  (the question is, which one),
4. and optimize it by moving the points (a practical question: how)

### Selecting neighbors

First, for each point finds its nearest point. Remember the distance to this point, this will be an important local parameter: $ρ_i = \min_j d(i,j)$. 

Then **expand each neighborhood** until it covers at least **k neighboring points**. There's a fast algorithm for that (and UMAP uses it) called the **Nearest Neighbor Descent algorithm** (Dong 2011). This k is the key parameter of the method. It does not guarantee that the manifold as a whole is connected (it can still consist of several unconnected pieces), just that no single point will be unconnected.

### Edge weights

Then we use **fuzzy distances** and **fuzzy sets**. Two intuitions here; first, we are trying to capture the fact that some points are close, and some points are far from each other. But another, because of the dimensionality curse ([[curse_dim]]), all pairwise distances in a large-D space are pretty similar, so while the closest point may be far away, the next closest point will be right behind it. So we offset all distances by ρ, to look not at their absolute values, but on distance between k closest points compared to the 1st closest point; and also exponentiate them, to shift them into a different distribution:

$\displaystyle \hat p_{ij} = \exp \left( \frac{d(i,j)-ρ_i}{σ_i} \right)$

> I'm assuming that σ_i may be the std of d(i,j) for these k distances, but I'm not sure.

Now these fuzzy distances aren't symmetric; $\hat p_{ij} ≠ \hat p_{ji}$, as $ρ_i ≠ ρ_j$, but if we treat them as two estimations of a probability that this edge exists, then a logical way to build a symmetric estimator: $p_{ij} = (ij + ji) - ij\cdot ji$.

A big practical trick here is that we never normalize probabilities (we never ensure that they sum to one by calculating a sum of draft probabilities and dividing by it). Which feels risky, but actually apparently works, yet saves a ton of resources during optimization.

### Embedding and objective function

Throw some points in a lower space. Now, in low-D we want to calculate something similar to what we had in high-D, but with an important correction: while in high-D the "scale" of neighborhood was different for every point (ρ_i), in low-D, it's a const. We make this const a hyperparameter `min_dist`. To calculate the "probability of connection in low-D" (q_ij, to compare it to p_ij), we look at positions of points, and transform the distance between them with a weird formula:

$q_{ij} = (1 + a \cdot d(i,j)^{2b})^{-1}$ , where d(i,j) is Euclidean distance in low-D, and a and b are hyperparameters that are found using non-linear list square fit (!!), to keep all distances below min_dist as flat (at 1) as possible, followed by an exponential decay $\exp (-d(ij)-\text{mindist})$ after that. This part sounds weird, but as far as I undestand, it's just to make it computationally fast.

Once all q_ij are found, we calculate binary cross-entropy ([[cross-entropy]]) between the probabilities of edges existence inferred from the original high-dim data, and from this new embedding graph. Because p and q are interpreted as probabilities of an edge existing, for each edge we get 2 terms:

$L = \sum_{ij} \big( p \log(\frac{p}{q}) + (1-p)\log(\frac{1-p}{1-q}) \big)$.

Here I was at first really confused, as 1) this formula looks more like KL divergence [[kl]] (just without a minus, and with fractions flipped), all while 2) some sources (like Oskolkov 2020) explicitly point out that UMAP uses cross-entropy, unlike tSNE [[tsne]] that uses KL.

The first confusion, is easy to resolve: $L = -\sum_{ij} \left( p \log(\frac{q}{p}) + (1-p)\log(\frac{1-q}{1-p})\right)$ that we get if we flip the fractions indeed looks like KL written for every edge (this is critical! Written for every edge, then summed across edges). But from this point of view, it doesn't matter whether it's a KL or a CE, as the distribution P is constant, so KL(p,q) and H(p,q) only differ by H(p), which is constant.

The 2nd confusion stems from the terminology used. tSNE looks at values of p and q, and explicitly optimizes KL across all edges (NOT for each edge, summed across all edges). So tSNE loss function looks like $-\sum_{ij} p_{ij} \log(q_{ij}/p_{ij})$, which means that compared to the loss above it doesn't have the 2nd term. And here lies the difference. For large p both tSNE and UMAP maximize the same $\sum p \log(q)$ (here for UMAP I ignore 2nd term that has (1-p) as a factor, and then for both UMAP and tSNE I get rid of the const H(P)). Which means that for high p, we try to make q as high as possible. which is achieved by minimizing distances in low-D. But for small p, tSNE does nothing, while UMAP optimizes $\sum (1-p) \log(1-q)$, moving points as far as possible. And that preserves global structure.

### Optimization process

Optimization happens via **Stochastic Gradient Descent**  (see [[06_DL]]).  

We init to graph laplacian ([[eigenmaps]], aka spectral embedding).

To avoid sampling through all possible edges (most of which don't exist anyways), we use the **negative sampling** trick, similar to how word2vec does it ([[neg_sampling]]). At the same time, it is this sampling that makes UMAP preserve some global structure, compared to tSNE, as essentially it works like [[mds]] at long distances, but as a fast neighborhood graph method at lower distances.

Because of SGD, technically, there is **no guarantee of replicability**. You can make the results fixed, given the data, by fixing the random generator seed (param `random_state`), but adding an extra point to the data would change everything once again.

# Parameters and their meaning

`n_neighbors`: small values (~5 or 10) create tiny clusters and wispy threads. Large values (>50) create more of a gradient cloud with no clusters. But even then, as it's not truly global, there's no guarantee that all global extremes of the distribution will be place logically, and also, that they won't be split into several "representations" in different parts of the cloud.

`min_dist`: literally, mean distance between projecting points (from 0 to 1; default of 0.1). Not sure what the units are, but with it too small, some points overlap, while with it too large (>0.25?) some points are so repelled from each other that clouds become airy and start to overlap with each other, ruining all local structure.

`spread`: somehow (not sure) regularizes the cloud of points on the plane. Default value of 1, and gives a reference for the `min_dist` parameter (they operate on the same scale).

`n_components`: unlike tSNE ([[tsne]]), scales well to high-dimension cases. A value of 1 is also possible, and produces a somewhat reasonable path through the manifold.

`metric`: which metric to use (unless we're using `precomputed`). Supports quite a few (see [[metric]] for definitions). One can also write custom metrics (as long as they support numba). See "umap params" ref for sample code.

# Comparison to other methods

There was a claim that UMAP works much better than t-SNE [[tsne]], in terms of recovering the local topology of low-D manifolds (a cord loop), but as it turned out later, all difference is due to the fact that by default UMAP initialized its point distribution with Laplacian Eigenmaps ([[eigenmaps]]), while tSNE used random init. If both are init identically (using PCA or something like that), they perform identically: sometimes one wins a bit, sometimes another one wins a bit, in terms of recovering the ground truth, or finding an embedding with highest "low-D distance to high-D distance correlation), depending on the dataset. Still, it's a very special case (round cord loop), where all structure is local, and there's almost no global structure to preserve (at least no complex, asymmetrical structure).

Footnotes:
* Original paper: Becht, E., McInnes, L., Healy, J., Dutertre, C. A., Kwok, I. W., Ng, L. G., ... & Newell, E. W. (2019). Dimensionality reduction for visualizing single-cell data using UMAP. Nature biotechnology, 37(1), 38-44.
* Critique: Initialization is critical for preserving global data structure in both t-SNE and UMAP. Kobak, Linderman. 2021. Nature Biotechnology

# Musings

A practical consequence to how the k neighbors is used, is that tightly placed points (cliques) will find these k neighbors for cheap (they are all close to each other), and so won't feel any pull or push from other points in the set. On the other hand, lonely points will reach out until they reach k neighbors, which means that points that will be "close" to them are likely to be pretty far from each other. At the embedding stage, it will tuck these "loners" in the middle of the picture, instead of repelling them on the periphery.

Now, for tightly place points, it means that the organization of points inside a tight cluster doesn't necessarily make much sense. Unless there's a continuous transition from one cluster to another, similar "axes" within clusters may be oriented differently in a projection, as clusters won't interact. Lonely points may actually introduce some organization to this, as after distance symmetrization, they will be placed "in-between" those points that happened to be "closest" to them. But if you think of it, it will only rotate clusters "towards" each other, not "along" each other. So if "ABC" and "XYZ" are clusters, there's no guarantee that you'll get them both aligned like that, and in fact more likely, you'll get "ABC" and "ZYX",  or maybe "CBA" and "XYZ", or maybe random.

# Refs

#toread
https://towardsdatascience.com/tsne-vs-umap-global-structure-4d8045acba17	

#toread
Böhm, J. N., Berens, P., & Kobak, D. (2020). A Unifying Perspective on Neighbor Embeddings along the Attraction-Repulsion Spectrum. arXiv preprint arXiv:2007.08902. https://arxiv.org/abs/2007.08902

* [https://pair-code.github.io/understanding-umap/](<https://pair-code.github.io/understanding-umap/>)
* [https://towardsdatascience.com/how-exactly-umap-works-13e3040e1668](<https://towardsdatascience.com/how-exactly-umap-works-13e3040e1668>)
* [https://towardsdatascience.com/how-to-program-umap-from-scratch-e6eff67f55fe](<https://towardsdatascience.com/how-to-program-umap-from-scratch-e6eff67f55fe>)
* Category theory behind UMAP: https://johncarlosbaez.wordpress.com/2020/02/10/the-category-theory-behind-umap/

Oskolkov 2020:
A really detailed well explained blog post that goes through a very detailed step-by-step comparison of both algorithms: https://towardsdatascience.com/how-exactly-umap-works-13e3040e1668

https://umap-learn.readthedocs.io/en/latest/how_umap_works.html
Very readable non-mathy explanation of the principles of UMAP from sklearn umap team.
2018, Leland McInnes

Parameters:
https://umap-learn.readthedocs.io/en/latest/parameters.html

Pretty, interactive simulation, with use case of UMAP on the ImageNet dataset:
https://tiga1231.github.io/umap-tour/
(by Mingwei Li, Carlos Scheidegger)
Here they project from 150K-dim original dataset to 15 dimentions, and then interpret / visualize / rotate through these dimensions. Then they use UMAP to interpret unit activations in a deep image classifying network,

Main paper:
McInnes, L., Healy, J., & Melville, J. (2018). Umap: Uniform manifold approximation and projection for dimension reduction. arXiv preprint arXiv:1802.03426.
https://arxiv.org/abs/1802.03426
(2k citations in 2 years heh)

Narayan, A., Berger, B., & Cho, H. (2020). Density-Preserving Data Visualization Unveils Dynamic Patterns of Single-Cell Transcriptomic Variability. bioRxiv.
https://www.biorxiv.org/content/10.1101/2020.05.12.077776v1?ct=
An further development of UMAP that  doesn't equalize density of points, but tries to preserve it. Important for good visualizations of real data. Trains a small deep network in the background (TF + Keras) to learn the transformation.
https://umap-learn.readthedocs.io/en/latest/densmap_demo.html
As of Dec 2020, the `densmap=True` version of IMAP wasn't yet in the default conda install, even tho the documentation totally describes it.

Related Wiki pages:
* https://en.wikipedia.org/wiki/%C4%8Cech_complex
* https://en.wikipedia.org/wiki/Vietoris%E2%80%93Rips_complex
* https://en.wikipedia.org/wiki/Nerve_of_a_covering

The "Dong 2011" paper cited above, about fast neighborhood comparison algorithm:
Dong, W., Moses, C., & Li, K. (2011, March). Efficient k-nearest neighbor graph construction for generic similarity measures. In_Proceedings of the 20th international conference on World wide web_(pp. 577-586).
https://www.cs.princeton.edu/cass/papers/www11.pdf