# Deep graph generation with graphRNN

Parents: [[09_Graphs]] / [[graph_generate]] ; [[07_RNNs]]
Related:  [[deep_graphs]], [[graphsage]]

#graph #generative


## Setting the problem

We want to be an **opposite of a graph convolution network** (GCC, or [[graphsage]]): we want to use RNNs to go from a network representation to a graph. We'll use RNNs for that.

Uses for that:
* Anomaly detection (compare a real graph to simulated graphs)
* Predictions (future graph structures from the past)
* Solving algorithmic / optimization problems - if a solution can be represented as a graph, it would be nice to just generate this graph
* Graph completion (guess missing data from existing data)

Practical example: generating novel molecules.

**Why generating graphs is hard?** All same problems that were previously true for graphs-as-inputs, now apply to graphs-as-outputs:
* Output space (A matrix) **dimensions are variable**. So we cannot produce it as an output of a  network.
* Output space is **large**:  n² elements, which grows super-fast, but also for IRL these outputs may be ginormous.
* **Non-unique representation**: an n-node graph can be represented in n! ways, making objective functions (something to optimize) a nightmare. But we need an objective function if we want to train a network!
* Complex **long-range dependencies**: say, if we care about cycles length k, the network should be able to "count to k", so that it wouldn't close the cycle too early, or too late. In extreme case, an existence of an edge may depend on the entire graph.

## Generation as a stats problem

The way we'll be thinking about it is that there's a **distribution of graphs**, and we are sampling from it. So we want to solve a reverse problem, and create a model that generates data with same statistical properties as what is observed in the training set.
* p_data (x) - actual underlying distribution that is never known to us
* {x} - a set of data points (graphs) that was observed
* p_model (x, θ) - a model parametrized by θ that we'll be trying to use to approximate the unknown p_data
* Goals: 
1. make p_model close to p_data
2. learn to sample from p_model, producing points (graphs)

What does it mean for p_model to be close to p_data? **Maximum likelihood**:

$\displaystyle θ^* = \text{argmax}_θ \text{E}_{x \sim p_{data}} \log p_{model}(x|θ)$

Find parameters θ that maximizes the probability of generating observed data.

But max-likelihood assumes that the data is independent and identically distributed. Is it true? Not necessarily… _No follow-up for this statement, just dropped it._

Common approach: **decoder**, which is a fancy function $f()$ that takes a normally distributed random value z of some given dimensionality, and transforms into a data-like point $f(θ, z)$. We'll be using DL to approximate (and optimize) this function $f()$. 

**Auto-regressive models**: A distribution of final outputs is presented as a joint distribution of its parts, which in turn is factorized as a product of conditional distributions:

p_model() = $\prod$ p(new something  | all previous somethings)

Sequential  generation, where we take "actions" (add a point, a component, a word to a sentence etc) based on all previously taken actions, until the result is complete. In our case, we would be adding nodes and edges one after another.

> He makes some vague statement that other generative approaches, including [[gan]]s and [[autoencoder]]s, are somehow related to ARMs?.. Not sure. He says that "at least one part of fancier generators contains an ARM", or something like that… #todo: what??

## graphRNNs

Generating a graph node-by-node means implicitly introducing a certain ordering of nodes in a graph that we'll call π. There are many different π-s that can apply to the same graph. The generation process is now a sequence of actions $S^π$. Sequence of actions is linked to π, and two different orderings would obviously requite different sequences. We will later see that some orderings π are more productive, and can make training easier.

We will be adding nodes, one node at a time, and connect them to some of the existing nodes. A sequence of sequences: a sequence of nodes, and then for each node, a sequence of edges.

We will have two RNNs:
* The **node-level RNN**, for each node, generates an initial state for the edge-level RNN
* The **edge-level RNN** uses this state to generate the edges, and then also updates the state of the node-level RNN

How would the RNN look like?
* Two things coming in: **state** $s_{t-1}$ and **input** x_t
* Two things coming out: **updated state** $s_t$ and **output** y_t

$\displaystyle \begin{cases} s_t = σ(Wx_t + Us_{t-1}) \\ y_t = Vs_t\end{cases}$

To which we add a simple additional recurrence:

$x_t = y_{t-1}$ for $t>0$ (that is, for all x after x_0. For x_0, they use some special starting state)

