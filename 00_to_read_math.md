# To-read: ML and AI

http://www.maths.qmul.ac.uk/~gbianconi/LTCCModule
A series of lectures on networks, and while it's on networks, and not ML, it's prob a priority read, just because it looks so cool and accessible (at least in the beginning).

Rahaman, N., Baratin, A., Arpit, D., Draxler, F., Lin, M., Hamprecht, F. A., ... & Courville, A. (2018). On the spectral bias of neural networks. arXiv preprint arXiv:1806.08734.
https://arxiv.org/abs/1806.08734
On what neural network can encode easily - seems to be a very useful read, potentially.

Lipton, Z. C., Berkowitz, J., & Elkan, C. (2015). A critical review of recurrent neural networks for sequence learning. arXiv preprint arXiv:1506.00019.
Great review for learning about RNNs (could be a textbook)

Finn, C., Abbeel, P., & Levine, S. (2017, August). Model-agnostic meta-learning for fast adaptation of deep networks. In Proceedings of the 34th International Conference on Machine Learning-Volume 70 (pp. 1126-1135). JMLR. org.
https://arxiv.org/pdf/1703.03400.pdf
About few-shots learning, and generalizing from a very limited number of labels? More than 1000 citations!

FaceNet: A Unified Embedding for Face Recognition and Clustering Florian Schroff, Dmitry Kalenichenko, James Philbin
About triplet loss and representation optimization.

Hu, J., Shen, L., & Sun, G. (2018). Squeeze-and-excitation networks. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 7132-7141).
https://arxiv.org/abs/1709.01507
Another paper with 1600 citations.

On evolving network architectures:
https://ai.googleblog.com/2018/03/using-evolutionary-automl-to-discover.html

