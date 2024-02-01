# Benchmarking LSTM networks

Breuel, T. M. (2015). Benchmarking of LSTM networks. arXiv preprint arXiv:1508.02774.
https://arxiv.org/pdf/1508.02774.pdf

#rnn #benchmarks

Parents: [[rnn]]
See also: [[Schneider2019benchmark]], [[mnist]]

Typically (at least in 2015) RNNs were tested on MNIST and "UW3 text OCR task" (it appears to be a text where a window slides over an image with text, and thee RNN is supposed to read the text).

Hyperparameter optimization for RNNs is crucial, but relatively easy (at least on MNIST dataset). Momentum and batch size are largerly irrelevant, meaning that RNNs are parallelizeable.

**Peephole connections** (?) were not useful (in the experiments reported in this paper).

Lots of hard-to-read plots. I'm not a big fan of this paper, tbh.