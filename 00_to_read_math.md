# To-read: ML and AI

#bib

#halfthere - a tag to be used for papers that have their separate entries, but that I haven't actually finished reading, or haven't read well. Papers to revisit!

**See also** (other thematic to-read lists):
* [[11_RL]] - Reinforcement Learning
* [[09_Graphs]] - Graph (aka Graphical) networks

# Tab-dumps

AutoML-Zero: Evolving Machine Learning Algorithms From Scratch:
* https://arxiv.org/abs/2003.03384
* https://github.com/google-research/google-research/tree/master/automl_zero#automl-zero

* https://distill.pub/2020/circuits/zoom-in/
* How does the non-conscious become conscious:
https://sci-hub.se/https://doi.org/10.1016/j.cub.2020.01.033

* https://ai.googleblog.com/2020/02/exploring-transfer-learning-with-t5.html
* https://github.com/sergio-hcsoft/FractalAI
* https://github.com/chuanli11/CNNMRF
* https://arxiv.org/abs/2003.03988
* https://arxiv.org/abs/2003.03461
* https://www.goodai.com/implementation-of-generative-teaching-networks-for-pytorch/

# Intros and top choices

Tishby, N., Pereira, F. C., & Bialek, W. (2000). The information bottleneck method. arXiv preprint physics/0004057.
https://arxiv.org/pdf/physics/0004057.pdf
2k citations.

Saxe, A. M., Bansal, Y., Dapello, J., Advani, M., Kolchinsky, A., Tracey, B. D., & Cox, D. D. (2019). On the information bottleneck theory of deep learning. Journal of Statistical Mechanics: Theory and Experiment, 2019(12), 124020.
https://iopscience.iop.org/article/10.1088/1742-5468/ab3985
Seems to have struck a nerve (>100 citations in just one month!). And not that hard on math.

Xie, Q., Luong, M. T., Hovy, E., & Le, Q. V. (2020). Self-training with noisy student improves imagenet classification. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (pp. 10687-10698).
https://arxiv.org/abs/1911.04252
Google. Something weird semi-supervised learning with noisy teachers and distillation. Essentially, it seems that a badly labeled large dataset is better than a well-labeled small dataset, so it's better to train one model on a small dataset, then have it label a huge dataset (even tho many labels will be wrong), and then use this large dataset to train the next model. Or something like that. Weird.
Stub: [[Xie2020noisy_student]]

Reflections on Innateness in Machine Learning
Thomas G. Dietterich. Mar 6, 2018.
https://medium.com/@tdietterich/reflections-on-innateness-in-machine-learning-4eebefa3e1af
Another must-do short read.

The Unreasonable Effectiveness of Recurrent Neural Networks
http://karpathy.github.io/2015/05/21/rnn-effectiveness/

Bahri, Y., Kadmon, J., Pennington, J., Schoenholz, S. S., Sohl-Dickstein, J., & Ganguli, S. (2020). Statistical mechanics of deep learning. Annual Review of Condensed Matter Physics.
https://www.annualreviews.org/doi/pdf/10.1146/annurev-conmatphys-031119-050745

Poggio, T., Banburski, A., & Liao, Q. (2020). Theoretical issues in deep networks. Proceedings of the National Academy of Sciences.	
https://www.pnas.org/content/pnas/early/2020/06/08/1907369117.full.pdf

One of the original DeepLerning review papers (28k citations!!!)
LeCun, Y., Bengio, Y., & Hinton, G. (2015). Deep learning. nature, 521(7553), 436-444.
https://www.nature.com/articles/nature14539

Rolnick, D., & Tegmark, M. (2017). The power of deeper networks for expressing natural functions. arXiv preprint arXiv:1705.05502.
https://arxiv.org/abs/1705.05502

Identity Crisis: Memorization and Generalization Under Extreme Overparameterization 
Chiyuan Zhang, Samy Bengio, Moritz Hardt, Michael C. Mozer, Yoram Singer
https://openreview.net/forum?id=B1l6y0VFPr
https://arxiv.org/abs/1902.04698
Google brain. Identity mapping: just train the output to be exactly like the input, then study the logic of transfer to other inputs.

Olah, C., Satyanarayan, A., Johnson, I., Carter, S., Schubert, L., Ye, K., & Mordvintsev, A. (2018). The building blocks of interpretability. Distill, 3(3), e10.
https://distill.pub/2018/building-blocks/
Google. About reverse-tracking through a visual network via optimizing inputs?
Should be placed here: [[Olah2018interpretability]]

Liao, Q., Miranda, B., Rosasco, L., Banburski, A., Liang, R., Hidary, J., & Poggio, T. (2019). Generalization Puzzles in Deep Networks.
https://openreview.net/pdf?id=BkelnhNFwB

Gatys, L. A., Ecker, A. S., & Bethge, M. (2015). A neural algorithm of artistic style. arXiv preprint arXiv:1508.06576.
https://arxiv.org/pdf/1508.06576.pdf

Deep Residual Learning for Image Recognition. Related to [[He2015init]]?
https://arxiv.org/pdf/1512.03385.pdf

Mishkin, D., & Matas, J. (2015). All you need is a good init. arXiv preprint arXiv:1511.06422.
https://arxiv.org/pdf/1511.06422.pdf
Related to [[He2015init]]?

Mordvintsev, A., Randazzo, E., Niklasson, E., & Levin, M. (2020). Growing Neural Cellular Automata. Distill, 5(2), e23.
https://distill.pub/2020/growing-ca/
Google. A paper and a sim, two in one!

A list of most influential DL papers of past decade, with pics and summaries!
https://leogao.dev/2019/12/31/The-Decade-of-Deep-Learning/

Raschka, S., Patterson, J., & Nolet, C. (2020). Machine Learning in Python: Main developments and technology trends in data science, machine learning, and artificial intelligence. arXiv preprint arXiv:2002.04803.
https://arxiv.org/abs/2002.04803

