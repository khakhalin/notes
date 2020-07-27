# Deep graph generation with graphRNN

#graphs #generative

Parents: [[09_Graphs]] / [[graph_generate]]
Related:  [[deep_graphs]], [[graphsage]], [[07_RNNs]]

Video markup:
* 1:25 - lecture proper begins, review of last week
* 4:35 - defining the task, setting the goals
        * 10:44 - why generating graphs is hard
* 14:56 - setting the problem in terms of sampling and distributions
* 30:30 - GraphRNNs
* 54:30 - putting things together, lots of answering questions, then tractability
* 1:12:11 - application to generating candidate drug molecules
* 1:18 - over

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

> #todo: type this formula

Find parameters θ that maximizes the probability of generating observed data.

But max-likelihood assumes that the data is independent and identically distributed. Is it true? Not necessarily… _No follow-up for this statement, just dropped it._

Common approach: **decoder**, which is a fancy function $f()$ that takes a normally distributed random value z of some given dimensionality, and transforms into a data-like point $f(θ, z)$. We'll be using DL to approximate (and optimize) this function $f()$. 

**Auto-regressive models**: A distribution of final outputs is presented as a joint distribution of its parts, which in turn is factorized as a product of conditional distributions:

p_model() = $\prod$ p(new something  | all previous somethings)

Sequential  generation, where we take "actions" (add a point, a component, a word to a sentence etc) based on all previously taken actions, until the result is complete. In our case, we would be adding nodes and edges one after another.

> He makes some vague statement that other generative approaches, including GANs and VAE, are somehow related to ARMs?.. Not sure. He says that "at least one part of fancier generators contains an ARM", or something like that… #todo: what??

## graphRNNs

Generating a graph node-by-node means implicitly introducing a certain ordering of nodes in a graph that we'll call π. There are many different π-s that can apply to the same graph. The generation process is now a sequence of actions $S^π$. Sequence of actions is linked to π, and two different orderings would obviously requite different sequences. We will later see that some orderings π are more productive, and can make training easier.

We will be adding nodes, one node at a time, and connect them to some of the existing nodes. A sequence of sequences: a sequence of nodes, and then for each node, a sequence of edges.

We will have two RNNs:
* The **node-level RNN**, for each node, generates an initial state for the edge-level RNN
* The **edge-level RNN** uses this state to generate the edges, and then also updates the state of the node-level RNN

41:41