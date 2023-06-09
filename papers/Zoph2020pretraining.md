# Rethinking Pre-training and Self-training

Zoph, B., Ghiasi, G., Lin, T. Y., Cui, Y., Liu, H., Cubuk, E. D., & Le, Q. V. (2020). Rethinking Pre-training and Self-training. arXiv preprint arXiv:2006.06882.
https://arxiv.org/abs/2006.06882

#self-supervised #image #transfer


Basic terms:
* **Pre-training**: train (or half-train) a model on one dataset, then train it on a different dataset. Example: **ImageNet** pre-trained networks (say, to about 85% of max performance).
* **Self-training**: When we have more unlabeled data than labeled data, we can first train a model on labeled data, then use predictions of this first model to train a "secondary model" on full data. Aka **pseudo-labels**, or **noisy student** ([[Xie2020noisy_student]])
* **Self-supervised learning**: try to learn some good, universal representations.

In this paper, authors tried to compare these different categories on **object detection** and **semantic segmentation** tasks. Datasets: COCO, OpenImages, ImageNet, 640x640 resolution. Also some "augmented PASCAL". 

For pre-training, they used either
* nothing, 
* or ImageNet at 85% training (image classification problem), 
* or some fancy ImageNet++ (achieved via noisy student, that supposedly eventually trains to a better performance)
For testing they used:
* COCO - object detection / segmentation dataset: that is, not about calling object, but about showing where they are, and maybe even drawing a margin around them (for the segmentation task)
* 4 different degrees of **data augmentation**, from flip+crop+small scale jitter, and down to **RandAugment** with large-scale everything (see: [[Cubik2019randaugment]])

# Findings

**When labeled data is strongly augmented, pre-training hurts performance.** So we have an effect reversal: if data is (almost) not augmented, pre-training may help a lot. But if it is data is strongly augmented (that increases the overall performance of the final model!), pre-training actually hurts.

My comments:
* Their **improvement** for un-augmented data looks a bit sketchy, as it's present only for this fancy ImageNet++, but not for "Normal" ImageNet. Like, if the effect was robust, wouldn't it be present for any pre-training, including basic ImageNet as well? Something is iffy here.
* The **hurt** of performance seems real though (weak but real). Does the hurt for havily augmented data come from the fact that augmented data doesn't "look like" the data on which the network was pre-trained? They call the hurt large, even tho it's about 1% only (from 44% to 43%).
* Also (as they point out), ImageNet-associated task is a less challenging one, so maybe in this case pre-training on an easier task before attempting a harder task is counter-productive, as the network becomes less experessive. They call it **task alignment**: even though tasks feel somewhat similar, they are not the same, and it is enough to make a network fine-tuned for one task, suboptimal for another one.
* They still use a lot of labels for training on the 2nd network: their smallest is 20% of the COCO dataset, which is 118k images × 0.2 = 23k images. But for this "smallest set of labels" the effect of pre-training is indeed the largest (4%: from 32% to 36%). Already for 50% of labels (60k) the difference is practically gone.

**Self-training always helps** My understanding is that in this case they first train on the COCO dataset, and then use ImageNet dataset (but not the labels) for a **noisy student** paradigm. While pre-training on ImageNet gave a ~1% hurt, self-training (post-training) on same (but unlabeled) data gives a 1% boost.

**Joint training** helps even more! In their notation, it's the same as self-training, so the model learns labels for COCO, but then tries to apply them to ImageNet. However in JointTraining it does it not after, but concurrently with supervised training. And apparently it's even better that way, if you still do full self-training after (1-2% more).

# Refs

Good write-up by Aakash Nain:
https://medium.com/@nainaakash012/rethinking-pre-training-and-self-training-53d489b53cbc

COCO dataset:
https://cocodataset.org/#home

He, K., Girshick, R., & Dollár, P. (2019). Rethinking imagenet pre-training. In Proceedings of the IEEE international conference on computer vision (pp. 4918-4927).
https://arxiv.org/pdf/1811.08883.pdf