More info on decision trees, pruning
https://towardsdatascience.com/the-complete-guide-to-decision-trees-28a4e3c7be14

Ioffe, S., & Szegedy, C. (2015). Batch normalization: Accelerating deep network training by reducing internal covariate shift. arXiv preprint arXiv:1502.03167.
[https://arxiv.org/abs/1502.03167](<https://arxiv.org/abs/1502.03167>)
Main paper on batch normalization (with like 30k references). Read and summarize in the [[06_DL]] section.
* Related paper on batch norm: Frankle, J., Schwab, D. J., & Morcos, A. S. (2020). Training BatchNorm and Only BatchNorm: On the Expressive Power of Random Features in CNNs. arXiv preprint arXiv:2003.00152. - about how only training batch norm layers, keeping all other parameters random, is enough to create a functional network
* [Some blog follow-up](https://app.wandb.ai/sayakpaul/training-bn-only/reports/The-Power-of-Random-Features-of-a-CNN--VmlldzoxMTIxODA) from which I actually learned about this paper.

Try to get the gist of it, even if just to participate in the discussion. File, even if math turns to be harder than comfortable:
Automatic Differentiation via Contour Integration
Jan 16, 2020. Aidan Rocke
https://keplerlounge.com/neural-computation/2020/01/16/complex-auto-diff.html
Aidan claims tha this math somehow helps to explain calculations in a dendritic tree, but it has lots of calculus, and seems like something one has to actually think about. It like has math and stuff! Complex functions, poles, and what not.

Research dept
https://distill.pub/2017/research-debt/
(Some general ideas about explaining research)

Rahaman, N., Baratin, A., Arpit, D., Draxler, F., Lin, M., Hamprecht, F. A., ... & Courville, A. (2018). On the spectral bias of neural networks. arXiv preprint arXiv:1806.08734.
https://arxiv.org/abs/1806.08734
On what neural network can encode easily - seems to be a very useful read, potentially.

Challenging Common Deep Learning Conventions – Part 1: Rethinking Layer-wise Feature Amounts in Convolutional Neural Network Architectures. Martin Mundt. Jan 2020
http://martin-mundt.com/rethinking-cnn-feature-amounts/

Neural Reparameterization Improves Structural Optimization, by Sam Greydanus
https://greydanus.github.io/2019/12/15/neural-reparam/
A really cool post on Neural reparameterization (ANN as a way to optimize search for new structures; like ANNs in the middle of a formal algorithms; the one with bridges for example - super-fun!)

He, K., Zhang, X., Ren, S., & Sun, J. (2016). Deep residual learning for image recognition. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 770-778).
http://openaccess.thecvf.com/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf
36k citations, haha. The famous residual networks paper.

Berseth, G., Geng, D., Devin, C., Finn, C., Jayaraman, D., & Levine, S. (2019). SMiRL: Surprise Minimizing RL in Dynamic Environments. arXiv preprint arXiv:1912.05510.
https://arxiv.org/abs/1912.05510
Blog:
https://bair.berkeley.edu/blog/2019/12/18/smirl/
Sounds extremely interesting!!

Ribeiro, M. T., Singh, S., & Guestrin, C. (2016, August). Why should i trust you?: Explaining the predictions of any classifier. In Proceedings of the 22nd ACM SIGKDD international conference on knowledge discovery and data mining (pp. 1135-1144). ACM.
https://arxiv.org/abs/1602.04938
Seems to be a classic text, on model interpretatibility (that is often critical for production)

Your Classifier is Secretly an Energy Based Model and You Should Treat it Like One Will Grathwohl, Kuan-Chieh Wang, Jörn-Henrik Jacobsen, David Duvenaud, Mohammad Norouzi, Kevin Swersky. https://arxiv.org/abs/1912.03263

Normalized Convolution: the idea that convolutional layers actually work much better if layers themselves, and not inputs or activation, are kept normalized
* https://www.reddit.com/r/MachineLearning/comments/g0nkof/d_normalized_convolution/
* https://arxiv.org/abs/1602.07868
* https://arxiv.org/abs/1903.10520

Metz, L., Maheswaranathan, N., Cheung, B., & Sohl-Dickstein, J. (2018). Meta-Learning Update Rules for Unsupervised Representation Learning. arXiv preprint arXiv:1804.00222.
https://arxiv.org/abs/1804.00222
Google.

Finn, C., Abbeel, P., & Levine, S. (2017, August). Model-agnostic meta-learning for fast adaptation of deep networks. In Proceedings of the 34th International Conference on Machine Learning-Volume 70 (pp. 1126-1135). JMLR. org.
https://arxiv.org/pdf/1703.03400.pdf
About few-shots learning, and generalizing from a very limited number of labels? More than 1000 citations!

Schroff, F., Kalenichenko, D., & Philbin, J. (2015). Facenet: A unified embedding for face recognition and clustering. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 815-823).
https://arxiv.org/abs/1503.03832 
See also: [[triplet_loss]]

Van Steenkiste, S., Chang, M., Greff, K., & Schmidhuber, J. (2018). Relational neural expectation maximization: Unsupervised discovery of objects and their interactions. arXiv preprint arXiv:1802.10353.
https://arxiv.org/abs/1802.10353
Unsupervized discovery of objects: seems interesting

Hu, J., Shen, L., & Sun, G. (2018). Squeeze-and-excitation networks. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 7132-7141).
https://arxiv.org/abs/1709.01507
Another paper with 1600 citations. Alternative to convolution networks, apparently? Is it faster, or better?

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

Simonyan, K., & Zisserman, A. (2014). Very deep convolutional networks for large-scale image recognition. arXiv preprint arXiv:1409.1556.
https://arxiv.org/pdf/1409.1556.pdf
40k citations. Seminal in some way?

The Topology of Neural Networks, Part 2: Compositions and Dimensions. 2020. Jesse Johnson. Blog post.
https://ldtopology.wordpress.com/2019/03/07/the-topology-of-neural-networks-part-2-compositions-and-dimensions/#more-4903

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

