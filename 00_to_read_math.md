# Things to read: ML and AI

Rahaman, N., Baratin, A., Arpit, D., Draxler, F., Lin, M., Hamprecht, F. A., ... & Courville, A. (2018). On the spectral bias of neural networks. arXiv preprint arXiv:1806.08734.
https://arxiv.org/abs/1806.08734
On what neural network can encode easily - seems to be a very useful read, potentially.

Hu, J., Shen, L., & Sun, G. (2018). Squeeze-and-excitation networks. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 7132-7141).
https://arxiv.org/abs/1709.01507
Another paper with 1600 citations.

On evolving network architectures:
https://ai.googleblog.com/2018/03/using-evolutionary-automl-to-discover.html

Press, O., Smith, N. A., & Levy, O. (2019). Improving Transformer Models by Reordering their Sublayers. arXiv preprint arXiv:1911.03864.
https://ofir.io/sandwich_transformer.pdf
An interesting tiny paper where all they did, I think, is play with the sequence of 2 common blocks, to show that the optimal sequence is not what everyone expected. May be neat. But I prob need to understand how transformers work to get it.

"Compressive Transformers for Long-Range Sequence Modelling"
by Jack W. Rae, Anna Potapenko, Siddhant M. Jayakumar, Timothy P. Lillicrap (at DeepMind)
https://arxiv.org/abs/1911.05507

Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., ... & Polosukhin, I. (2017). Attention is all you need. In Advances in neural information processing systems (pp. 5998-6008).
Transformers network
https://papers.nips.cc/paper/7181-attention-is-all-you-need.pdf

https://arxiv.org/abs/1911.04252
Self-training with Noisy Student improves ImageNet classification Qizhe Xie, Eduard Hovy, Minh-Thang Luong, Quoc V. Le
Something weird semi-supervised learning with noisy teachers and distillation. Essentially, it seems that a badly labeled large dataset is better than a well-labeled small dataset, so it's better to train one model on a small dataset, then have it label a huge dataset (even tho many labels will be wrong), and then use this large dataset to train the next model. Or something like that. Weird.

FaceNet: A Unified Embedding for Face Recognition and Clustering Florian Schroff, Dmitry Kalenichenko, James Philbin
About triplet loss and representation optimization.

Interesting project on automatic data augmentation, and generation of strong labels from a bunch of weak labels:
[https://www.snorkel.org/](<https://www.snorkel.org/>)
Find some papers they published? Cool terms mentioned on the website: automatic data augmentation, identifying critical data subsets, slicing functions

## RL

Vinyals, O., Babuschkin, I., Czarnecki, W. M., Mathieu, M., Dudzik, A., Chung, J., ... & Oh, J. (2019). Grandmaster level in StarCraft II using multi-agent reinforcement learning. Nature, 1-5.
No open link?

## Swarming

O’Keeffe, K. P., Hong, H., & Strogatz, S. H. (2017). Oscillators that sync and swarm. Nature communications, 8(1), 1504.
https://www.nature.com/articles/s41467-017-01190-3
About collective self-organized behaviors. Can be useful for the modeling class maybe?

## Generative

https://arxiv.org/abs/1905.01164 
SinGAN: Learning a Generative Model from a Single Natural Image Tamar Rott Shaham, Tali Dekel, Tomer Michaeli

Reinke, C., Etcheverry, M., & Oudeyer, P. Y. (2019). Intrinsically Motivated Exploration for Automated Discovery of Patterns in Morphogenetic Systems. arXiv preprint arXiv:1908.06663.
arxiv.org/abs/1908.06663
Apparently used an intrinsically motivated AI to explore Game Of Life to find cool patterns. The most interesting part (judging from the abstract) is that it developed a non-obvious representation, just looking for novelty (or whatever it used: looking for something _interesting_). Would be super-cool to try it for my networks, huh?

## Meanings

Marcus, G. (2018). Innateness, alphazero, and artificial intelligence. arXiv preprint arXiv:1801.05667.
[https://arxiv.org/ftp/arxiv/papers/1801/1801.05667.pdf](<https://arxiv.org/ftp/arxiv/papers/1801/1801.05667.pdf>)

Cardon, D., Cointet, J. P., & Mazieres, A. (2018). Neurons spike back: The Invention of Inductive Machines and the Artificial Intelligence Controversy.
[https://neurovenge.antonomase.fr/NeuronsSpikeBack.pdf](<https://neurovenge.antonomase.fr/NeuronsSpikeBack.pdf>)

## Graph

Dehmamy, N., Barabási, A. L., & Yu, R. (2019). Understanding the Representation Power of Graph Neural Networks in Learning Graph Topology. arXiv preprint arXiv:1907.05008.
https://arxiv.org/abs/1907.05008
