# Pruning by conserving synaptic flow

Finding lottery tickets without training, by looking at "Synaptic flow":
Hidenori Tanaka, Daniel Kunin, Daniel L. K. Yamins, Surya Ganguli
Pruning neural networks without any data by iteratively conserving synaptic flow
https://arxiv.org/abs/2006.05467

#pruning #init #ticket

Coverage:
* Tweetprint: https://twitter.com/Hidenori8Tanaka/status/1271709221206151168
* Explanation in a youtube video (44 min!): https://www.youtube.com/watch?v=8l-TDqpoUQs

The paper is not too technical, but is kinda hard to read. This video explanation (above) though is very good.

The paper essentially restates pruning in terms of initialization. It claims (and shows) that lottery ticket is good not because it somehow "matches the data", but because it is just a "good part" of the initialization! I wonder if there could be parallels with homeostatic / spontaneous activity-driven processes in the biological brain.

As they want to learn to cut a given % of all connections, they introduce the concept of **layer collapse**: pruning so much that the input is no longer connected to the output, and training becomes impossible. Then  introduce a **Maximal Critical Compession**  that is basically "the maximal" number of weights that can be removed without ruining (collapsing) the network. What they show is that other algorithms induce layer collapse much earlier than this "maximal compression", while their new proposed algorithm never ruins a network before it needs to be ruined (Fig 1). (And I guess what they imply is that if it's true for the final ruining of the network, it may also be true for the intermediate stages, in the sense that their algorithm would produce a more functional network if a given share of connections were to be removed, compared to other algorithms.)

Note that current "standard approach to pruning" is to perform **iterative magnitude pruning** (prune a bit, retrain, repeat). So they show that at least in a one-shot situation, and for strong pruning (when only ~1% or less of weights are retained) their method works better. Two other methods, **SNIP** and **GraSP**, work better than magnitude pruning, but worse than their new method.

**Synaptic Saliency** is defined as $\frac{∂R}{∂θ} \odot θ$, where R is some (any) scalar loss function for the network output. Existing pruning methods can be described in these terms. Then they show that regardless of your init, and regardless how how you train the network, synaptic saleiency is **conserved** in two surpsing (in their own words) ways:
* **Neuron-wise conservation**. For a FF network with ReLUs (or leaky, or linear), for every hidden neuron in this network, the Σ of synaptic saliencies to it = the Σ of synaptic saliences from it.
* And because it is true, synaptic saliency is also conserved for **layers**, and thus also for the **entire network** (as long as you pick a subset of units that, if deleted, disconnects a network: kinda a generalization of a layer).

As the saliency is conserved, any algorithm that ranks weights for pruning by saliency, will butcher wider layers first (more connections ⇒ higher saliency), to the point that wide layers may be disconnected by mistake, while narrower layers will be preserved. This is true even for magnitude pruning, which may seem to be random, but actually is not, as modern initialization schemes assign higher variance to smaller layers (see [[He2015init]]). They show that this "large layers are more vulnerable" statement is true both theoretically, and with experiments (Fig 2-3).

One good solution to it is, again, to perform **iterative magnitude pruning**, which is what most people now understand by "pruning" (aka "lottery ticket"). If you do stuff iteratively, gradient descent redistributes importance (influence) among remaining edges, helping to prevent premature layer collapse. Even two iterations almost do the trick, and with three iterations the "collapse" curve (the decline of performance with pruning) becomes quite smooth. Training equalizes saliency.

But it's not the only way. What they say should be done, is just to re-evaluate saliency after each parameter is pruned. They call it **SynFlow**. What they do is to pick a very simplistic loss: a sum of all outputs of a network that got all-ones as an input, and that has abs(θ_i) instead of each θ_i as parameters. It's a weird loss (R), but they backprop it to get ∂R/∂θ, and calculate the synaptic saliency. Without any training.

> Their formula (5) is written really weirdly, and it's almost impossible to believe that is expresses the simple operation that is described here, but apparently it does! Their notation is jut really obfuscatory.