# Yamins2016viscortex

Yamins, D. L., & DiCarlo, J. J. (2016). Using goal-driven deep learning models to understand sensory cortex. Nature neuroscience, 19(3), 356-365. http://brainmind.umin.jp/PDF/wt17/Yamins3.pdf

Parents: [[vision]]
Related: [[convnet]], [[KhalighRazavi2014cortex]]

#vision #review

Vision is weird, because nothing in the way the first layers work is aligned with how we perceive the world or respond to it; from colors to the fact that retinal projection is not invariant to either translation, depth, lighting, or deformation, yet our perception is. We need some fancy non-linear transformations to explain vision. And while DL may help here, to count as an "explanation" for vision, it should also be at least to some extent mappable on the architecture of the brain.

An archetypical architecture loosely based on Hubel and Wiesel experiments: **HCNN**: **Hierarchical Convolutional Neural Network**. A bunch of them can be stacked as "layers" (in a brain sense). 

Footnote: referencing classic LeCun Bengio work from the 1990s:
* LeCun, Y. & Bengio, Y. Convolutional networks for images, speech, and time series. in The Handbook of Brain Theory and Neural Networks 255–258 (MIT Press, 1995).

Primate brain has at least 8 "large-scale layers" like that: Retina → LGN → V1 → V2 → V4 → PIT → CIT → AIT (here **IT** stands for **Inferior Temporal** cortex). Except starting with V1 there are also within-layer recurrent connections, and all the way from AIT to LGN there are not only bottom-up, but also top-down connections.

In convnets, each HCNN meta-layer performs non-linear filtering and transformation, typically including a conv-layer, a non-linear activation function, some sort of normalization, and pooling. The locality of these operations is a nice parallel to the brain, although weight-sharing is of course an approximation; in the brain weight-similarity has to naturally arise from architecture parallelization and translation invariance. (Except that in the brain, there's also a "fovea bias").

**History**: early (pre-convnet) models were explicitly Hubel-Wiesel inspired, with manual Gabor filters. But then filters were parametrized, and parameters learned. In late 1990s [[hmax]] was developed, with explicitly defined simple cells, complex cells, composite and "complex composite" features, etc. However HMAX turned to be not expressive enough (failed to replicate activation of cells in IT area), while deep dense networks overfitted training data and wouldn't generalize. Also it's hard to fit these models to real data (and thus debug), as real data is relatively scarce.

**How to map DL to Neuro?**
1. **Task information consistency**: decision borders and other stats produced by a DL model (a simple linear classifier sitting on top of the vision model) should replicate those recorded in a brain, or observed behavioral responses (in humans, monkeys)
2. **Population representation consistency**: build a similarity matrix, compare.
3. **Single-unit response prodictivity**: try to predict activity of biological neurons from activity of neurons in the DL model (as simple linear combinations). Measure accuracy of this prediction. If this is high, we consider layers similar. This also works for mapping neurons between animals, or between different DL models.

**Goal-driven networks**: replicate the drive (evolutionary?) behind network creation, and it is likely to converge on a solution similar to one found by bio brains. It turns out that last layers of convnets (HCNNs) are a good model of primate IT (in terms of single unit response prediction, as described above). Moreover, the better a convnet is in prediction the closer its final layer is to IT. Yet it's not just about reconstruction quality, as a "magic" "perfect observer" (one neuron per category, fired directly by learning labels)is not a good model of IT, even though by design it's a perfect image classifier (cheater). As a model trains to label, it also converges closer to IT-like activations in its last layer. Population response consistency is also good (see also [[KhalighRazavi2014cortex]]). While early layers of convnets are good models of V1, while later along the convnet this resemblance decreases

How to understand networks? What does it even mean, to "understand" a network? (see [[interpretability]], [[Lillicrap2019understand]])
* Optimal stimuli
* Perturbation analysis

As main **caveat**, this is all about feed-forward networks, but IRL networks are not feed-forward, but recurrent, one way or another.
