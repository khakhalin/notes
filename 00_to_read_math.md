# To-read: ML and AI

http://www.maths.qmul.ac.uk/~gbianconi/LTCCModule
A series of lectures on networks, and while it's on networks, and not ML, it's prob a priority read, just because it looks so cool and accessible (at least in the beginning).

Rahaman, N., Baratin, A., Arpit, D., Draxler, F., Lin, M., Hamprecht, F. A., ... & Courville, A. (2018). On the spectral bias of neural networks. arXiv preprint arXiv:1806.08734.
https://arxiv.org/abs/1806.08734
On what neural network can encode easily - seems to be a very useful read, potentially.

Belkin, M., Hsu, D., Ma, S., & Mandal, S. (2019). Reconciling modern machine-learning practice and the classical bias–variance trade-off. Proceedings of the National Academy of Sciences, 116(32), 15849-15854.
https://arxiv.org/abs/1812.11118

Stone, J. V. (2018). Information Theory: A Tutorial Introduction. arXiv preprint arXiv:1802.05968.
https://arxiv.org/abs/1802.05968

Your Classifier is Secretly an Energy Based Model and You Should Treat it Like One
Will Grathwohl, Kuan-Chieh Wang, Jörn-Henrik Jacobsen, David Duvenaud, Mohammad Norouzi, Kevin Swersky
https://arxiv.org/abs/1912.03263

Metz, L., Maheswaranathan, N., Cheung, B., & Sohl-Dickstein, J. (2018). Meta-Learning Update Rules for Unsupervised Representation Learning. arXiv preprint arXiv:1804.00222.
https://arxiv.org/abs/1804.00222

Leibo, J. Z., Hughes, E., Lanctot, M., & Graepel, T. (2019). Autocurricula and the emergence of innovation from social interaction: A manifesto for multi-agent intelligence research. arXiv preprint arXiv:1903.00742.
https://arxiv.org/pdf/1903.00742.pdf
Something related to their multi-agent simulation (hide-and-seek). Seems very promising.

Proximal Policy Optimization by OpenAI:
https://openai.com/blog/openai-baselines-ppo/
Something for agent training?

Finn, C., Abbeel, P., & Levine, S. (2017, August). Model-agnostic meta-learning for fast adaptation of deep networks. In Proceedings of the 34th International Conference on Machine Learning-Volume 70 (pp. 1126-1135). JMLR. org.
https://arxiv.org/pdf/1703.03400.pdf
About few-shots learning, and generalizing from a very limited number of labels? More than 1000 citations!

FaceNet: A Unified Embedding for Face Recognition and Clustering Florian Schroff, Dmitry Kalenichenko, James Philbin
About triplet loss and representation optimization.

Van Steenkiste, S., Chang, M., Greff, K., & Schmidhuber, J. (2018). Relational neural expectation maximization: Unsupervised discovery of objects and their interactions. arXiv preprint arXiv:1802.10353.
https://arxiv.org/abs/1802.10353
Unsupervized discovery of objects: seems interesting

Hu, J., Shen, L., & Sun, G. (2018). Squeeze-and-excitation networks. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 7132-7141).
https://arxiv.org/abs/1709.01507
Another paper with 1600 citations.

On evolving network architectures:
https://ai.googleblog.com/2018/03/using-evolutionary-automl-to-discover.html

Klein, B., & Hoel, E. (2019). Uncertainty and causal emergence in complex networks. arXiv preprint arXiv:1907.03902.
https://arxiv.org/pdf/1907.03902.pdf
Analyze how some basic network properties (?) can be derived by very basic information principles (?).

Chen, T. Q., Rubanova, Y., Bettencourt, J., & Duvenaud, D. K. (2018). Neural ordinary differential equations. In Advances in neural information processing systems (pp. 6571-6583).
https://arxiv.org/abs/1806.07366

Advani, M. S., & Saxe, A. M. (2017). High-dimensional dynamics of generalization error in neural networks. arXiv preprint arXiv:1710.03667.
https://arxiv.org/abs/1710.03667

