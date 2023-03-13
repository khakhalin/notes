# Catastrophic forgetting

#memory #ml #pruning #distillation #bib

Parents: [[06_DL]]
Related: [[transfer]], memory

Aka **catastrophic interference**. A tendency of AN to abruptly and completely "forget" new information upon learning new information. First discovered in about 1990. A dilemma between flexibility and ability to generalize, and sensitivity to perturbation.

Main cause is probably an overlap of hidden representations. Learning both A and B is a harder task than learning both A and B, and moreover, the learning process that would preserve A while learning B, is even less natural (must be crafted artificially).

**Approaches to battle it**, from oldest to newest:

* **Separation of inputs**: have enough capacity, and only send inputs to a subset of a network for any given input, hoping that the overlap in hidden activations won't be too big
* Forced **orthogonal input vectors** (aka force no overlap between inputs statistically)
* **Activation sharpening**: force only a subset of hidden neurons be active during learning of every consecutive "new thing". Doesn't have to be a 0/1 kind of inactivation; it may be enough to just make some neurons more active (sharpening)
* **Novelty rule**: for each consecutive task only train units that were not too active in previous tasks (that are not dedicated to useful knowledge)
* **Pre-training**: the argument is that in a good pre-trained network (pre-trained on data that is diverse enough?), internal representations will be somehow be forced to be non-exclusionary.
* Pseudo-recurrent network (?)
* Self-refreshing network - _sounds a bit like sleep / wake architecture, no_?

# To Read

#todo

Kirkpatrick, J., Pascanu, R., Rabinowitz, N., Veness, J., Desjardins, G., Rusu, A. A., ... & Hassabis, D. (2017). Overcoming catastrophic forgetting in neural networks. Proceedings of the national academy of sciences, 114(13), 3521-3526.

Kemker, R., McClure, M., Abitino, A., Hayes, T. L., & Kanan, C. (2018, April). Measuring catastrophic forgetting in neural networks. In Thirty-second AAAI conference on artificial intelligence.

# Refs

First mention: The Sequential Learning Problem: McCloskey and Cohen (1989)

Wiki has a good long article, summarizing older aproaches to this problem:
https://en.wikipedia.org/wiki/Catastrophic_interference

