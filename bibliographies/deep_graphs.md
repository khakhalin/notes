# Deep learning on graphs

Parents: [[dl]], [[graphs]]
Related: [[graph_embedding]] (shallow encoders), [[convnet]], [[transformers]]

#dl #graph


Children:
* [[graphsage]] - the next iteration of this idea


**Reminder**: We want a function f( ): graph → low-dim embedding. How to learn f()? In some sense, f has to learn the structure of the network (graphlets, motifs, etc.) Similar nodes (graph similarity) should be projected to points that are close together (say, cosine proximity = $z_u ^⊤ z_v$). Encoder ENV(v) = z_v. **Shallow encoder**: directly learn coordinates Z, + similarity coming from random walks.

**Limitations of shallow encoders:**
* Lots of parameters to remember (V nodes × N dim)
* Inherently transductive (learning one network doesn't help to embedd the other; we have to start anew). Same actually if the network changes a bit: no obvious way to update.
* Does not naturally incorporate note features / properties / labels.

We now want to build a **deep network** that captures the idea of **convolution** (matrices that are shared across the graph), meaningful outputs embeddings (as before), and can naturally incorporate and mix note features and topology. But modern DL techniques aren't automatically applicable to graphs because ConvNets rely on uniform local structure, and the absense of combinatorial invariance (both untrue for graphs).

There's some hope though: if we look at smallest convolutions 3×3 → 1 px. We can replciate a similar similar extremely local logic for graphs (as the idea of closest neighbors is well defined on graphs). We need to look at "messages" (information) that is sent by immediate neighbors of each node to this node. And later if we iteratively integrate this info for all nodes, it should capture the structure of the graph. Another good thing about this idea that the "information" sent by every node may include half-baked info about the topology of the graph around it, as well as node features (as this time around we want node features to actively parcitipate in graph encoding).

Naive idea: feed the adjacency matrix row by row directly to the network. Wouldn't work:
* Too many parameters (V, as every node may be connected to every one)
* Doesn't scale across graphs of different size (even if we learn it)
* Still not permutation-invariant

So let's return to the "send information" approach. Let every node aggregate information from its neighbors. This **aggregation function** has be order-invariant (mean, sum, max, Hadamard etc.). If we do it recursively and **unroll**(each node collects info from its neighbors, that in turn collect info from their neighbors etc.), we get a funnel-like "information collection basin" for every node in the network. We init the "leafs" with feature vectors, and then pass these vectors on. The total depth of this info collection shouldn't be too deep (≤ graph diameter).

> I don't quite understand why we believe that it has to work with depth ≤ graph diameter. Overall, even though we look at these networks in an "unrolled" way, it does look like a recurrent neural network that tries to come to some sort of equilibrium. Isn't it possible that giving it more time to run would create a more interesting distribution of "characteristic vectors" along nodes? Or do we have some reasons to believe that it should be fast? Or are we trying to intentially force it to be fast, in order not to ran an actual RNN?

How to define **aggregators** (that collect info from heighbors)? The simplest approach: mean().

> Apparently there's some theoretical result that claims that sum() is the theoretically best approach. Really?

Let's call $h^k_v$ a representation stored at node $v$ at $k$-th step of this process (k runs from 0 to K). 
$h^0_v$ is initialized as node features (or see below if there are no features), then recursively:
$\displaystyle h^k_v = ϕ \left(W_k \sum_{u ∈ N(v)} \frac{h^{k-1}_u}{|N(v)|} 	+ B_k h^{k-1}_v\right)$. Here:
* ϕ - some non-linearity (usually ReLU)
* $N(v)$ - the neighborhood of node v; and we can see that we're averaging over neighbors
* W_k - transformation matrix. Dimensionality of about ~100×100 typically.
* B_k - another transofmation matrix that adjust old info (k-1) for this node to the "new format" (k)

Note that the transformation matrix W_k doesn't see vectors of neighboring nodes directly; it sees them only after aggregation, so its dimensionality is defined by the dimensions of our target representation vectors, not by the degree of each particular degree! A node can be overconnected, or underconnected, and the equation will still work, and still use the same (shared) W_k! It's the $h^{k-1}_u$ vectors that will probably be pretty different for over-connected and under-connected nodes, once the network is properly trained.

What if there are no (or little) of node features? (_Some speculations here:_)
* We can try to algorithmically (heuristically) describe these nodes to make them more unique (create a starting feature vectors with their degrees, clustering coefficients, centralities etc.)
* If we use non-mean aggregation, we can probably just init them with scalar constants. It won't work for mean, as mean(1,1,1,1,1) = mean(1), but it will work for sum(), in which case already $h^1$ will encode node degree.
* If the graph is small, we can give each node a unique id (one-hot or binary?)
* We could also try to assign them random vectors, as unique ids, hoping that most of these vectors will be orthogonal, and thus it wouldn't be that different from careful id-ing.

> Would it be useful to combine sum() and mean() in this aggregation? sum() for some coordinates, and mean() for some others? Say, for the network to "see" how well the node is connected, it would be nice to be able to sum(), not mean(), some of the dimensions. If every naive node at step 0 has a 1 for this dimension, at each point the network woudl have direct access to the connectess of this node. With mean() you can no longer encode it with just one dimension. At the same time, for some other properties (features, motifs), intuitively, mean() could work better (and mitigate the problem of scale unpredictability, that of course becomes a bit of an issue with sum() ).

**How do we train them?** We are trying to learn the weights of transformation matrices {W_k} and {B_k}.

We can try to train it in unsupervised mode, as we did before ([[graph_embedding]]), reconstructing something about the graph (edges? node features?). We can also use supervised training, if there's something task-specific that we want to learn to do (node label prediction?)

> Philosophically, this "unsupervised" mode is of course "supervised", or "self-supervised", as we have the labels (presence of edges; node classes) or target values (node properties) to learn. Just these labels / values come from the same dataset, not from some external data, as we repeatedly hide it from the model, and use the rest of the graph to "guess them". Same as for image augmentation, for example. It's kinda unsupervised, but with the math of supervised learning.

**Loss function**: if $y_v$ are labels, we would just try to predict tem from node embeddings:

$\displaystyle L = \sum_{v ∈ V} y_v \log \big( σ(z_v^⊤ θ) \big) + (1-y_v) \log\big( 1- σ(z_v^⊤ θ)\big)$ . A classic cross-entropy loss; 
σ is a sigmoid; see: [[classification]]

Now the model should be able to transfer from one graph to another, as long as the logic is similar. More like normal DL (across graphs), and more like convolution (within a graph). We can now recalculate node representations if a graph changed a bit, and we can train on smaller parts of a graph, and then generalize to the entire graph.

# Refs

Lecture by Leskovec:
https://www.youtube.com/watch?v=6izOYKGuXTQ&list=PL1OaWjIc3zJ4xhom40qFY5jkZfyO5EDOZ&index=11
Lecture slides:
http://web.stanford.edu/class/cs224w/slides/08-GNN.pdf
Conspect:
https://snap-stanford.github.io/cs224w-notes/machine-learning-with-networks/graph-neural-networks