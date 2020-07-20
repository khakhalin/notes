# Deep learning on graphs

#dl #graph

Parents: [[06_DL]], [[09_Graphs]]
Related: [[graph_embedding]] (shallow encoders), [[cnn]], [[transformers]]

Subtopics:
* [[graphsage]] - the next iteration of this idea



**Reminder**: We want a function f( ): graph → low-dim embedding. How to learn f()? In some sense, f has to learn the structure of the network (graphlets, motifs, etc.) Similar nodes (graph similarity) should be projected to points that are close together (say, cosine proximity = $z_u ^⊤ z_v$). Encoder ENV(v) = z_v. **Shallow encoder**: directly learn coordinates Z, + similarity coming from random walks.

**Limitations of shallow encoders:**
* Lots of parameters to remember (V nodes × N dim)
* Inherently transductive (learning one network doesn't help to embedd the other; we have to start anew). Same actually if the network changes a bit: no obvious way to update.
* Does not naturally incorporate note features / properties / labels.

We now want to build a **deep network** that captures the idea of **convolution**, outputs embeddings, and can naturally incorporate note features. But modern DL techniques aren't automatically applicable to graphs because ConvNets rely on local structure, and the absense of combinatorial invariance. 

There's some hope though if we look at smallest convolutions 3×3 → 1 px. We can do similar at graphs (because at least the idea of closest neighbors is well defined): we need to look at what "messages" (information) is sent by immediate neighbors of each node to this node. And later if we integrate this info for all nodes, it should capture the structure of the graph. Another good thing about this idea that the "information" sent by every node may include half-baked info about the topology of the graph around it, and node features (and this time around we want node features to actively parcitipate in graph encoding).

Naive idea: feed the adjacency matrix row by row directly to the network. Wouldn't work though:
* Too many parameters (V, as every node may be connected to every one)
* Doesn't scale across graphs of different size (even if we learn it)
* Still not permutation-invariant

So let's return to the "send information" idea. Let every node aggregate information from its neighbors. This aggregation has be order-invariant (mean, hadamard etc.). If we do it recursively and **unroll**(each node collects info from its neighbors, that in turn collect info from their neighbors etc.), we get a slightly different fan-like "information collection basin" for every node in the network. We init the "leafs" with feature vectors, and then pass it on. The total depth of this info collection shouldn't be too deep (≤ graph diameter).

> I don't quite understand why we believe that it has to work with depth ≤ graph diameter. Overall, even though we look at these networks in an "unrolled" way, it does look like a recurrent neural network that tries to come to some sort of equilibrium. Isn't it possible that giving it more time to run would create a more interesting distribution of "characteristic vectors" along nodes? Or do we have some reasons to believe that it should be fast? Or are we trying to intentially force it to be fast, in order not to ran an actual RNN?

How to well define **aggregators** (that collect info from heighbors)? The simplest approach: mean().

> Apparently there's some theoretical result that claims that sum() is the theoretically best approach. Really?

Let's call $h^k_v$ a representation stored at node $v$ at $k$-th step of this process (k runs from 0 to K). 
$h^0_v$ is initialized as node features (or see below if there are no features), then recursively:
$\displaystyle h^k_v = ϕ \left(W_k \sum_{u ∈ N(v)} \frac{h^{k-1}_u}{|N(v)|} 	+ B_k h^{k-1}_v\right)$. Here:
* ϕ - some non-linearity (usually ReLU)
* $N(v)$ - the neighborhood of node v; and we can see that we're averaging over neighbors
* W_k - transformation matrix. Typical dimensionality ~100×100 typically.
* B_k - another transofmation matrix that adjust old info (k-1) for this node to the "new format" (k)

Note that the transformation matrix W_k doesn't see vectors of neighboring nodes directly; it sees them only after aggregation, so its dimensionality is set by the dimensions of our target representation vector, not by the degree of each particular degree! A node can be overconnected, or underconnected, and the equation will still work, and still use the same (shared) W_k! It is the $h^{k-1}_u$ vectors that will probably be pretty different for over-connected and under-connected nodes, once we train this thing well.

What if there are no (or little) of node features? (_Some speculations here:_)
* We can try to algorithmically (heuristically) describe these nodes to make them more unique (put there their degrees, some other properties)
* If you use non-mean aggregation, you can probably just init it is constants
* We could also try to give them random vectors, as unique ids?

> Would it be useful to combine sum() and mean() in this aggregatin: sum() for some coordinates, and mean() for some others? Say, for the network to "see" how well the node is connected, it would be nice to be able to sum(), not mean(), some of the dimensions. If every naive node at step 0 has a 1 for this dimension, at each point the network woudl have direct access to the connectess of this node. With mean() you can no longer encode it with just one dimension. At the same time, for some other properties (features, motifs), intuitively, mean() could work better (and mitigate the problem of scale unpredictability, that of course becomes a bit of an issue with sum() ).

**How do we train them?** We are trying to learn the weights of transformation matrices {W_k} and {B_k}.

We can try to train it in unsupervised mode, as we did before ([[graph_embedding]]), reconstructing somethign about the graph. We can also use supervised training, if there's something task-specific we want to learn to do (be it link prediction, node type prediction etc.)

**Loss function**: if $y_v$ are labels, we would just try to predict tem from node embeddings:

$\displaystyle L = \sum_{v ∈ V} y_v \log \big( σ(z_v^⊤ θ) \big) + (1-y_v) \log\big( 1- σ(z_v^⊤ θ)\big)$ . A classic cross-entropy loss: [[03_Classification]]

> Here σ is a sigmoid, no longer a ReLU, right?

Now the model should be able to transfer from one graph to another, as long as the logic is similar. More like normal DL (across graphs), and more like convolution (within a graph). We can now recalculate node representations if a graph changed a bit, and we can train on smaller parts of a graph, and then generalize to the entire graph.