# To-read: ML and AI

An overview of gradient descent optimization algorithms.  19 Jan 2016 (updated in 2018). By Sebastian Ruder.
https://ruder.io/optimizing-gradient-descent/
Adagrad, adam, and others like that.

The Unreasonable Effectiveness of Recurrent Neural Networks
http://karpathy.github.io/2015/05/21/rnn-effectiveness/

Rahaman, N., Baratin, A., Arpit, D., Draxler, F., Lin, M., Hamprecht, F. A., ... & Courville, A. (2018). On the spectral bias of neural networks. arXiv preprint arXiv:1806.08734.
https://arxiv.org/abs/1806.08734
On what neural network can encode easily - seems to be a very useful read, potentially.

He, K., Zhang, X., Ren, S., & Sun, J. (2016). Deep residual learning for image recognition. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 770-778).
http://openaccess.thecvf.com/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf
36k citations, haha. The famous residual networks paper.

Identity Crisis: Memorization and Generalization Under Extreme Overparameterization 
Chiyuan Zhang, Samy Bengio, Moritz Hardt, Michael C. Mozer, Yoram Singer
https://openreview.net/forum?id=B1l6y0VFPr
https://openreview.net/pdf?id=B1l6y0VFPr
Identity mapping (just training the output be exactly like the input).

Berseth, G., Geng, D., Devin, C., Finn, C., Jayaraman, D., & Levine, S. (2019). SMiRL: Surprise Minimizing RL in Dynamic Environments. arXiv preprint arXiv:1912.05510.
https://arxiv.org/abs/1912.05510
Blog:
https://bair.berkeley.edu/blog/2019/12/18/smirl/
Sounds extremely interesting!!

Ribeiro, M. T., Singh, S., & Guestrin, C. (2016, August). Why should i trust you?: Explaining the predictions of any classifier. In Proceedings of the 22nd ACM SIGKDD international conference on knowledge discovery and data mining (pp. 1135-1144). ACM.
https://arxiv.org/abs/1602.04938
Seems to be a classic text, on model interpretatibility (that is often critical for production)

Your Classifier is Secretly an Energy Based Model and You Should Treat it Like One Will Grathwohl, Kuan-Chieh Wang, Jörn-Henrik Jacobsen, David Duvenaud, Mohammad Norouzi, Kevin Swersky. https://arxiv.org/abs/1912.03263

Metz, L., Maheswaranathan, N., Cheung, B., & Sohl-Dickstein, J. (2018). Meta-Learning Update Rules for Unsupervised Representation Learning. arXiv preprint arXiv:1804.00222.
https://arxiv.org/abs/1804.00222

Finn, C., Abbeel, P., & Levine, S. (2017, August). Model-agnostic meta-learning for fast adaptation of deep networks. In Proceedings of the 34th International Conference on Machine Learning-Volume 70 (pp. 1126-1135). JMLR. org.
https://arxiv.org/pdf/1703.03400.pdf
About few-shots learning, and generalizing from a very limited number of labels? More than 1000 citations!

