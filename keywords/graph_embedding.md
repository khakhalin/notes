# Graph representation learning

Parent: [[graphs]]
See also: [[embedding]], [[deep_graphs]]
Children: [[node2vec]], [[transE]] (knowledge graph)

#graph #embedding


A reminder on possible tasks on graphs:
* Node classification: some notes are labeled, but not all; we want to label unlabeled nodes.
* Link prediction: some edges are unknown; we want to guess them.

Typical part of a data science pipeline: Feature engineering (from raw data to something that can be used). 80% of effort during DS development :) It would be nice to learn features automatically. And that something DL is supposed to be able to do. But how to apply it to graphs? Can we avoid engineering all these graph features [[graph_properties]] manually?

Ultimately we want graph → vector pipeline. Feature representation embedding. Probabily node → vector (and then we can deal with vectors for diff nodes later). We want:
* similar nodes have different representations.
* network info be recorvered from node representations (through summing, averaging, pooling?)
* this process to be parallelizable

Even a typical 2D representation of a graph for visualization purposes may be considered a simple case of node representation, as node position in (x,y) depends on its connections, and may resolve communities (if the graph is small / sparse enough, like the Karate club z.B.)

Key problem with graphs: no single reference point, aka **isomorphism problem**. You can permute a lot, and it's still the same graph supposedly. (Not the case with images, text, or audio).

Consider graph G(V, E), A is the adjacency matrix (no weights, no node features for now)

Encoder: enc(v) → z ∈ $R^d$ (embedding space). d (dimension) is either custom, or default, but usually in the order of 10^2.

Two **similar** nodes should have high dot product ⟨ zi, zj ⟩ = ziᵀzj. In practice, "Similar" needs to be defined to better reflect your particualr task, but some common themes include:
* connected
* share neightors
* have "similar roles within a graph, e.g. similar positions within a motif"
* external properties (features, labels)

# Random walks

**Shallow encoder**: embedding lookup, enc(v) = Zv where v is an indicator vector, and Z is essentially a 	table describing nodes. Matrix Z will be optimized to drive a loss down. Examples: DeepWalk, node2vec, TransE

**Random walk approaches**: for every node, generate a bunch of random walks starting from this node (aka "random walks with restart"). How many walks to run, and what the stopping criteria a, would form a strategy (R). A simplest strategy would be to just run lots of short random walks of fixed length L; a fancier strategy may include some adaptive sampling / exploration.

Simplest situation: If we are going after co-location in a graph, we can expect two nodes v and u co-occur within random walks if they are located "close" to each other. Each random walk is a sequence, or a multi-set (like a set, but each element may be present several times, as we may cycle and visit every node more than once). 

> They claim that random walks are efficient, as they are local, and so allow us to work with very large graphs, aka they scale well. A natural question perhaps is why not use some flow-based approach; why keep it discrete. Isn't it a bit lazy? Or is there a real practical or theoretical benefit here?

> The answer here is that you cannot afford to look at the full neighborhood, as it exponentially explodes, so you have to use some heuristic to prune it, in which case it's not different practically from a smart (aka "biased") random walk exploration strategy. I.e. if you explore the "let's not do random walks" strategy honestly, you'll proably converge on a very similar practical solution. Moreover, in practice random walks are often simulated.

> So a follow-up question: how to implement (or simulate) random walks in practice, effeciently?

## Implementations

* [[node2vec]] - optimization of random walk embedding using biased graph exploration, negative sampling and chierarchical softmax (essentially, optimized random walks, and a segue to [[word2vec]])

# Other methods

[[knowledge_graph]] - semantic embedding through negative sampling

# Embedding entire graphs

Once you embedded nodes meaningfully, how do you embedd an entire graph, to classify them? One option: just sum all vectors together, then build a classifier on this.

You can also do similar summarization on subgraphs (like in graph condensation), then run a graph embedding again (now representing each **virtual node** (Li 2016) as a point). Repeat recursively?

**Anonymous walks** (2018): instead of preserving node identity, only look at how often you visit notes again (as in "one node, another node, the 1st node again, etc.") If you process all these anonymous walks together, it will tell you something about the character of your graph.

# Refs

Standford Lecture:
https://www.youtube.com/watch?v=2XFMAdHa8uY&list=PL1OaWjIc3zJ4xhom40qFY5jkZfyO5EDOZ&index=6

Conspects of this lecture:
https://snap-stanford.github.io/cs224w-notes/machine-learning-with-networks/node-representation-learning