# Why networks work?

Milne, T. (2019). Piecewise strong convexity of neural networks. In Advances in Neural Information Processing Systems (pp. 12953-12963).
https://arxiv.org/abs/1810.12805

Yang, G. (2019). Wide Feedforward or Recurrent Neural Networks of Any Architecture are Gaussian Processes. In Advances in Neural Information Processing Systems (pp. 9947-9960).
https://arxiv.org/abs/1910.12478

## Bias-variance and tickets

Arora, S., Du, S. S., Li, Z., Salakhutdinov, R., Wang, R., & Yu, D. (2019). Harnessing the Power of Infinitely Wide Deep Nets on Small-data Tasks. arXiv preprint arXiv:1910.01663.
https://arxiv.org/pdf/1910.01663.pdf

Ba, J., & Caruana, R. (2014). Do deep nets really need to be deep?. In Advances in neural information processing systems (pp. 2654-2662).
https://papers.nips.cc/paper/5484-do-deep-nets-really-need-to-be-deep.pdf
Seems to be one of the original network distillation papers (800 refs)

Frankle, J., & Carbin, M. (2018). The lottery ticket hypothesis: Finding sparse, trainable neural networks. arXiv preprint arXiv:1803.03635.
https://arxiv.org/abs/1803.03635
Original paper presenting the lottery #ticket hypothesis.

Zhou, H., Lan, J., Liu, R., & Yosinski, J. (2019). Deconstructing lottery tickets: Zeros, signs, and the supermask. arXiv preprint arXiv:1905.01067.
https://arxiv.org/abs/1905.01067

Tian, Y., Jiang, T., Gong, Q., & Morcos, A. (2019). Luck Matters: Understanding Training Dynamics of Deep ReLU Networks. arXiv preprint arXiv:1905.13405.
https://arxiv.org/pdf/1905.13405.pdf
To understand how "lucky tickets" work, they train a larger network from a smaller network, then study activation.

Morcos, A. S., Yu, H., Paganini, M., & Tian, Y. (2019). One ticket to win them all: generalizing lottery ticket initializations across datasets and optimizers. arXiv preprint arXiv:1906.02773.
https://arxiv.org/abs/1906.02773
New paper about whether "lucky tickets" generalize across datasets.

## Transfer, distillation

#distillation

Hinton, G., Vinyals, O., & Dean, J. (2015). Distilling the knowledge in a neural network. arXiv preprint arXiv:1503.02531.
https://arxiv.org/pdf/1503.02531.pdf
The splashy "original distillation paper" (2500 refs)

Furlanello, T., Lipton, Z. C., Tschannen, M., Itti, L., & Anandkumar, A. (2018). Born again neural networks. arXiv preprint arXiv:1805.04770.
https://arxiv.org/pdf/1805.04770.pdf
Top read!

Self-training with Noisy Student improves ImageNet classification Qizhe Xie, Eduard Hovy, Minh-Thang Luong, Quoc V. Le
https://arxiv.org/abs/1911.04252
Something weird semi-supervised learning with noisy teachers and distillation. Essentially, it seems that a badly labeled large dataset is better than a well-labeled small dataset, so it's better to train one model on a small dataset, then have it label a huge dataset (even tho many labels will be wrong), and then use this large dataset to train the next model. Or something like that. Weird.

## Attention

Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., ... & Polosukhin, I. (2017). Attention is all you need. In Advances in neural information processing systems (pp. 5998-6008).
Transformers network
https://papers.nips.cc/paper/7181-attention-is-all-you-need.pdf
Top choice

Jaderberg, M., Simonyan, K., & Zisserman, A. (2015). Spatial transformer networks. In Advances in neural information processing systems (pp. 2017-2025).
https://arxiv.org/pdf/1506.02025.pdf
2000 citations. Seems important.

