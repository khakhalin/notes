# Randomly wired networks for image recognition

Xie, S., Kirillov, A., Girshick, R., & He, K. (2019). Exploring randomly wired neural networks for image recognition. In Proceedings of the IEEE International Conference on Computer Vision (pp. 1284-1293).
https://arxiv.org/pdf/1904.01569v2.pdf
Facebook AI research

#archsearch

Instead of training a pre-design, they explore random connectivities. They succeed in finding connectivity patterns that have competitive accuracy on ImageNet recognition.

But these guys **train weights** after generating the networks. Unlike these guys: [[Gaier2019agnostic]]

They built a **parameterized stochastic generator** g(θ,s), where s is a random generator seed. A good combo of randomness and reproducibility. Networks were generated as DAGs, guided by several priors:
* Network is split into "cells" - each a randomized subgraph
* Each cell received all inputs (???) from 2 previous cells
* Among other things (?) each cell contained 5 nodes connected _only_ to 2 nodes (???). Which nodes - was defined by LSTM during network generation (???)
* Once everything was rando-generated, all nodes within each cell without an output were forced to output to an extra "output" node

Interesting connections with NAS (Neural Architecture Search), which is LSTM-based, and optimized by RL.

> At this point I stopped reading, for now. #halfthere

# Other fun highlights

* Some nice links on architecture search in the beginning.
* A gret figure (Fig 4) with a bunch of rando-architectures. Looks like some kids book from the 70s, about yarn. Potentially a great pic for a "confusing slide" :)