Interesting project on automatic data augmentation, and generation of strong labels from a bunch of weak labels:
[https://www.snorkel.org/](<https://www.snorkel.org/>)
Find some papers they published? Cool terms mentioned on the website: automatic data augmentation, identifying critical data subsets, slicing functions

## Transfer, distillation, tickets

Frankle, J., & Carbin, M. (2018). The lottery ticket hypothesis: Finding sparse, trainable neural networks. arXiv preprint arXiv:1803.03635.
https://arxiv.org/abs/1803.03635
Original paper presenting the lottery #ticket hypothesis.

Self-training with Noisy Student improves ImageNet classification Qizhe Xie, Eduard Hovy, Minh-Thang Luong, Quoc V. Le
https://arxiv.org/abs/1911.04252
Something weird semi-supervised learning with noisy teachers and distillation. Essentially, it seems that a badly labeled large dataset is better than a well-labeled small dataset, so it's better to train one model on a small dataset, then have it label a huge dataset (even tho many labels will be wrong), and then use this large dataset to train the next model. Or something like that. Weird.

Tian, Y., Jiang, T., Gong, Q., & Morcos, A. (2019). Luck Matters: Understanding Training Dynamics of Deep ReLU Networks. arXiv preprint arXiv:1905.13405.
https://arxiv.org/pdf/1905.13405.pdf
To understand how "lucky tickets" work, they train a larger network from a smaller network, then study activation.

Morcos, A. S., Yu, H., Paganini, M., & Tian, Y. (2019). One ticket to win them all: generalizing lottery ticket initializations across datasets and optimizers. arXiv preprint arXiv:1906.02773.
https://arxiv.org/abs/1906.02773
New paper about whether "lucky tickets" generalize across datasets.

## Attention

Press, O., Smith, N. A., & Levy, O. (2019). Improving Transformer Models by Reordering their Sublayers. arXiv preprint arXiv:1911.03864.
https://ofir.io/sandwich_transformer.pdf
An interesting tiny paper where all they did, I think, is play with the sequence of 2 common blocks, to show that the optimal sequence is not what everyone expected. May be neat. But I prob need to understand how transformers work to get it.

"Compressive Transformers for Long-Range Sequence Modelling"
by Jack W. Rae, Anna Potapenko, Siddhant M. Jayakumar, Timothy P. Lillicrap (at DeepMind)
https://arxiv.org/abs/1911.05507

Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., ... & Polosukhin, I. (2017). Attention is all you need. In Advances in neural information processing systems (pp. 5998-6008).
Transformers network
https://papers.nips.cc/paper/7181-attention-is-all-you-need.pdf

Single Headed Attention RNN: Stop Thinking With Your Head
https://arxiv.org/abs/1911.11423
Looks somewhat advanced (from the technical, not math pov), but if I get it right, they tried to diversify the field by deliberately sticking to a non-mainstream approach (something that is NOT transformers), and got good performance.

## RL

Behavior Regularized Offline Reinforcement Learning
Yifan Wu, George Tucker, Ofir Nachum
https://arxiv.org/abs/1911.11361
Most horrible type of learning: offline (model of the world instead of the real world) and reinforcement. Some sort of a review?

Vinyals, O., Babuschkin, I., Czarnecki, W. M., Mathieu, M., Dudzik, A., Chung, J., ... & Oh, J. (2019). Grandmaster level in StarCraft II using multi-agent reinforcement learning. Nature, 1-5.
No open link?

Yu, H., Edunov, S., Tian, Y., & Morcos, A. S. (2019). Playing the lottery with rewards and multiple languages: lottery tickets in RL and NLP. arXiv preprint arXiv:1906.02768.
https://arxiv.org/abs/1906.02768
Lottery tickets in RL domain.

## GANs and generation

Original StyleGan?
https://arxiv.org/abs/1812.04948
Karras, T., Laine, S., & Aila, T. (2019). A style-based generator architecture for generative adversarial networks. In Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (pp. 4401-4410).

Contrastive Learning of Structured World Models
Thomas Kipf, Elise van der Pol, Max Welling
[https://arxiv.org/abs/1911.12247](<https://arxiv.org/abs/1911.12247>)
Not sure if filed correctly, but they claim to identify objects in an unsupervised manner using some tricks from graph networks. What??

Lin, Z., Thekumparampil, K. K., Fanti, G., & Oh, S. (2019). InfoGAN-CR: Disentangling Generative Adversarial Networks with Contrastive Regularizers. arXiv preprint arXiv:1906.06034.
https://arxiv.org/abs/1906.06034
Separating latent vectors (aka "disentanglement"). Apparently something that we know how to do in VAE, here applied to GANs?

https://arxiv.org/abs/1905.01164 
SinGAN: Learning a Generative Model from a Single Natural Image Tamar Rott Shaham, Tali Dekel, Tomer Michaeli

Reinke, C., Etcheverry, M., & Oudeyer, P. Y. (2019). Intrinsically Motivated Exploration for Automated Discovery of Patterns in Morphogenetic Systems. arXiv preprint arXiv:1908.06663.
arxiv.org/abs/1908.06663
Apparently used an intrinsically motivated AI to explore Game Of Life to find cool patterns. The most interesting part (judging from the abstract) is that it developed a non-obvious representation, just looking for novelty (or whatever it used: looking for something _interesting_). Would be super-cool to try it for my networks, huh?

## Meanings

Marcus, G. (2018). Innateness, alphazero, and artificial intelligence. arXiv preprint arXiv:1801.05667.
[https://arxiv.org/ftp/arxiv/papers/1801/1801.05667.pdf](<https://arxiv.org/ftp/arxiv/papers/1801/1801.05667.pdf>)

Fodor, J. A., & Pylyshyn, Z. W. (1988). Connectionism and cognitive architecture: A critical analysis. Cognition, 28(1-2), 3-71.
[pdf on rutgers edu](http://ruccs.rutgers.edu/images/personal-zenon-pylyshyn/proseminars/Proseminar13/ConnectionistArchitecture.pdf)
Seminal old paper (5k refs) against neural networks :) that everybody reference.

Cardon, D., Cointet, J. P., & Mazieres, A. (2018). Neurons spike back: The Invention of Inductive Machines and the Artificial Intelligence Controversy.
[https://neurovenge.antonomase.fr/NeuronsSpikeBack.pdf](<https://neurovenge.antonomase.fr/NeuronsSpikeBack.pdf>)

## Graph networks

Dehmamy, N., Barabási, A. L., & Yu, R. (2019). Understanding the Representation Power of Graph Neural Networks in Learning Graph Topology. arXiv preprint arXiv:1907.05008.
https://arxiv.org/abs/1907.05008

NetGAN: Generating Graphs via Random Walks
A. Bojchevski, O. Shchur, D. Zugner, S. Gunnemann.
ArXiv e-prints. 2018.

## Code to read

Reimplementation of StyleGAN from scratch in TF 2.0. Seem to be well-written, and worth studying:
https://github.com/manicman1999/StyleGAN-Tensorflow-2.0