Schroff, F., Kalenichenko, D., & Philbin, J. (2015). Facenet: A unified embedding for face recognition and clustering. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 815-823).
https://arxiv.org/abs/1503.03832 
[[triplet_loss]] triplet_loss

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
Insteresting postscript a year later (20 min video from Neurips 2019; [youtube](https://www.youtube.com/watch?v=YZ-_E7A3V2w)). Lists items that were ovehyped, and cites follow-up studies that show limitations of this approach. Also a nice story of how they gradually updated the paper, to reflect the critique (including citing older and related work). Recommends as a replacement for resnets and time-series (in the future? once they make them stochastic?).

Advani, M. S., & Saxe, A. M. (2017). High-dimensional dynamics of generalization error in neural networks. arXiv preprint arXiv:1710.03667.
https://arxiv.org/abs/1710.03667

Allamanis, M., Barr, E. T., Devanbu, P., & Sutton, C. (2018). A survey of machine learning for big code and naturalness. ACM Computing Surveys (CSUR), 51(4), 81.
https://arxiv.org/pdf/1709.06182.pdf
A longish review (30 pages) on using ML to analyze programming code.

# Why networks work?

Milne, T. (2019). Piecewise strong convexity of neural networks. In Advances in Neural Information Processing Systems (pp. 12953-12963).
https://arxiv.org/abs/1810.12805

Yang, G. (2019). Wide Feedforward or Recurrent Neural Networks of Any Architecture are Gaussian Processes. In Advances in Neural Information Processing Systems (pp. 9947-9960).
https://arxiv.org/abs/1910.12478

#variance
Belkin, M., Hsu, D., Ma, S., & Mandal, S. (2019). Reconciling modern machine-learning practice and the classical bias–variance trade-off. Proceedings of the National Academy of Sciences, 116(32), 15849-15854.
https://arxiv.org/abs/1812.11118

Geman, S., Bienenstock, E., & Doursat, R. (1992). Neural Networks and the Bias/Variance Dilemma. Neural Computation.
Classical work everybody reference, even though now people are saying it may not be completely correct?

Neal, B. (2019). On the Bias-Variance Tradeoff: Textbooks Need an Update. arXiv preprint arXiv:1912.08286.
https://arxiv.org/abs/1912.08286
Blog post to accompany it: [[Neal2019blog]]

Neal, B., Mittal, S., Baratin, A., Tantia, V., Scicluna, M., Lacoste-Julien, S., & Mitliagkas, I. (2018). A modern take on the bias-variance tradeoff in neural networks. arXiv preprint arXiv:1810.08591.
https://arxiv.org/abs/1810.08591

Nakkiran, P., Kaplun, G., Bansal, Y., Yang, T., Barak, B., & Sutskever, I. (2019). Deep Double Descent: Where Bigger Models and More Data Hurt. arXiv preprint arXiv:1912.02292.
https://mltheory.org/deep.pdf
Blog description:
https://openai.com/blog/deep-double-descent/

Arora, S., Du, S. S., Li, Z., Salakhutdinov, R., Wang, R., & Yu, D. (2019). Harnessing the Power of Infinitely Wide Deep Nets on Small-data Tasks. arXiv preprint arXiv:1910.01663.
https://arxiv.org/pdf/1910.01663.pdf

Ba, J., & Caruana, R. (2014). Do deep nets really need to be deep?. In Advances in neural information processing systems (pp. 2654-2662).
https://papers.nips.cc/paper/5484-do-deep-nets-really-need-to-be-deep.pdf
Seems to be one of the original network distillation papers (800 refs)

# Interpretability

Gilboa, D., & Gur-Ari, G. (2019). Wider Networks Learn Better Features. arXiv preprint arXiv:1909.11572.
https://arxiv.org/pdf/1909.11572.pdf
train a network (they used MNIST), do UMAP of activations, find groups of neurons that together encode useful features, visualize them. Claim it to be a reasonable approach to understanding networks.

# Tickets, distillation, transfer, curriculum
#ticket

Frankle, J., & Carbin, M. (2018). The lottery ticket hypothesis: Finding sparse, trainable neural networks. arXiv preprint arXiv:1803.03635.
https://arxiv.org/abs/1803.03635
Original paper presenting the lottery ticket hypothesis.

Zhou, H., Lan, J., Liu, R., & Yosinski, J. (2019). Deconstructing lottery tickets: Zeros, signs, and the supermask. arXiv preprint arXiv:1905.01067.
https://arxiv.org/abs/1905.01067

Tian, Y., Jiang, T., Gong, Q., & Morcos, A. (2019). Luck Matters: Understanding Training Dynamics of Deep ReLU Networks. arXiv preprint arXiv:1905.13405.
https://arxiv.org/pdf/1905.13405.pdf
To understand how "lucky tickets" work, they train a larger network from a smaller network, then study activation.

Individual differences among deep neural network models (2020)
Johannes Mehrer, Courtney J. Spoerer, Nikolaus Kriegeskorte, Tim C Kietzmann
https://www.biorxiv.org/content/10.1101/2020.01.08.898288v1
Neuro-inspired: approach ANNs as one would approach individual animals. Use "Representation Similarity analysis" from neuro, changing nothing but weight init. Describe differences (pretty pics!). Claim that dropout helps.
Tweetprint: https://twitter.com/TimKietzmann/status/1215620270679044096

Morcos, A. S., Yu, H., Paganini, M., & Tian, Y. (2019). One ticket to win them all: generalizing lottery ticket initializations across datasets and optimizers. arXiv preprint arXiv:1906.02773.
https://arxiv.org/abs/1906.02773
New paper about whether "lucky tickets" generalize across datasets.

Hooker, S., Courville, A., Dauphin, Y., & Frome, A. (2019). Selective Brain Damage: Measuring the Disparate Impact of Model Pruning. arXiv preprint arXiv:1911.05248.
https://weightpruningdamage.github.io/

Maheswaranathan, N., Williams, A., Golub, M., Ganguli, S., & Sussillo, D. (2019). Universality and individuality in neural dynamics across large populations of recurrent networks. In Advances in Neural Information Processing Systems (pp. 15603-15615).
https://arxiv.org/abs/1907.08549
They trained the same network (or similar networks) thousands of times, and checked how similar the profiles are. Kind of related to both O'Leary neuro idea, and the ticket hypothesis. An interesting follow-up would be something like performance outside of the preferred distributions, and how this performance (individuality?) is distributed itself, and whether some architectures tend to produce clusters rather then smooth distributions (personality?)
Tweetprint:
https://twitter.com/SussilloDavid/status/1153427790672171009

#distillation #transfer

Hinton, G., Vinyals, O., & Dean, J. (2015). Distilling the knowledge in a neural network. arXiv preprint arXiv:1503.02531.
https://arxiv.org/pdf/1503.02531.pdf
The splashy "original distillation paper" (2500 refs)

Furlanello, T., Lipton, Z. C., Tschannen, M., Itti, L., & Anandkumar, A. (2018). Born-again neural networks. arXiv preprint arXiv:1805.04770.
https://arxiv.org/pdf/1805.04770.pdf
Top read!

Self-training with Noisy Student improves ImageNet classification Qizhe Xie, Eduard Hovy, Minh-Thang Luong, Quoc V. Le
https://arxiv.org/abs/1911.04252
Something weird semi-supervised learning with noisy teachers and distillation. Essentially, it seems that a badly labeled large dataset is better than a well-labeled small dataset, so it's better to train one model on a small dataset, then have it label a huge dataset (even tho many labels will be wrong), and then use this large dataset to train the next model. Or something like that. Weird.

von Oswald, J., Henning, C., Sacramento, J., & Grewe, B. F. (2019). Continual learning with hypernetworks. arXiv preprint arXiv:1906.00695.
https://arxiv.org/abs/1906.00695
If it get it right from the abstract, this can be called meta-networks as well: a network that learns to predict weights for a network that would follow a task; so one step above learning the weights for each individual task. Essentially, instead of remembering all possible network configurations for all possible tasks, to reset the network each time, they use this hypernetwork to interpolate in the space of parameters.

#bayesian

Fort, S., Hu, H., & Lakshminarayanan, B. (2019). Deep Ensembles: A Loss Landscape Perspective. arXiv preprint arXiv:1912.02757. 
https://arxiv.org/pdf/1912.02757.pdf
How Bayesian networks help to understand ensemble effects in deep learning (as they learn the distributino of parameters instead of instances?). Some lovely, inspiring pictures.

#curriculum

Wang, T., Zhu, J. Y., Torralba, A., & Efros, A. A. (2018). Dataset Distillation. arXiv preprint arXiv:1811.10959.
https://arxiv.org/pdf/1811.10959.pdf
10 images give 94% accuracy on MNIST.

Generative Teaching Networks: Accelerating Neural Architecture Search by Learning to Generate Synthetic Training Data.
Felipe Petroski Such, Aditya Rawal, Joel Lehman, Kenneth O. Stanley, Jeff Clune
https://arxiv.org/abs/1912.07768
Blog post with a rather detailed description and commentary:
https://eng.uber.com/generative-teaching-networks/

Panagiotatos, G., Passalis, N., Iosifidis, A., Gabbouj, M., & Tefas, A. (2019, September). Curriculum-based Teacher Ensemble for Robust Neural Network Distillation. In 2019 27th European Signal Processing Conference (EUSIPCO) (pp. 1-5). IEEE.
https://ieeexplore.ieee.org/abstract/document/8903112
Something similar: auto-generated curriculum. Not available online for some reason.

**MNIST for small experiments**

Kim, T. H., & Choi, J. (2018). ScreenerNet: Learning Self-Paced Curriculum for Deep Neural Networks. arXiv preprint arXiv:1801.00904.
https://arxiv.org/pdf/1801.00904.pdf
Not a popular one (9 citations since 2018), but may have some points relevant for me specifically.

Reduced MNIST: how well can machines learn from small data? By Michael Nielsen. Nov 15, 2017
http://cognitivemedium.com/rmnist
Blog post on learning on super-small subsets of MNIST (not an official pub, so never cited). One example of each digit apparently brings accuracy to 42% for a naive NN (vs 97% for full), and ~56% (vs 99% for full) for a pretrained conv net (2 layers, not trained on mnist specifically), followed by 2 fully connected. Regularization becomes extremely important, as a way to fight overfitting, and even switching to convnet for this may be considered a type of regularization, I think.

# Self-supervised learning

Jing, L., & Tian, Y. (2019). [Self-supervised visual feature learning with deep neural networks: A survey](https://arxiv.org/pdf/1902.06162.pdf). arXiv preprint arXiv:1902.06162.

Kolesnikov, A., Zhai, X., & Beyer, L. (2019). [Revisiting self-supervised visual representation learning](http://openaccess.thecvf.com/content_CVPR_2019/papers/Kolesnikov_Revisiting_Self-Supervised_Visual_Representation_Learning_CVPR_2019_paper.pdf). arXiv preprint arXiv:1901.09005.

He, K., Fan, H., Wu, Y., Xie, S., & Girshick, R. (2019). Momentum Contrast for Unsupervised Visual Representation Learning. arXiv preprint arXiv:1911.05722. https://arxiv.org/pdf/1911.05722.pdf

Misra, I., & van der Maaten, L. (2019). Self-Supervised Learning of Pretext-Invariant Representations. arXiv preprint arXiv:1912.01991. https://arxiv.org/abs/1912.01991 

Hénaff, O. J., Razavi, A., Doersch, C., Eslami, S. M., & Oord, A. V. D. (2019). Data-efficient image recognition with contrastive predictive coding. arXiv preprint arXiv:1905.09272. https://arxiv.org/abs/1905.09272v2

What's the current view of greedy training of autoencoders? Is it an abandoned technique, or are people still working on it? Sample seminal papers:
* Bengio, Y., Lamblin, P., Popovici, D., & Larochelle, H. (2007). Greedy layer-wise training of deep networks. In Advances in neural information processing systems (pp. 153-160).
* Hinton, G. E, Osindero, S., and Teh, Y. W. (2006). A fast learning algorithm for deep belief nets. Neural Computation, 18:1527-1554.
* http://scholarpedia.org/article/Deep_belief_networks

Zhai, X., Oliver, A., Kolesnikov, A., & Beyer, L. (2019). S4L: Self-Supervised Semi-Supervised Learning. arXiv preprint arXiv:1905.03670.
https://arxiv.org/abs/1905.03670

# Exploration, curiosity, RL

Meire Fortunato, Mohammad Gheshlaghi Azar, Bilal Piot, Jacob Menick, Ian Osband, Alex Graves, Vlad Mnih, Remi Munos, Demis Hassabis, Olivier Pietquin, et al. Noisy networks for exploration. arXiv preprint arXiv:1706.10295, 2017
A weak (not good enough) way to add exploration to AI playing videogames

Ortega, P. A., Wang, J. X., Rowland, M., Genewein, T., Kurth-Nelson, Z., Pascanu, R., ... & Jayakumar, S. M. (2019). Meta-learning of Sequential Strategies. arXiv preprint arXiv:1905.03030.
https://arxiv.org/pdf/1905.03030.pdf

Lázaro-Gredilla, M., Lin, D., Guntupalli, J. S., & George, D. (2018). Beyond imitation: Zero-shot task transfer on robots by learning concepts as cognitive programs. arXiv preprint arXiv:1812.02788.
https://robotics.sciencemag.org/content/4/26/eaav3150.full?ijkey=9p/p9D23WW2Ek&keytype=ref&siteid=robotics
Research article on imagination, recursion, and abstraction. Guessing context via transfer from prior experience.

Dauphin, Y. N., & Schoenholz, S. (2019). MetaInit: Initializing learning by learning to initialize. In Advances in Neural Information Processing Systems (pp. 12624-12636).
https://papers.nips.cc/paper/9427-metainit-initializing-learning-by-learning-to-initialize

#rl

Leibo, J. Z., Hughes, E., Lanctot, M., & Graepel, T. (2019). Autocurricula and the emergence of innovation from social interaction: A manifesto for multi-agent intelligence research. arXiv preprint arXiv:1903.00742.
https://arxiv.org/pdf/1903.00742.pdf
Something related to their multi-agent simulation (hide-and-seek). Seems promising.

Proximal Policy Optimization by OpenAI:
https://openai.com/blog/openai-baselines-ppo/
Something for agent training?

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

## RNNs, Attention
#attention

https://distill.pub/2019/memorization-in-rnns/
How LSTM networks remember text: a visual intro.

Hafner, D., Irpan, A., Davidson, J., & Heess, N. (2017). Learning hierarchical information flow with recurrent neural modules. In Advances in Neural Information Processing Systems (pp. 6724-6733).
https://papers.nips.cc/paper/7249-learning-hierarchical-information-flow-with-recurrent-neural-modules.pdf
ThalNet, which like a virtual thalamus. Only cited by 4 since 2017, so doesn't look like it caught on.

Jaderberg, M., Simonyan, K., & Zisserman, A. (2015). Spatial transformer networks. In Advances in neural information processing systems (pp. 2017-2025).
https://arxiv.org/pdf/1506.02025.pdf
2000 citations. Seems important.

Compressive Transformers for Long-Range Sequence Modelling
by Jack W. Rae, Anna Potapenko, Siddhant M. Jayakumar, Timothy P. Lillicrap (at DeepMind)
https://arxiv.org/abs/1911.05507

Single Headed Attention RNN: Stop Thinking With Your Head. S Merity. (2019)
https://arxiv.org/abs/1911.11423
He tried to diversify the field by deliberately sticking to a non-mainstream approach (something that is NOT transformers), and got good performance. Also the writing is wild.

Kitaev, N., Kaiser, Ł., & Levskaya, A. (2020). Reformer: The Efficient Transformer. arXiv preprint arXiv:2001.04451.
https://arxiv.org/abs/2001.04451v1
Claim that somehow by two changes in logic they improved [[transformers]] so much that now they would be O(L log L) instead of O(L²) efficient, which is like a big deal.

On the Relationship between Self-Attention and Convolutional Layers (2020)
Jean-Baptiste Cordonnier, Andreas Loukas, Martin Jaggi
https://openreview.net/forum?id=HJlnC1rKPB
Describe that attention networks spontaneiously develop convolution?

Lipton, Z. C., Berkowitz, J., & Elkan, C. (2015). A critical review of recurrent neural networks for sequence learning. arXiv preprint arXiv:1506.00019.
[https://arxiv.org/pdf/1506.00019.pdf](<https://arxiv.org/pdf/1506.00019.pdf>)
Great review for learning about RNNs (could be a textbook) 700 refs.

Press, O., Smith, N. A., & Levy, O. (2019). Improving Transformer Models by Reordering their Sublayers. arXiv preprint arXiv:1911.03864.
https://ofir.io/sandwich_transformer.pdf
An interesting tiny paper where all they did, I think, is play with the sequence of 2 common blocks, to show that the optimal sequence is not what everyone expected. May be neat. But I prob need to understand how transformers work to get it.

Arabshahi, F., Lu, Z., Singh, S., & Anandkumar, A. (2019). Memory Augmented Recursive Neural Networks. arXiv preprint arXiv:1911.01545.
https://arxiv.org/abs/1911.01545

# AEs, GANs, Generation

Oord, A. V. D., Dieleman, S., Zen, H., Simonyan, K., Vinyals, O., Graves, A., ... & Kavukcuoglu, K. (2016). Wavenet: A generative model for raw audio. arXiv preprint arXiv:1609.03499.
https://arxiv.org/abs/1609.03499
1.4k citations: famous really successful realtime audio generation network from Google DeepMind.

#gan

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

# Graphical
Bacciu, D., Errica, F., Micheli, A., & Podda, M. (2019). A Gentle Introduction to Deep Learning for Graphs. arXiv preprint arXiv:1912.12693.
https://arxiv.org/abs/1912.12693
Must read (seems short and gentle indeed)

Battaglia, P. W., Hamrick, J. B., Bapst, V., Sanchez-Gonzalez, A., Zambaldi, V., Malinowski, M., ... & Gulcehre, C. (2018). Relational inductive biases, deep learning, and graph networks. arXiv preprint arXiv:1806.01261.
https://arxiv.org/abs/1806.01261
Was described as practical and particularly well-written.

David Belli (2019). Generative graph transformer
https://davide-belli.github.io/generative-graph-transformer.html
Blog post with math and pretty pics: investigate!

Dehmamy, N., Barabási, A. L., & Yu, R. (2019). Understanding the Representation Power of Graph Neural Networks in Learning Graph Topology. arXiv preprint arXiv:1907.05008.
https://arxiv.org/abs/1907.05008

Bojchevski, A., Shchur, O., Zügner, D., & Günnemann, S. (2018). Netgan: Generating graphs via random walks. arXiv preprint arXiv:1803.00816.
https://arxiv.org/pdf/1803.00816.pdf

# Alternative network designs

**Helmholtz Machines**

Kevin G. Kirby (2006). A Tutorial on Helmholtz Machines 
https://www.nku.edu/~kirby/docs/HelmholtzTutorialKoeln.pdf

Dayan, P., Hinton, G. E., Neal, R. M., & Zemel, R. S. (1995). The helmholtz machine. Neural computation, 7(5), 889-904.
http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.34.1800&rep=rep1&type=pdf
Original paper by Hinton, 1k citations.

# CogSci and Meta
#meaning

Chollet, F. (2019). The Measure of Intelligence. arXiv preprint arXiv:1911.01547.
https://arxiv.org/abs/1911.01547
Seems to be endorsed by many good people. Bump it up on the list!

Turing, A. M. (1950). Computing machinery and intelligence. In Parsing the Turing Test (pp. 23-65). Springer, Dordrecht. 2005.
http://www.geielettronica.it/images/pdf/turing.pdf
Said to be a really nicely written paper, seminal, good examples, and with 13k citations.

Do language network learn what they think they learn?
https://thegradient.pub/nlps-clever-hans-moment-has-arrived/

van Rooij, I., Wright, C. D., Kwisthout, J., & Wareham, T. (2018). Rational analysis, intractability, and the prospects of ‘as if’-explanations. Synthese, 195(2), 491-510.
On how humans (and thus AI) could handle intractable problems.
Also these 2 blog posts that summarize and comment on this research: [one](https://experiencemachines.wordpress.com/2019/11/13/five-ways-the-mind-does-not-solve-computationally-intractable-problems-and-one-way-it-does/), and [two](https://irisvanrooijcogsci.com/2020/01/01/sampling-cannot-make-hard-work-light/)

Mesulam, M. M. (1998). From sensation to cognition. Brain: a journal of neurology, 121(6), 1013-1052.
http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.120.8687&rep=rep1&type=pdf
Review with 3k citations, which is a ton for this area. Something about modular organization of brain, and apparently so well written that highly influential?

Ullman, S. (1987). Visual routines. In Readings in computer vision (pp. 298-328). Morgan Kaufmann.
https://www.cs.cmu.edu/afs/cs/academic/class/15494-s12/readings/ullman-visual-routines.pdf
Some influential piece with 1.2k citations. Some take on the psychology of visual perception?
And a commentary / computer implementation of it (a presentation from 2008)
https://www.cs.cmu.edu/afs/cs/academic/class/15494-s08/lectures/visual_routines.pdf

Barsalou, L. W. (1999). Perceptual symbol systems. Behavioral and brain sciences, 22(4), 577-660.
http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.4.5511&rep=rep1&type=pdf
A hugely influential conceptual piece: 7k citations! Something about how knowledge is all about predicting your perception, and perceiving your actions? Lots of philosophy, followed by sensorimotor examples, neural representations. Then back-and-forth exchange with other cognitive scientists.

Marcus, G. (2018). Innateness, alphazero, and artificial intelligence. arXiv preprint arXiv:1801.05667.
[https://arxiv.org/ftp/arxiv/papers/1801/1801.05667.pdf](<https://arxiv.org/ftp/arxiv/papers/1801/1801.05667.pdf>)

Fodor, J. A., & Pylyshyn, Z. W. (1988). Connectionism and cognitive architecture: A critical analysis. Cognition, 28(1-2), 3-71.
[pdf on rutgers edu](http://ruccs.rutgers.edu/images/personal-zenon-pylyshyn/proseminars/Proseminar13/ConnectionistArchitecture.pdf)
Seminal old paper (5k refs) against neural networks :) that everybody reference when then want to question the supposed universality of deep learning.

Cardon, D., Cointet, J. P., & Mazieres, A. (2018). Neurons spike back: The Invention of Inductive Machines and the Artificial Intelligence Controversy.
[https://neurovenge.antonomase.fr/NeuronsSpikeBack.pdf](<https://neurovenge.antonomase.fr/NeuronsSpikeBack.pdf>)

Cisek, P. (2019). Resynthesizing behavior through phylogenetic refinement. Attention, Perception, & Psychophysics, 1-23.
https://link.springer.com/article/10.3758/s13414-019-01760-1
On evolution of mamallian brain, and how you can draw a link between behavioral (computational?) complexity, and hierarchical organization of a vertebrate brain. Tries to ambitiously draw parallels between evolution, development, and behavior.

# Symbolic and related

Yoshua Bengio’s short reading list:
* BabyAI: First Steps Towards Grounded Language Learning With a Human In the Loop, Chevalier-Boisvert et al.,
2018: https://arxiv.org/abs/1810.08272v2.
* A Meta-Transfer Objective for Learning to Disentangle Causal Mechanisms, Bengio et al., 2019:
https://arxiv.org/abs/1901.10912.
* Learning Neural Causal Models from Unknown Interventions, Ke et al., 2019: https://arxiv.org/abs/1910.
01075.
* Recurrent Independent Mechanisms, Goyal et al., 2019: https://arxiv.org/abs/1909.10893.
* The Consciousness Prior, Bengio et al., 2017: https://arxiv.org/abs/1709.08568.

Paul Smolensky. Next-generation architectures bridge gap between neural and symbolic representations with neural symbols. Microsoft Research blog. December 12, 2019.
https://www.microsoft.com/en-us/research/blog/next-generation-architectures-bridge-gap-between-neural-and-symbolic-representations-with-neural-symbols/

# Complexity
#complexity

Corominas-Murtra, B., Seoane, L. F., & Solé, R. (2018). Zipf’s law, unbounded complexity and open-ended evolution. Journal of the Royal Society Interface, 15(149), 20180395.
https://royalsocietypublishing.org/doi/pdf/10.1098/rsif.2018.0395
General patterns of increase in complexity during evolution, using several natural and artificial examples (texts, proteins, logic circuits, and even combinations of Legos). Information and string complexity.

Zenil, H., & Villarreal-Zapata, E. (2013). Asymptotic behavior and ratios of complexity in cellular automata. International Journal of Bifurcation and Chaos, 23(09), 1350159.
https://arxiv.org/pdf/1304.2816.pdf
Some measures of complexity (Shannon's block entropy and Kolmogorov) applied to 1D cellular automata. They try to make some general statements. Give a look.