Le Cun, Y., Kanter, I., & Solla, S. A. (1991). Eigenvalues of covariance matrices: Application to neural-network learning. Physical Review Letters, 66(18), 2396.
https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.66.2396
Related to "Double Descent", but 30 years prior haha :)

Arora, S., Du, S. S., Li, Z., Salakhutdinov, R., Wang, R., & Yu, D. (2019). Harnessing the Power of Infinitely Wide Deep Nets on Small-data Tasks. arXiv preprint arXiv:1910.01663.
https://arxiv.org/pdf/1910.01663.pdf

Ba, J., & Caruana, R. (2014). Do deep nets really need to be deep?. In Advances in neural information processing systems (pp. 2654-2662).
https://papers.nips.cc/paper/5484-do-deep-nets-really-need-to-be-deep.pdf
Seems to be one of the original network distillation papers (800 refs)

Marc Finzi, Samuel Stanton, Pavel Izmailov, Andrew Gordon Wilson (2020). Generalizing Convolutional Neural Networks for Equivariance to Lie Groups on Arbitrary Continuous Data
https://arxiv.org/pdf/2002.12880.pdf
How to build convolution-like networks for data that has different invariants than rotation and translation. (Mathy, but try?)

Johnson, J. (2018). Deep, skinny neural networks are not universal approximators. arXiv preprint arXiv:1810.00393.
https://arxiv.org/abs/1810.00393

Poggio, T., Liao, Q., & Banburski, A. (2020). Complexity control by gradient descent in deep networks. Nature communications, 11(1), 1-5.
https://www.nature.com/articles/s41467-020-14663-9

# Interpretability

https://distill.pub/2020/circuits/early-vision/
Can we understand some very first layers of an image recognition network? Those that supposedly should be "simple", and maybe similar to that of first few connections in the brain? (The answer seems to be, that it is much tricker than one could have hoped)

Golan, T., Raju, P. C., & Kriegeskorte, N. (2019). Controversial stimuli: pitting neural networks against each other as models of human recognition. arXiv preprint arXiv:1911.09288.
https://arxiv.org/abs/1911.09288
They played with MNIST, trained a bunch of models, and compared them. And also compared it all to humans. Also a nice twitter thread:
https://twitter.com/TalGolanNeuro/status/1198394188481671168

Virgolin, M., Alderliesten, T., & Bosman, P. A. (2020). On explaining machine learning models by evolving crucial and compact features. Swarm and Evolutionary Computation, 53, 100640.
https://arxiv.org/abs/1907.02260