Press, O., Smith, N. A., & Levy, O. (2019). Improving Transformer Models by Reordering their Sublayers. arXiv preprint arXiv:1911.03864.
https://ofir.io/sandwich_transformer.pdf
An interesting tiny paper where all they did, I think, is play with the sequence of 2 common blocks, to show that the optimal sequence is not what everyone expected. May be neat. But I prob need to understand how transformers work to get it.

"Compressive Transformers for Long-Range Sequence Modelling"
by Jack W. Rae, Anna Potapenko, Siddhant M. Jayakumar, Timothy P. Lillicrap (at DeepMind)
https://arxiv.org/abs/1911.05507

Single Headed Attention RNN: Stop Thinking With Your Head
https://arxiv.org/abs/1911.11423
Looks somewhat advanced (from the technical, not math pov), but if I get it right, they tried to diversify the field by deliberately sticking to a non-mainstream approach (something that is NOT transformers), and got good performance.

## Quasi-Symbolic

Yoshua Bengio’s short reading list
* BabyAI: First Steps Towards Grounded Language Learning With a Human In the Loop, Chevalier-Boisvert et al.,
2018: https://arxiv.org/abs/1810.08272v2.
* A Meta-Transfer Objective for Learning to Disentangle Causal Mechanisms, Bengio et al., 2019:
https://arxiv.org/abs/1901.10912.
* Learning Neural Causal Models from Unknown Interventions, Ke et al., 2019: https://arxiv.org/abs/1910.
01075.
* Recurrent Independent Mechanisms, Goyal et al., 2019: https://arxiv.org/abs/1909.10893.
* The Consciousness Prior, Bengio et al., 2017: https://arxiv.org/abs/1709.08568.

## RL

Vinyals, O., Babuschkin, I., Czarnecki, W. M., Mathieu, M., Dudzik, A., Chung, J., ... & Oh, J. (2019). Grandmaster level in StarCraft II using multi-agent reinforcement learning. Nature, 1-5.
No open link?

Freeman, D., Ha, D., & Metz, L. (2019). Learning to Predict Without Looking Ahead: World Models Without Forward Prediction. In Advances in Neural Information Processing Systems (pp. 5380-5391).
https://arxiv.org/abs/1910.13038
If you limit agents ability too observe real world to "key frames", making it more sporadic, it develops some predicting capabilities, even tho you never told it do?

Raghu, A., Raghu, M., Bengio, S., & Vinyals, O. (2019). Rapid learning or feature reuse? towards understanding the effectiveness of maml. arXiv preprint arXiv:1909.09157.
https://arxiv.org/abs/1909.09157

Behavior Regularized Offline Reinforcement Learning
Yifan Wu, George Tucker, Ofir Nachum
https://arxiv.org/abs/1911.11361
A paper about the most horrible type of learning: offline (model of the world instead of the real world) and reinforcement. Some sort of a review?

Yu, H., Edunov, S., Tian, Y., & Morcos, A. S. (2019). Playing the lottery with rewards and multiple languages: lottery tickets in RL and NLP. arXiv preprint arXiv:1906.02768.
https://arxiv.org/abs/1906.02768
Lottery tickets in RL domain.

## Autoencoders

Oord, A. V. D., Dieleman, S., Zen, H., Simonyan, K., Vinyals, O., Graves, A., ... & Kavukcuoglu, K. (2016). Wavenet: A generative model for raw audio. arXiv preprint arXiv:1609.03499.
https://arxiv.org/abs/1609.03499
1.4k citations: famous really successful realtime audio generation network from Google DeepMind.

## GANs and generation

#todo: Find original Neurips GAN paper from 2016?

Video tutorial on GANs from Ian Goodfellow (2016):
https://www.youtube.com/watch?v=HGYYEUSm-0Q

Goodfellow, I., Pouget-Abadie, J., Mirza, M., Xu, B., Warde-Farley, D., Ozair, S., ... & Bengio, Y. (2014). Generative adversarial nets. In Advances in neural information processing systems (pp. 2672-2680).
https://arxiv.org/pdf/1406.2661v1.pdf
Original paper with 14000 citations.

