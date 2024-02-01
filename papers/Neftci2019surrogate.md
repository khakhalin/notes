# Surrogate gradient learning in spiking networks

Neftci, E. O., Mostafa, H., & Zenke, F. (2019). Surrogate gradient learning in spiking neural networks: Bringing the power of gradient-based optimization to spiking neural networks. IEEE Signal Processing Magazine, 36(6), 51-63. https://arxiv.org/pdf/1901.09948.pdf

Parents: [[neuromorphic]], [[credit]], [[rnn]]
Related: [[Zenke2021neuromorphic]]

#spiking #credit #neuromorphic


They use the term RNN only for conventional ("classic") recurrent network, and call their networks **SNNs** (for **Spiking**). Then differentiate between Recurrent Networks broadly, and **Recurrently Connected Networks**(RCNNs). The difference is that a feed-forward network of internally-recurrent (stateful) elements will also exhibit recurrent activity, as recent activity will shape its future inputs. _I'm guessing they are doing it because spiking networks are both: they may be recurrently connected, but also neurons have a state (voltage, at the very least)._

#halfthere

(abandoned for now)