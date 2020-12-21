# Benchmarking for RNN sequence modeling

Bai, S., Kolter, J. Z., & Koltun, V. (2018). An empirical evaluation of generic convolutional and recurrent networks for sequence modeling. arXiv preprint arXiv:1803.01271.
https://arxiv.org/pdf/1803.01271.pdf

Github with all tasks and datasets (or an easy way to download the datasets): https://github.com/locuslab/TCN

#rnn #benchmarks

Parent: [[07_RNNs]], [[convnet]]
Related: [[Breuel2015benchmark]], [[mnist]]


Almost 1k citations in 2 years, which is very good. Here they compare RNNs and ConvNets on a set of benchmarks that are standard for RNNs (they call it "RNN home turf"). Use TCN (Temporal Convolution Network) abbreviation as a "generic convnet" to compete against RNNs.

The main conclusion is that **ConvNets consistently outperform RNNs**

Benchmarks offered:

#### The Adding Problem

#### Copying Memory Task

Sequential MNIST digit classification

#### Permuted Sequential MNIST

Based on Seq. MNIST, but more challenging.

#### JSB Chorales polyphonic music

#### Nottingham polyphonic music

#### PennTreebank

Small word-level language modeling and larger char-level language modeling.

## Larger text datasets:

#### Wikitext-103

World-level.

#### LAMBADA

Word-level.

#### text8

Char-level.

