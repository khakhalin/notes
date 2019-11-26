# Understanding the generalization of ‘lottery tickets’ in neural networks

November 25, 2019, Ari Morcos, Yuandong Tian
https://ai.facebook.com/blog/understanding-the-generalization-of-lottery-tickets-in-neural-networks

#blog #ticket

A summary of several of their papers.

**Lottery ticket hypothesis**: one can find a 10-100 times smaller network with same loss, if it's trained from a "lucky" initial configuration. 

It's not about pruning, although related. With pruning, you remove unhelpful weights (up to 99% of them, really??) With lottery ticket, you start with a small network and train it, but have a "lucky" (trainable) original configuration. And it outperforms pruning. How to find it? By trying many starting points ("tickets") and training again and again, which obv requires lots of compute.

Of course these "tickets" would be more helpful if they generalized across data. Now they show that they do actually generalize to some extent (in their case, from one images dataset to another: CIFAR-10 to ImageNet). Tickets from larger datasets generalize better. More image classes (with n images kept constant) also generalized better.

Tickets were first described in image processing, but now they show that they exist in RL and NLP domains as well. Also outpefrom pruning. For NLP, their "ticket" is only 60% smaller than the full network (and the full one is 2e8 parameters), so perhaps not as impressive as for images?

To understand how tickets work, they trained a *larger* network as a student, from a *smaller* network used as a teacher (so the opposite of distillation). Found that neurons in a student network are more correlated with neurons in the smaller network than with each other, meaning that the larger network actually emulates the teacher directly, not just via learning from it. And this *specialization* (neurons in network 2 impersonating neurons from network 1) depends on the original weights to these neurons in the student network (**they should by chance happen to be close to that in the teacher network**). Which makes it sorta a "reverse ticket situation". It appears they also did some theoretical analysis of it.


### Refs:

Frankle, J., & Carbin, M. (2018). The lottery ticket hypothesis: Finding sparse, trainable neural networks. arXiv preprint arXiv:1803.03635.

Morcos, A. S., Yu, H., Paganini, M., & Tian, Y. (2019). One ticket to win them all: generalizing lottery ticket initializations across datasets and optimizers. arXiv preprint arXiv:1906.02773.

Yu, H., Edunov, S., Tian, Y., & Morcos, A. S. (2019). Playing the lottery with rewards and multiple languages: lottery tickets in RL and NLP. arXiv preprint arXiv:1906.02768.

Tian, Y., Jiang, T., Gong, Q., & Morcos, A. (2019). Luck Matters: Understanding Training Dynamics of Deep ReLU Networks. arXiv preprint arXiv:1905.13405.

Tian. Student Specialization in Deep ReLU Networks With Finite Width and Input Dimension
https://arxiv.org/abs/1909.13458