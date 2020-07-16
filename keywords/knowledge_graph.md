# Knowledge graphs - semantic embedding

#graph #embedding #semantic

Parents: [[09_Graphs]] / [[graph_embedding]]
Siblings: [[node2vec]]
See also: [[word2vec]], [[embedding]]

## TransE

**TransE**: an alternative approach that doesn't actually rely on random walks. Ref: Borders 2013.

We have a bunch of objects (for example, books, authors, genres, usually referred to as **entities**), and edge connecting them, that correspond to predicates (like, this book belong to this genre, usually called **relations**), and there are many different types of links. The task we have is **graph completion**, or guessing links that are missing.

**Translating embeddings**: relationships are represented as triplets (h, l, r) = (head entity, relation, tail entity). Entities are embedded in entity space $ℝ^k$ (k~50 for a dataset with ~1000 different relation types!!). Relationships are represented as translations in this entity space: h+l ≈ t if the relationship is true (so $l ∈ℝ^k$ as well, but they have a distribution of their own)

> This sounds similar to word2vec idea of king-male+female = queen. But how is it possible that a dataset with so many different types of relationships (Freebase, ~1000 types of edges) can be successfully presented in a space so much flatter (only k~50 dimensions? How is it even possible? Would not it mean that very different relationship types (genre, author) have to "share" dimensions (subspaces, hyperplanes)? How can they avoid conflicts?

We initialize entities e_i as a uniform distribution. Translation vectors are init as uiform, then normalized (so uniform on a sphere).

Then we use contrastive loss (aka triplett loss [[triplet_loss]]): at each step we move 2 related items closer to their "target positions" (based on the meaningful translation l), and 2 unrelated (randomly sampled) items further apart. The gradient is given by the formula:

$\displaystyle \sum \nabla\big[ γ+d(h+l, t)-d(h'+l, t') \big]$ where (h,l, t)is a positive sample, (h', l, t') is a negative sample, and d is a distance between the target point h+l and the actual point t (**dissimilarity measure**: in practice either L1 or L2 norm). We perform SGD on both entites and translations. Every now and then (after each minibatch of triplets) we also normalize all entity vectors e ← e/||e||.

When sampling negative elements, we do not care about the "type" of these elements; we sample completely at random, which is the whole point. For example, if a correct statement is (Paris , is the capital of, France), then we don't hunt for h' and t' that would make the statement "incorrect but sensible" (London, is the capital of, Romania). We sample entities completely at random (JKRowling, is the capital of, bird), and that's the whole point, as it will eventually align values "properly". Typically, in practice, we keep either head or tail the same as for the "positive triplet", and only randomly sample the other element.

"Sibling relationships" (both A and B are written by X) are represented by points that are very close to each other, at least within some hyperplane.

> How do translation vectors get to be distributed eventually? Are similar relationships (in some semantic way) co-aligned? Or is it the other way around, and it is very dissimiarl relationships that are forced to share a hyperplane, as if there's no overlap between the items that engage in these relationships, and so there's no chance for a mistake? Say, if "being a capital of" and "having made of" are co-aligned, we may never notice it, as London is not made of anything in particular, and a spoon is a capital of no country. Is it what makes a low-dim embedding possible to begin with?

# Refs

Conspects of a lecture describing TransE
https://snap-stanford.github.io/cs224w-notes/machine-learning-with-networks/node-representation-learning

Main TransE publication:
Bordes, A., Usunier, N., Garcia-Duran, A., Weston, J., & Yakhnenko, O. (2013). Translating embeddings for modeling multi-relational data. In Advances in neural information processing systems (pp. 2787-2795).
http://papers.nips.cc/paper/5071-translating-embeddings-for-modeling-multi-relational-data.pdf
(2.5k citations)

Potentially another relevant paper?:
Wang, Z., Zhang, J., Feng, J., & Chen, Z. (2014, July). Knowledge graph embedding by translating on hyperplanes. In Aaai (Vol. 14, No. 2014, pp. 1112-1119). (1k+ citations)