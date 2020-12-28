# Benchmarking for RNN sequence modeling

Bai, S., Kolter, J. Z., & Koltun, V. (2018). An empirical evaluation of generic convolutional and recurrent networks for sequence modeling. arXiv preprint arXiv:1803.01271.
https://arxiv.org/pdf/1803.01271.pdf

Github with all tasks and datasets (or an easy way to download the datasets): https://github.com/locuslab/TCN

#rnn #benchmarks #convnet #review #timeseries

Parent: [[07_RNNs]], [[convnet]]
Related: [[Breuel2015benchmark]], [[mnist]]


Almost 1k citations in 2 years, which is very good. Here they compare RNNs and ConvNets on a set of benchmarks that are standard for RNNs (they call it "RNN home turf"). Use TCN (Temporal Convolution Network) abbreviation as a "generic convnet" to compete against RNNs. The main conclusion is that **ConvNets consistently outperform RNNs**

Really nice overview of **temporal convolution networks** with residual blocks (see [[residual]]).

Benchmarks offered (all with references):

#### The Adding problem

A sequence of random values from {0, 1}. But each value presented as a length-2 vector, where the 2nd element is always 0. Except for 2 values, picked at random, for which the 2nd element is 1. The network should learn to sum these two randomly picked elements and output the result.

#### Copy Memory

Input sequence: 10 elements from {1.. 8}, followed by T zeros, followed by 11 symbols 9. Output sequence: T+10+1 zeros, followed by 10 elements from the input sequence, repeated. So the model needs to learn to remember the sequence of these 10 elements for increasingly long periods of time T (like the delayed word repetition task from psychology).

#### Sequential MNIST and Permuted Sequential MNIST

In sequential mnist, you just feed pixels of pictures from the MNIST dataset ([[mnist]]), one after another, resulting in a 784-long sequence. In permuted sequential mnist (P-MNIST), "the order of the sequence is permuted at random" (_what???_), and it is obviously harder.

#### JSB Chorales and Nottingham polyphonic music

**JSB**: Entire corpus of 382 four-part chorales by Bach. 88-bit binaries, corresponding to keys on piano.

**Nottingham Music Database** (or NMB): 1200 American and British folk tunes (also polyphonic)

#### PennTreebank

Can be used either at word-level (10k vocab, 888k train set, 70k validation, 79k testing), or char-level (harder: 5000k train, 400k validation, 450k testing). A relatively small, well-studied dataset.

#### Wikitext-103

World-level; about 100 times larger than Penn, and a huge vocab (270k).

#### LAMBADA

World-level; 10k passages from novels, each one 4-5 sentences long. Most models fail on this dataset, as to a human a missing word in one of the sentence is easy to understand within a context of other sentences, but not without them. Can therefore be used to measure model's ability ot "understand the context".

#### text8

Char-level; ~20 times larger than Penn; 27 unique alphabets.

