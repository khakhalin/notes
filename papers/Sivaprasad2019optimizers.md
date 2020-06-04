# Sivaprasad 2019: Benchmarking and hyperparameters

Originally had a different title:
Sivaprasad, P. T., Mai, F., Vogels, T., Jaggi, M., & Fleuret, F. (2019). On the Tunability of Optimizers in Deep Learning. arXiv preprint arXiv:1910.11758.

New name: **Optimizer Benchmarking Needs to Account for Hyperparameter Tuning**
https://arxiv.org/abs/1910.11758

#hyperparameters #optimizers

Related: 
* [[ADAM]], [[Ruder2016descent]] - other comparisons of optimizers
* [[Schneider2019benchmark]] - the benchmark set

It's pretty hard to compare optimizers, as they are sensitive to hyperparameters. But moreover, if you are doing diverse tasks repeadely, you need to optimize your optimizer (tune hyperparameters), and it takes time. Which means that fancier optimizers are naturally disadvantaged: even if there exists a combination of hyperparameters that can reach better pefromance, it doesn't matter if you don't have the budget to find them.

They found that if you put a certain fixed effort into fine-tuning your optimizer, ADAM typically has a highest probaibility of giving you the best peformance (between ~ 0.5 and 0.75), in a race that included SGD with momentum, AdaGrad, ADAM, and weight decay.

Tasks and architectures that they used: **DeepOBS benchmark set** ([[Schneider2019benchmark]]). This includes: ConvNet MNIST, VAE MNIST, Wide Residual Network SVHN (Street View House Numbers), Character RNN (on War and Peace), approximation of some function (???), and LSTMs on IMDB.