Original StyleGan (is it?)
https://arxiv.org/abs/1812.04948
Karras, T., Laine, S., & Aila, T. (2019). A style-based generator architecture for generative adversarial networks. In Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (pp. 4401-4410).

Reimplementation of StyleGAN from scratch in TF 2.0. Seem to be well-written, and worth studying:
https://github.com/manicman1999/StyleGAN-Tensorflow-2.0

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

Hoyer, S., Sohl-Dickstein, J., & Greydanus, S. (2019). Neural reparameterization improves structural optimization. arXiv preprint arXiv:1909.04240.
https://arxiv.org/pdf/1909.04240.pdf
They are trying to create some fancy mechanical structures, and instead of directly optimizing the parameters of these structures, they optimize the parameters of an ANN that output these structures. Claim that it works better. Looks fun.

Makhzani, A. (2018). Implicit autoencoders. arXiv preprint arXiv:1805.09804.
https://arxiv.org/abs/1805.09804
Sort of GAN-like architecture that tries to optimize the latent space directly? Interesting, but hard to get from the abstract.

## Classic RNNs

https://distill.pub/2019/memorization-in-rnns/
How LSTM networks remember text: a visual intro.

Hafner, D., Irpan, A., Davidson, J., & Heess, N. (2017). Learning hierarchical information flow with recurrent neural modules. In Advances in Neural Information Processing Systems (pp. 6724-6733).
https://papers.nips.cc/paper/7249-learning-hierarchical-information-flow-with-recurrent-neural-modules.pdf
ThalNet, which like a virtual thalamus. Only cited by 4 since 2017, so doesn't look like it caught on.

## Graph networks

David Belli (2019). Generative graph transformer
https://davide-belli.github.io/generative-graph-transformer.html
Blog post with math and pretty pics: investigate!

Dehmamy, N., Barabási, A. L., & Yu, R. (2019). Understanding the Representation Power of Graph Neural Networks in Learning Graph Topology. arXiv preprint arXiv:1907.05008.
https://arxiv.org/abs/1907.05008

NetGAN: Generating Graphs via Random Walks
A. Bojchevski, O. Shchur, D. Zugner, S. Gunnemann.
ArXiv e-prints. 2018.

## Alternative network designes

### Helmholtz Machines

Kevin G. Kirby (2006). A Tutorial on Helmholtz Machines 
https://www.nku.edu/~kirby/docs/HelmholtzTutorialKoeln.pdf

Dayan, P., Hinton, G. E., Neal, R. M., & Zemel, R. S. (1995). The helmholtz machine. Neural computation, 7(5), 889-904.
http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.34.1800&rep=rep1&type=pdf
Original paper by Hinton, 1k citations.

## Meta

Marcus, G. (2018). Innateness, alphazero, and artificial intelligence. arXiv preprint arXiv:1801.05667.
[https://arxiv.org/ftp/arxiv/papers/1801/1801.05667.pdf](<https://arxiv.org/ftp/arxiv/papers/1801/1801.05667.pdf>)

Fodor, J. A., & Pylyshyn, Z. W. (1988). Connectionism and cognitive architecture: A critical analysis. Cognition, 28(1-2), 3-71.
[pdf on rutgers edu](http://ruccs.rutgers.edu/images/personal-zenon-pylyshyn/proseminars/Proseminar13/ConnectionistArchitecture.pdf)
Seminal old paper (5k refs) against neural networks :) that everybody reference when then want to question the supposed universality of deep learning.

Cardon, D., Cointet, J. P., & Mazieres, A. (2018). Neurons spike back: The Invention of Inductive Machines and the Artificial Intelligence Controversy.
[https://neurovenge.antonomase.fr/NeuronsSpikeBack.pdf](<https://neurovenge.antonomase.fr/NeuronsSpikeBack.pdf>)