> Dimensions are really unclear here. On the slide they claim that s_0 = SOS, but also x_0 = SOS that makes it seem like they have the same dimensionality, but also x=y, and y is supposedly binary (add an edge or don't add an edge). That's confusing.

> So, how does a model "know" what node "to connect to" it is considering at each step? If the input x_i is just the previous output, and not the info that was "written" at that remote node, the only way to tell what node we are connecting to is to count, and rely on the fact that we are covering one triangular half of the matrix. Like, node number 3 will always be the 3d on the list. So technically the model can count to it… It feels a bit weird tho for huge graphs. Do we really expect our model to remember that a node number 100 or 1000 was somehow special, if it is supposed to be special? It seems to me that it would have been more productive to plant some "notes for the future" (like, messages in the past) on nodes themselves, and use these messages as inputs in the future. Maybe rewrite them as well. No?

We use a special state s_0 called **Start of Sequence** (SOS) to init the process, and we'll treat one of the outputs y_T as the **End of Sequence** (EOS) signal (once EOS is produced, we stop the process, and consider the graph "done"). Actually in practice EOS is probably not used for the node-level RNN (as we probably have a pre-conceived idea for how many nodes we need), but we need it for the edge-level RNN, so that it could quit connecting a new node to past nodes when it wants to.

> Are s-SOS and x-SOS different SOSes? If so, fix (subscripts). #todo Check in the paper / code.

> Note that if EOS is one option for y, then y cannot have dimension 1 (0 or 1). At the very least, it has to have dimension 2 and a softmax. The actual dimensionality depends on the actual implementation.

More expressive cells ([[gru]], [[lstm]] etc) are also possible. And as usual, we can "unroll in time" this RNN to analyze it. Except 

> Note that technically if we honestly fill the entire matrix, then the length of unroll increases as we go along the graph, so the network becomes deeper and deeper, from 1 to N (number of nodes). We'll have to do something about it later.

But so far the system is deterministic, so it will just learn to produce one graph, while we want many different graphs from the same distribution. **To make it stochastic**, let's treat outputs y_t not as a binary classifying (edge / no edge), but as a probability. Every time y_t is output, we'll flip a weighted coin, either add an edge or not, generate a new input (either 1 or 0 depending on whether we ended up adding an edge), and feed this input back to the network. More generally (for a case of fancier y_t), we can describe this procedure in terms of sampling: new $x_t$ is a sample from $y_{t-1}$.

> It may feel a bit weird as their output is not cumulative, even locally: the model only "sees" its previous choice. But if you think of it, nothing prevents the model from remembering it's past decisions y_t, as they are fed in as x_t and transformed; it can "pack" it into the state s_t somehow, and thus remember as many past steps (be it a broad general idea of past decisions, or precise counting) as necessary.

**How to train this model?** We'll be using something called **Teacher Forcing**: during training, instead of rerouting outputs y_t to inputs x_t, we'll be using the _actual sequence_ of decisions you would have needed to produce the input graph as x_t. In other words, instead of flipping a coin, we look at what a well-trained model would have done to produce the graph (whether the edge is actually present or not), and use this value $y^*_{t-1}$ as an input $x_t$ at the next step. You "ignore the actions of a student, and just force the truth upon it". 

And that's where the ordering π could become critical, as the graph is easy to learn only if the ordering makes sense. Leskovets' example: think of a giant cycle. A topological order is good, as the rule is simple (always connect to the previous node, except the very last node that also connects to node 1). But random order would be decidedly bad. Which hints at the definition of a "good ordering": it allows the model to remember as little as possible, especially in the long-term. To minimize memory.

**Loss**: binary cross-entropy, as for any binary classifier (edge / no edge). See: [[loss]]

$\displaystyle L = -\big[ y_1^* \log(y_1) + (1-y_1^*)\log(1-y_1)\big]$where $y_1^*$ is the actual frequency of "Yes" answers to the question (should we add the edge?), and $y_1$ is the probability that the model would answer "yes" to this question.

> #todo: change notation, as y_1 is not a good notation here

Is teacher-forcing the best way to train it? Teacher-forcing obviously doesn't let the network explore, but it uses max supervised information possible.

**Backpropagation**: Essentially (due to inheritance of state $s$) we now have a VERY long RNN (up to O(N2) edges), with varied length (because of EOSes that each edge-level RNN can output), and with layers going in some randomized order (because different matrices for node-level and edge-level networks). **How to limit this complexity?** How to make it easier for the network? How to ensure that the unroll is not too deep, and the network doesn't have to remember everything?

Let's use a good node ordering, and by "good" we mean **BFS search** ordering. This way the highest number of nodes to remember back, is limited (_not sure by what???_). So instead of filling an upper triangle of the matrix, we only fill a shifted diagonal, driving complexity down. Leskovec says that this 2018 BFS approach can handle up to ~1000 nodes, and as of 2018 it was the SOTA.

> Open questions:
>* What is the worst-case scenario for a graph of N nodes? What graph would require most "deep memory" from an RNN generating it? Is it two fully connected graphs size N/2 connected with one bridge, as it would require a memory of N/2? Or is it a giant cycle length N? Or are both actually implicitly trainable even without a full roll-out? 
>* For BFS ordering π, does is matter which node we pick first? If we cannot prove which node is best, is there at least a heuristic? Is it better to start with the highest-degree node, lowest degree node, most central node, node ajacent to a [[bridge]], or something else? Is there a way to find the "best possible node", even for some simple graphs? Similarly, while doing BFS, does it matter how we order the edges for each node?
>* Given a graph of N nodes and a degree distribution p(k), with max degree K, can we give some guarantees on the depth of RNN unrolling using BFS ordering? Is there a way to restate this question to make it more informative? (Some simple statement about the network leading to a guarantee on network depth). - My impression is that the answer is no and no, because of cycles. An N-long cycle strictly speaking requires an N-deep unroll, which means that the guarantee on unroll depth can only be based on a statement about cycles, and counting cycles is hard (grows as k! for k-long cycles).
>* Is the "answer" statement above correct? Or does the "Teacher forcing" approach eliminate the need for N-long unroll?
>* Related question: is there a value in generating and training on many different orderings for the same graph? (Say, by trying different starting points for BFS) Or is it bad / useless? What kind of transfer is easy, and which one is hard? Is it easier to transfer within the same graph but for  different orderings π of it, or across different graphs, but with the node ordering precisely optimized in the same way for each graph?
>* What would be the best (even if approximate / imperfect / probabilistic) way to scale this approach up to distributed, parallel processing?

In practice, this model may also be embed into a reinforcement learning system, or into a GAN system, to rate its outputs based on some external properties of this graph.

Good test: generating grids. Generating grids is relatively hard for a network like that, and mistakes are clearly visible.

# Refs

Main publication (not yet processed here):
You, J., Ying, R., Ren, X., Hamilton, W. L., & Leskovec, J. (2018). GraphRNN: Generating realistic graphs with deep auto-regressive models. arXiv preprint arXiv:1802.08773.
https://cs.stanford.edu/people/jure/pubs/graphrnn-icml18.pdf

Lecture summary:
https://snap-stanford.github.io/cs224w-notes/machine-learning-with-networks/graph-generative-models