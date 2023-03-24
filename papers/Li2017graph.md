# Gated graph sequence neural networks

Li, Y., Tarlow, D., Brockschmidt, M., & Zemel, R. (2015). Gated graph sequence neural networks. arXiv preprint arXiv:1511.05493.

Related: [[09_Graphs]], [[gru]]

#rnn #graphs


A graph analysis network on GRU units.

Two matrices: one adjacency matrix, another one to encode edge types. For every "real" (meaningful) edge type, we also automatically generate an "opposite" edge type, pointing in the opposite direction.

#halfthere