Mahendran, A., & Vedaldi, A. (2015). Understanding deep image representations by inverting them. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 5188-5196).
https://arxiv.org/pdf/1412.0035v1.pdf
[final pdf](https://www.cv-foundation.org/openaccess/content_cvpr_2015/papers/Mahendran_Understanding_Deep_Image_2015_CVPR_paper.pdf)

TensorFlow Lattice: Flexible, controlled and interpretable ML
https://blog.tensorflow.org/2020/02/tensorflow-lattice-flexible-controlled-and-interpretable-ML.html?linkId=82088125
Constrained models? May be very nice and practical; not just theoretically cool, but immedeately useful.

Gilboa, D., & Gur-Ari, G. (2019). Wider Networks Learn Better Features. arXiv preprint arXiv:1909.11572.
https://arxiv.org/pdf/1909.11572.pdf
Google. Train a network (they used MNIST), do UMAP of activations, find groups of neurons that together encode useful features, visualize them. Reasonable approach to understanding networks.

Simonyan, K., Vedaldi, A., & Zisserman, A. (2013). Deep inside convolutional networks: Visualising image classification models and saliency maps. arXiv preprint arXiv:1312.6034.
https://arxiv.org/pdf/1312.6034.pdf
Features visualization.

Doshi-Velez, F., & Kim, B. (2017). Towards a rigorous science of interpretable machine learning. arXiv preprint arXiv:1702.08608.
https://arxiv.org/abs/1702.08608
(This is actually about [[bib_fairness]])

# Tickets, distillation, transfer
#ticket

Frankle, J., & Carbin, M. (2018). The lottery ticket hypothesis: Finding sparse, trainable neural networks. arXiv preprint arXiv:1803.03635.
https://arxiv.org/abs/1803.03635
Original paper presenting the lottery ticket hypothesis.

http://news.mit.edu/2020/foolproof-way-shrink-deep-learning-models-0430
Recent blog post and publication from MIT that got some decent press.

Zhou, H., Lan, J., Liu, R., & Yosinski, J. (2019). Deconstructing lottery tickets: Zeros, signs, and the supermask. arXiv preprint arXiv:1905.01067.
https://arxiv.org/abs/1905.01067

Tian, Y., Jiang, T., Gong, Q., & Morcos, A. (2019). Luck Matters: Understanding Training Dynamics of Deep ReLU Networks. arXiv preprint arXiv:1905.13405.
https://arxiv.org/pdf/1905.13405.pdf
Facebook AI. To understand how "lucky tickets" work, they train a larger network from a smaller network, then study activation.

Hooker, S., Courville, A., Dauphin, Y., & Frome, A. (2019). Selective Brain Damage: Measuring the Disparate Impact of Model Pruning. arXiv preprint arXiv:1911.05248.
https://weightpruningdamage.github.io/
Tweet-thread: https://twitter.com/sarahookr/status/1197514514167545858
tldr: while pruning may preserve the bulk of performance, it's still a trade-off, and usually it lowers performance on selective rare special cases that networks stops recognizing. They used ImageNet, and identified which images exactly are "lost" during pruning.

Liu, Z., Sun, M., Zhou, T., Huang, G., & Darrell, T. (2018). Rethinking the value of network pruning. arXiv preprint arXiv:1810.05270.
https://arxiv.org/pdf/1810.05270.pdf
They expressed concerns that ticket hypothesis might not be true, but later decided that for sparse enough networks it is true after all:
Frankle, J., Dziugaite, G. K., Roy, D. M., & Carbin, M. Stabilizing the Lottery Ticket Hypothesis. arXiv, page.
https://arxiv.org/abs/1903.01611

Gale, T., Elsen, E., & Hooker, S. (2019). The state of sparsity in deep neural networks. arXiv preprint arXiv:1902.09574.ellu
https://arxiv.org/abs/1902.09574
Showing that for networks that are sparse enough, the ticket hypothesis still works.

Individual differences among deep neural network models (2020)
Johannes Mehrer, Courtney J. Spoerer, Nikolaus Kriegeskorte, Tim C Kietzmann
https://www.biorxiv.org/content/10.1101/2020.01.08.898288v1
Neuro-inspired: approach ANNs as one would approach individual animals. Use "Representation Similarity analysis" from neuro, changing nothing but weight init. Describe differences (pretty pics!). Claim that dropout helps.
Tweetprint: https://twitter.com/TimKietzmann/status/1215620270679044096

Morcos, A. S., Yu, H., Paganini, M., & Tian, Y. (2019). One ticket to win them all: generalizing lottery ticket initializations across datasets and optimizers. arXiv preprint arXiv:1906.02773.
https://arxiv.org/abs/1906.02773
Facebook AI. Whether "lucky tickets" generalize across datasets.

Maheswaranathan, N., Williams, A., Golub, M., Ganguli, S., & Sussillo, D. (2019). Universality and individuality in neural dynamics across large populations of recurrent networks. In Advances in Neural Information Processing Systems (pp. 15603-15615).
https://arxiv.org/abs/1907.08549
Google. They trained the same network (or similar networks) thousands of times, and checked how similar the profiles are. Kind of related to both O'Leary neuro idea, and the ticket hypothesis. An interesting follow-up would be something like performance outside of the preferred distributions, and how this performance (individuality?) is distributed itself, and whether some architectures tend to produce clusters rather then smooth distributions (personality?) Tweetprint: https://twitter.com/SussilloDavid/status/1153427790672171009

Ramanujan, V., Wortsman, M., Kembhavi, A., Farhadi, A., & Rastegari, M. (2019). What's Hidden in a Randomly Weighted Neural Network?. arXiv preprint arXiv:1911.13299.
https://arxiv.org/pdf/1911.13299.pdf
They do some math, and some modeling, trying to estimate how easy it is to find a subnetwork, within a randomly weighted wide network, that would work great without any training. A combination of ticket story, and weight-agnostic networks (see [[Gaier2019agnostic]]). Allen institute.

Proving the Lottery Ticket Hypothesis: Pruning is All You Need. Eran Malach, Gilad Yehudai, Shai Shalev-Shwartz, Ohad Shamir. (3 Feb 2020)
https://arxiv.org/abs/2002.00585

#distillation #transfer

Hinton, G., Vinyals, O., & Dean, J. (2015). Distilling the knowledge in a neural network. arXiv preprint arXiv:1503.02531.
https://arxiv.org/pdf/1503.02531.pdf
The splashy "original distillation paper" (2500 refs)

Furlanello, T., Lipton, Z. C., Tschannen, M., Itti, L., & Anandkumar, A. (2018). Born-again neural networks. arXiv preprint arXiv:1805.04770.
https://arxiv.org/pdf/1805.04770.pdf

Hossein Mobahi, Mehrdad Farajtabar, Peter L. Bartlett (2020). Self-Distillation Amplifies Regularization in Hilbert Space. https://arxiv.org/abs/2002.05715
Google. Feeding predictions of network as additional training points serves as regularization (or maybe rather, amplifies existing regularization?), and improves accuracy. Really good visuals, in this paper. In a way, self-distillation may be considered a case of self-supervised learning (almost unsupervised, in a way). But this is a mathy paper. Also a good twitter-thread:
https://twitter.com/TheGradient/status/1228132843630387201

von Oswald, J., Henning, C., Sacramento, J., & Grewe, B. F. (2019). Continual learning with hypernetworks. arXiv preprint arXiv:1906.00695.
https://arxiv.org/abs/1906.00695
If it get it right from the abstract, this can be called meta-networks as well: a network that learns to predict weights for a network that would follow a task; so one step above learning the weights for each individual task. Essentially, instead of remembering all possible network configurations for all possible tasks, to reset the network each time, they use this hypernetwork to interpolate in the space of parameters.

Sanh, V., Wolf, T., & Rush, A. M. (2020). Movement Pruning: Adaptive Sparsity by Fine-Tuning. arXiv preprint arXiv:2005.07683.
https://arxiv.org/abs/2005.07683
Pruning somehow optimized for language models.

How We Scaled Bert To Serve 1+ Billion Daily Requests on CPUs. By: Quoc N. Le and Kip Kaehler (2020).
https://medium.com/roblox-tech-blog/how-we-scaled-bert-to-serve-1-billion-daily-requests-on-cpus-d99be090db26
On pruning BERT to achieve both high throughput and high accuracy (claim that they found a better way to do it, compared to pruning precise models, or training small models)

# Curriculum

Wang, T., Zhu, J. Y., Torralba, A., & Efros, A. A. (2018). Dataset Distillation. arXiv preprint arXiv:1811.10959.
https://arxiv.org/pdf/1811.10959.pdf
Facebook AI. 10 images give 94% accuracy on MNIST. Images however are completely not-letter like (weird noisy fields), so in spirit it's closer to hacking (data poisoning, adversarial examples, except positive ones) than to interpretation.

Generative Teaching Networks: Accelerating Neural Architecture Search by Learning to Generate Synthetic Training Data.
Felipe Petroski Such, Aditya Rawal, Joel Lehman, Kenneth O. Stanley, Jeff Clune
https://arxiv.org/abs/1912.07768
Uber AI. Blog post with a rather detailed description and commentary:
https://eng.uber.com/generative-teaching-networks/

Panagiotatos, G., Passalis, N., Iosifidis, A., Gabbouj, M., & Tefas, A. (2019, September). Curriculum-based Teacher Ensemble for Robust Neural Network Distillation. In 2019 27th European Signal Processing Conference (EUSIPCO) (pp. 1-5). IEEE.
https://ieeexplore.ieee.org/abstract/document/8903112
Something similar: auto-generated curriculum. Not available online for some reason.

Kim, T. H., & Choi, J. (2018). ScreenerNet: Learning Self-Paced Curriculum for Deep Neural Networks. arXiv preprint arXiv:1801.00904.
https://arxiv.org/pdf/1801.00904.pdf
Not a popular one (9 citations since 2018), but may have some points relevant for me specifically. MNIST-based?

Reduced MNIST: how well can machines learn from small data? By Michael Nielsen. Nov 15, 2017
http://cognitivemedium.com/rmnist
Blog post on learning on super-small subsets of MNIST (not an official pub, so never cited). One example of each digit apparently brings accuracy to 42% for a naive NN (vs 97% for full), and ~56% (vs 99% for full) for a pretrained conv net (2 layers, not trained on mnist specifically), followed by 2 fully connected. Regularization becomes extremely important, as a way to fight overfitting, and even switching to convnet for this may be considered a type of regularization, I think.

**Data poisoning and similar ideas**

Radioactive data: tracing through training. Alexandre Sablayrolles, Matthijs Douze, Cordelia Schmid, Hervé Jégou
(3 Feb 2020). https://arxiv.org/abs/2002.00937
~1% of the dataset, make models trained on this data uniquely identifiable.

**Sparsity**
> Exploring a general idea that intentional sparsity may be a good way to improve learning, aid pruning and/or distillation, improve interpretability (because fewer weights are involved, leading to more "high-contrast" connectivity patterns) etc.

https://en.wikipedia.org/wiki/Sparse_dictionary_learning

Makhzani, A., & Frey, B. (2013). K-sparse autoencoders. arXiv preprint arXiv:1312.5663.
https://arxiv.org/pdf/1312.5663.pdf

# Architectures and Arch search

#archsearch

Ha, D., Dai, A., & Le, Q. V. (2016). Hypernetworks. arXiv preprint arXiv:1609.09106.
https://arxiv.org/pdf/1609.09106.pdf
Networks that output weights of a network.

Cohen, T., & Welling, M. (2016, June). Group equivariant convolutional networks. In International conference on machine learning (pp. 2990-2999).
https://arxiv.org/pdf/1602.07576.pdf
Group-CNNs: an extention of CNNs that are not only translational, but rotation and flip-symmetric. For a neuro person, it seems to be a no-brainer that they should be a strictly better solution than common CNNs. So, are they?

Mishkin, D., Sergievskiy, N., & Matas, J. (2017). Systematic evaluation of convolution neural network advances on the imagenet. Computer Vision and Image Understanding, 161, 11-19.
https://arxiv.org/abs/1606.02228
A paper posed as an excellent, archetypal, systematic ablation study that really went down to the core of what aspects of a certain architecture are critical, and which ones are not. As an example of good, solid research methodology.

Li, Z., Wang, R., Yu, D., Du, S. S., Hu, W., Salakhutdinov, R., & Arora, S. (2019). Enhanced convolutional neural tangent kernels. arXiv preprint arXiv:1911.00809.
https://arxiv.org/abs/1911.00809

Liu, H., Simonyan, K., Vinyals, O., Fernando, C., & Kavukcuoglu, K. (2017). Hierarchical representations for efficient architecture search. arXiv preprint arXiv:1711.00436.
https://arxiv.org/pdf/1711.00436.pdf
Summary by Connor Shorten. Sep 12 2019.
https://towardsdatascience.com/hierarchical-neural-architecture-search-aae6bbdc3624
DeepMind. How to optimize network architectures if training each model is so ridiculously expensive? By defining blocks, and then using them recursively to create larger networks… Not sure how it helps, but apparently it helps a lot?

Elsken, T., Metzen, J. H., & Hutter, F. (2018). Neural architecture search: A survey. arXiv preprint arXiv:1808.05377.
http://www.jmlr.org/papers/volume20/18-598/18-598.pdf


# Self-supervised learning

Jing, L., & Tian, Y. (2019). [Self-supervised visual feature learning with deep neural networks: A survey](https://arxiv.org/pdf/1902.06162.pdf). arXiv preprint arXiv:1902.06162.

Kolesnikov, A., Zhai, X., & Beyer, L. (2019). [Revisiting self-supervised visual representation learning](http://openaccess.thecvf.com/content_CVPR_2019/papers/Kolesnikov_Revisiting_Self-Supervised_Visual_Representation_Learning_CVPR_2019_paper.pdf). arXiv preprint arXiv:1901.09005.
Google.

Kevin Musgrave, Serge Belongie, Ser-Nam Lim (2020). A Metric Learning Reality Check
https://arxiv.org/pdf/2003.08505v1.pdf
On a first glance, throw a bucket of cold water over the head of recent advancements in this particular type of self-supervized learning (all sorts of contrastive learning and negative sampling). Seem to claim that with proper optimization all methods proposed in last 4 years or so yield results of exactly same quality, and there was no progress after all. Facebook and Cornell.

Chollet, F. (2019). The Measure of Intelligence. arXiv preprint arXiv:1911.01547.
https://arxiv.org/abs/1911.01547
Google. On how transfer and generalized intelligence may look, and how they may be tested. That's why I'm placing it in "Self-supervised learning" section: it is not really about it, but it is something that goes beyond approximating a data set; something like "transfer par excellence!""
There is now also a Keggle challenge developed after it:
https://www.kaggle.com/c/abstraction-and-reasoning-challenge
Nice set of examples: repeat the pattern, move one part relative to another, color according to a certain rule, fill the gaps, etc. Like a set of nice IQ tests on a grid. A human (me) has no problem with some, and can figure others after 30-60s of concentration. But it's quite obvious that modern types of AI would have enormous problems with it.
Will be hosted at [[Chollet2019measure]]

A Simple Framework for Contrastive Learning of Visual Representations
Ting Chen, Simon Kornblith, Mohammad Norouzi, Geoffrey Hinton
https://arxiv.org/pdf/2002.05709.pdf
Google. Exploration of various unsupervized learning methods (contrastive learning with data augmentation). As a test of effectiveness, train the network in a supervized way on 1% of labels, measure its performance compared to the full set.

Roads, B. D., & Love, B. C. (2020). Learning as the unsupervised alignment of conceptual systems. Nature Machine Intelligence, 1-7.
https://arxiv.org/abs/1906.09012

He, K., Fan, H., Wu, Y., Xie, S., & Girshick, R. (2019). Momentum Contrast for Unsupervised Visual Representation Learning. arXiv preprint arXiv:1911.05722. https://arxiv.org/pdf/1911.05722.pdf
Facebook AI.

Misra, I., & van der Maaten, L. (2019). Self-Supervised Learning of Pretext-Invariant Representations. arXiv preprint arXiv:1912.01991. https://arxiv.org/abs/1912.01991 

Hénaff, O. J., Razavi, A., Doersch, C., Eslami, S. M., & Oord, A. V. D. (2019). Data-efficient image recognition with contrastive predictive coding. arXiv preprint arXiv:1905.09272. https://arxiv.org/abs/1905.09272v2
DeepMind.

What's the current view of greedy training of autoencoders? Is it an abandoned technique, or are people still working on it? Sample seminal papers:
* Bengio, Y., Lamblin, P., Popovici, D., & Larochelle, H. (2007). Greedy layer-wise training of deep networks. In Advances in neural information processing systems (pp. 153-160).
* Hinton, G. E, Osindero, S., and Teh, Y. W. (2006). A fast learning algorithm for deep belief nets. Neural Computation, 18:1527-1554.
* http://scholarpedia.org/article/Deep_belief_networks

Zhai, X., Oliver, A., Kolesnikov, A., & Beyer, L. (2019). S4L: Self-Supervised Semi-Supervised Learning. arXiv preprint arXiv:1905.03670.
https://arxiv.org/abs/1905.03670
Google.

Ravanelli, M., Zhong, J., Pascual, S., Swietojanski, P., Monteiro, J., Trmal, J., & Bengio, Y. (2020). Multi-task self-supervised learning for Robust Speech Recognition. arXiv preprint arXiv:2001.09239.
https://arxiv.org/abs/2001.09239

Alex Nichol (2020). VQ-DRAW: A Sequential Discrete VAE
https://arxiv.org/abs/2003.01599
https://blog.aqnichol.com/2020/03/04/vq-draw-a-new-generative-model/
Discrete varioational autoencoders; sorted latent space (from more important to less important factors), allowing approximation. Is it relevant?

# Exploration, curiosity, RL

Yujin Tang, Duong Nguyen, David Ha (2020). Neuroevolution of Self-Interpretable Agents
https://attentionagent.github.io/
https://arxiv.org/abs/2003.08165
Limiting attention of agents helps them to learn to play (bottlenecking of sorts?). Google Brain + Mind.

Introducing Dreamer: Scalable Reinforcement Learning Using World Models (2020)
https://ai.googleblog.com/2020/03/introducing-dreamer-scalable.html

Lázaro-Gredilla, M., Lin, D., Guntupalli, J. S., & George, D. (2018). Beyond imitation: Zero-shot task transfer on robots by learning concepts as cognitive programs. arXiv preprint arXiv:1812.02788.
https://robotics.sciencemag.org/content/4/26/eaav3150.full?ijkey=9p/p9D23WW2Ek&keytype=ref&siteid=robotics
Research article on imagination, recursion, and abstraction. Guessing context via transfer from prior experience.

Ortega, P. A., Wang, J. X., Rowland, M., Genewein, T., Kurth-Nelson, Z., Pascanu, R., ... & Jayakumar, S. M. (2019). Meta-learning of Sequential Strategies. arXiv preprint arXiv:1905.03030.
https://arxiv.org/pdf/1905.03030.pdf
DeepMind.

Berseth, G., Geng, D., Devin, C., Finn, C., Jayaraman, D., & Levine, S. (2019). SMiRL: Surprise Minimizing RL in Dynamic Environments. arXiv preprint arXiv:1912.05510.
https://arxiv.org/abs/1912.05510
Unsupervised learning by chaos minimization!
Blog post: Emergent Behavior by Minimizing Chaos
https://bair.berkeley.edu/blog/2019/12/18/smirl/

Meire Fortunato, Mohammad Gheshlaghi Azar, Bilal Piot, Jacob Menick, Ian Osband, Alex Graves, Vlad Mnih, Remi Munos, Demis Hassabis, Olivier Pietquin, et al. Noisy networks for exploration. arXiv preprint arXiv:1706.10295, 2017
https://arxiv.org/pdf/1706.10295.pdf
DeepMind. A weak (official term?) way to add exploration to AI playing videogames, by target-injecting noise into the network (?). Sometimes (?) elevates the performance from "sub-human to super-human" level (funny turn of a phrase).

Dauphin, Y. N., & Schoenholz, S. (2019). MetaInit: Initializing learning by learning to initialize. In Advances in Neural Information Processing Systems (pp. 12624-12636).
https://papers.nips.cc/paper/9427-metainit-initializing-learning-by-learning-to-initialize
Google. Quite mathy (not this paper necessarily, but their line of research.)

#rl

Ha, D., & Schmidhuber, J. (2018). World models. arXiv preprint arXiv:1803.10122.
https://arxiv.org/abs/1803.10122
Generative network models for learning environments!

Leibo, J. Z., Hughes, E., Lanctot, M., & Graepel, T. (2019). Autocurricula and the emergence of innovation from social interaction: A manifesto for multi-agent intelligence research. arXiv preprint arXiv:1903.00742.
https://arxiv.org/pdf/1903.00742.pdf
DeepMind. Something related to their multi-agent simulation (hide-and-seek). Seems promising.

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
Facebook AI. Lottery tickets in RL domain.

Two papers describing a similar idea: Just going from 3d-person to first-person helps generalization in RL, as first-person is inherently closer to translation to actions.
* Felix Hill, Andrew Lampinen, Rosalia Schneider, Stephen Clark, Matthew Botvinick, James L. McClelland, Adam Santoro (2019). Environmental drivers of systematicity and generalization in a situated agent. arXiv preprint arXiv:1910.00571. https://arxiv.org/abs/1910.00571 . DeepMind.
* Ye, C., Khalifa, A., Bontrager, P., & Togelius, J. (2020). Rotation, Translation, and Cropping for Zero-Shot Generalization. arXiv preprint arXiv:2001.09908. https://arxiv.org/abs/2001.09908
I wonder what are the implications for graphical networks. Arguably, RNNs and sliding attention-based convolutional networks are already like 1st-person for texts. But what about graphs? Does it mean that graph-crawlers can be better than holistic processors?

## RNNs, Attention
#attention

Kitaev, N., Kaiser, Ł., & Levskaya, A. (2020). Reformer: The Efficient Transformer. arXiv preprint arXiv:2001.04451.
https://arxiv.org/pdf/2001.04451.pdf
Potentially a newer link:
https://openreview.net/forum?id=rkgNKkHtvB
Google research. Claim that somehow by two changes in logic they improved [[transformers]] so much that now they would be O(L log L) instead of O(L²) efficient, which is like a big deal.

https://distill.pub/2019/memorization-in-rnns/
How LSTM networks remember text: a visual intro.

Hafner, D., Irpan, A., Davidson, J., & Heess, N. (2017). Learning hierarchical information flow with recurrent neural modules. In Advances in Neural Information Processing Systems (pp. 6724-6733).
https://papers.nips.cc/paper/7249-learning-hierarchical-information-flow-with-recurrent-neural-modules.pdf
ThalNet, which like a virtual thalamus. Only cited by 4 since 2017, so doesn't look like it caught on.

Jaderberg, M., Simonyan, K., & Zisserman, A. (2015). Spatial transformer networks. In Advances in neural information processing systems (pp. 2017-2025).
https://arxiv.org/pdf/1506.02025.pdf
2000 citations. Seems important.

Single Headed Attention RNN: Stop Thinking With Your Head. S Merity. (2019)
https://arxiv.org/abs/1911.11423
He tried to diversify the field by deliberately sticking to a non-mainstream approach (something that is NOT transformers), and got good performance. Also the writing is wild.

Yang, Y., Sautière, G., Ryu, J. J., & Cohen, T. S. (2019). Feedback Recurrent AutoEncoder. arXiv preprint arXiv:1911.04018.
https://arxiv.org/abs/1911.04018
Qualcomm AI Research.

On the Relationship between Self-Attention and Convolutional Layers (2020)
Jean-Baptiste Cordonnier, Andreas Loukas, Martin Jaggi
https://arxiv.org/pdf/1911.03584.pdf
EPFL. Describe that attention networks spontaneiously develop convolution?

Lipton, Z. C., Berkowitz, J., & Elkan, C. (2015). A critical review of recurrent neural networks for sequence learning. arXiv preprint arXiv:1506.00019.
[https://arxiv.org/pdf/1506.00019.pdf](<https://arxiv.org/pdf/1506.00019.pdf>)
Great review for learning about RNNs (could be a textbook) 700 refs.

Press, O., Smith, N. A., & Levy, O. (2019). Improving Transformer Models by Reordering their Sublayers. arXiv preprint arXiv:1911.03864.
https://ofir.io/sandwich_transformer.pdf
Facebook AI. An interesting tiny paper where all they did, I think, is play with the sequence of 2 common blocks, to show that the optimal sequence is not what everyone expected. May be neat. But I prob need to understand how transformers work to get it.

Arabshahi, F., Lu, Z., Singh, S., & Anandkumar, A. (2019). Memory Augmented Recursive Neural Networks. arXiv preprint arXiv:1911.01545.
https://arxiv.org/abs/1911.01545

Compressive Transformers for Long-Range Sequence Modelling
by Jack W. Rae, Anna Potapenko, Siddhant M. Jayakumar, Timothy P. Lillicrap
https://arxiv.org/abs/1911.05507
Deepmind

# AEs, GANs, Generation
#gan

A Review on Generative Adversarial Networks: Algorithms, Theory, and Applications. Jie Gui, Zhenan Sun, Yonggang Wen, Dacheng Tao, Jieping Ye. 20 Jan 2020
https://arxiv.org/abs/2001.06937
Most recent review of the field.

Reinke, C., Etcheverry, M., & Oudeyer, P. Y. (2019). Intrinsically Motivated Exploration for Automated Discovery of Patterns in Morphogenetic Systems. arXiv preprint arXiv:1908.06663.
arxiv.org/abs/1908.06663
Inria, France.
Used an intrinsically motivated AI to explore Game Of Life to find cool patterns. The most interesting part (judging from the abstract) is that it developed a non-obvious representation, just looking for novelty (or whatever it used: looking for something interesting). Would be super-cool to try it for my networks, huh?

PCGRL: Procedural Content Generation via Reinforcement Learning. Ahmed Khalifa, Philip Bontrager, Sam Earle, Julian Togelius. 24 Jan 2020.
https://arxiv.org/abs/2001.09212
Independent researchers. Treat game level generation as a "game"; then try 3 different approaches (?) to solve this game with Markovian processes and #rl.

Oord, A. V. D., Dieleman, S., Zen, H., Simonyan, K., Vinyals, O., Graves, A., ... & Kavukcuoglu, K. (2016). Wavenet: A generative model for raw audio. arXiv preprint arXiv:1609.03499.
https://arxiv.org/abs/1609.03499
1.4k citations: famous really successful realtime audio generation network from DeepMind.

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

Hoyer, S., Sohl-Dickstein, J., & Greydanus, S. (2019). Neural reparameterization improves structural optimization. arXiv preprint arXiv:1909.04240.
https://arxiv.org/pdf/1909.04240.pdf
They are trying to create some fancy mechanical structures, and instead of directly optimizing the parameters of these structures, they optimize the parameters of an ANN that output these structures. Claim that it works better. Looks fun.

Makhzani, A. (2018). Implicit autoencoders. arXiv preprint arXiv:1805.09804.
https://arxiv.org/abs/1805.09804
Sort of GAN-like architecture that tries to optimize the latent space directly? Interesting, but hard to get from the abstract.

# Bayesian and stats

Negative probabilities (a short essay by Richard Feinman)
http://cds.cern.ch/record/154856/files/pre-27827.pdf

The Case for Bayesian Deep Learning
Andrew Gordon Wilson. January 11, 2020
https://cims.nyu.edu/~andrewgw/caseforbdl/
Serious blog post with some (non-scary) math.

A Sober Look at Bayesian Neural Networks. January 17, 2020
by Carles Gelada and Jacob Buckman
https://jacobbuckman.com/2020-01-17-a-sober-look-at-bayesian-neural-networks/
A response to Wilson 2020 above! :) Best type of scientific exchange! Also a blog post, also with math.

Fort, S., Hu, H., & Lakshminarayanan, B. (2019). Deep Ensembles: A Loss Landscape Perspective. arXiv preprint arXiv:1912.02757. 
https://arxiv.org/pdf/1912.02757.pdf
How Bayesian networks help to understand ensemble effects in deep learning (as they learn the distributino of parameters instead of instances?). Some lovely, inspiring pictures.

Bayesian Deep Learning and a Probabilistic Perspective of Generalization
Andrew Gordon Wilson, Pavel Izmailov (2020)
https://arxiv.org/abs/2002.08791
Also a tweeprint: https://twitter.com/andrewgwils/status/1230669857840123906
Something interesting about generalization and priors; I didn't quite understand it, so return to the tweetprint after reading the paper. They seem to be talking to people in the narrow field even while tweetprinting, unfortunately :)

# Alternative network designs

**Helmholtz Machines**

Kevin G. Kirby (2006). A Tutorial on Helmholtz Machines 
https://www.nku.edu/~kirby/docs/HelmholtzTutorialKoeln.pdf

Dayan, P., Hinton, G. E., Neal, R. M., & Zemel, R. S. (1995). The helmholtz machine. Neural computation, 7(5), 889-904.
http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.34.1800&rep=rep1&type=pdf
Original paper by Hinton, 1k citations.

# Meta, Meaning, CogSci
#meaning

Artificial Intelligence, Values and Alignment. Iason Gabriel. 2020
https://deepmind.com/research/publications/Artificial-Intelligence-Values-and-Alignment

Arjovsky, M., Bottou, L., Gulrajani, I., & Lopez-Paz, D. (2019). Invariant risk minimization. arXiv preprint arXiv:1907.02893.
https://arxiv.org/abs/1907.02893
On how to make ML models less racist (some mathematical way to eliminate spurious correlations)

Turing, A. M. (1950). Computing machinery and intelligence. In Parsing the Turing Test (pp. 23-65). Springer, Dordrecht. 2005.
http://www.geielettronica.it/images/pdf/turing.pdf
Said to be a really nicely written paper, seminal, good examples, and with 13k citations.

Do language network learn what they think they learn?
https://thegradient.pub/nlps-clever-hans-moment-has-arrived/

Peterson, J. C., Battleday, R. M., Griffiths, T. L., & Russakovsky, O. (2019). Human uncertainty makes classification more robust. In Proceedings of the IEEE International Conference on Computer Vision (pp. 9617-9626).
https://arxiv.org/abs/1908.07086
On how categorization by humans (compareed to any single ANN) seems to benefit from uncertainty. Propose training on edge cases, which sounds like boosting. But also in the abstract (I haven't opened the paper yet) mention something abou increasing uncertainty (?) that sounds suprisingly as if meta-learning for harnessing uncertainty as a factor may help. Is it true, or is it just a turn of a phrase?

Sinz, F. H., Pitkow, X., Reimer, J., Bethge, M., & Tolias, A. S. (2019). Engineering a less artificial intelligence. Neuron, 103(6), 967-979.
http://xaqlab.com/wp-content/uploads/2019/09/LessArtificialIntelligence.pdf
Review / Opinion piece on differences between intelligence as it operates in biological brains, and what modern AI offers. Very neuro-style, but it's more about AI than about neuro, so I file it here.

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

Axelrod, R., & Hamilton, W. D. (1981). The evolution of cooperation. science, 211(4489), 1390-1396.
41k citations. Also Henry's one single favorite paper ever :)

Zenil, H., & Villarreal-Zapata, E. (2013). Asymptotic behavior and ratios of complexity in cellular automata. International Journal of Bifurcation and Chaos, 23(09), 1350159.
https://arxiv.org/pdf/1304.2816.pdf
Some measures of complexity (Shannon's block entropy and Kolmogorov) applied to 1D cellular automata. They try to make some general statements. Give a look.

Cellular automata for music (a review from 2005, but seems fun)
https://web2.qatar.cmu.edu/~gdicaro/15382/additional/generative-music-ca-review.pdf

# Math and Other

Eta-trick (η-trick) to smooth out local minima in the minima landscape, and improve convergence. Reformulates a non-quadratic problem as a series of quadratic problems.
https://francisbach.com/the-%CE%B7-trick-or-the-effectiveness-of-reweighted-least-squares/

Xiaoying PuMatthew Kay (2020). A Probabilistic Grammar of Graphics.
https://osf.io/dy8qv/
What visuals are better for presenting different types of conditional probabilities (interactions between various variables), with an inspiring zoo-gallery of solutions.