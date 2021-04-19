# Recurrent Neural Networks

#rnn #bib

Subtopics:
* [[lstm]] - Long Short-Term Memory
* [[bptt]] - back-propagation through time
* [[gru]] - Gated Recurrent Unit (kinda a simplified version of LSTM)
* [[boltzmann_machine]] - a famous older model
* [[hopfield]] - another famous older model
* [[echo]] - Echo-State Networks (Reservoir computing)

Papers:
* ?

Benchmarking of RNNs:
* [[Bai2018sequence]] - a review of sequence modeling benchmarking
* [[Breuel2015benchmark]] - on hyperparameters (LSTM RNNs are sensitive to learning rate, but not batch)

# Bibliography

https://distill.pub/2019/memorization-in-rnns/

Cooijmans, T., Ballas, N., Laurent, C., Gülçehre, Ç., & Courville, A. (2016). Recurrent batch normalization. arXiv preprint arXiv:1603.09025.
https://arxiv.org/pdf/1603.09025.pdf
Describe how to do batch-norm in RNNs. Is it like inhibition, essentially? ~300 citations (good)

Attention and Augmented Recurrent Neural Networks: https://distill.pub/2016/augmented-rnns/

Linsley, D., Ashok, A. K., Govindarajan, L. N., Liu, R., & Serre, T. (2020). Stable and expressive recurrent vision models. arXiv preprint arXiv:2005.11362.
https://arxiv.org/abs/2005.11362
Claim to have modeled human-like visual system, and that "back-propagation in time" is bad, but they have found something better.
Tweetprint: https://twitter.com/DrewLinsley/status/1275109943792852997