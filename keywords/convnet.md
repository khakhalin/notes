# CNN, Convolutional Neural Network, convnets

#dl #convnet

Parent: [[06_DL]]
See also: [[pooling]], [[wavenet]]

Papers:
* [[alexnet]] - the breakthrough paper from 2012
* [[Bai2018sequence]] - convnets consistently outperfm RNNs in temporal sequence modeling

# To read

Zeiler, M. D., & Fergus, R. (2014, September). Visualizing and understanding convolutional networks. In European conference on computer vision (pp. 818-833). Springer, Cham.
https://cs.nyu.edu/~fergus/papers/zeilerECCV2014.pdf

Huang, G., Liu, Z., Van Der Maaten, L., & Weinberger, K. Q. (2017). Densely connected convolutional networks. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 4700-4708).
https://openaccess.thecvf.com/content_cvpr_2017/papers/Huang_Densely_Connected_Convolutional_CVPR_2017_paper.pdf
14k citations. If I get it right, it's about preventing information loss by introducing some additional "skip" connections between early and late layers in a convnet. That somehow improves stuff?

http://cs231n.github.io/convolutional-networks/ - nice concise introduction to CNNs

Simonyan, K., & Zisserman, A. (2014). Very deep convolutional networks for large-scale image recognition. arXiv preprint arXiv:1409.1556. https://arxiv.org/abs/1409.1556
50k citations. A summary of solutions with tables that compare them. No fun pictures. It it practical, or a just a nominal citation?

# ConvNets and biological vision

Bashivan, P., Kar, K., & DiCarlo, J. J. (2019). Neural population control via deep image synthesis. Science, 364(6439). https://www.gwern.net/docs/ai/2019-bashivan.pdf

Khaligh-Razavi, S. M., & Kriegeskorte, N. (2014). Deep supervised, but not unsupervised, models may explain IT cortical representation. PLoS computational biology, 10(11), e1003915. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4222664/

Yamins, D. L., & DiCarlo, J. J. (2016). Using goal-driven deep learning models to understand sensory cortex. Nature neuroscience, 19(3), 356-365. http://brainmind.umin.jp/PDF/wt17/Yamins3.pdf

Pospisil, D. A., Pasupathy, A., & Bair, W. (2018). 'Artiphysiology'reveals V4-like shape tuning in a deep network trained for image classification. Elife, 7, e38242. https://elifesciences.org/articles/38242 - They seem to claim that CNN deep layer neurons selectivity is actually similar to primate V4. Not too wildly cited tho.

# General Refs

Really cool lecture from Brandon Rohrer: https://www.youtube.com/watch?v=JB8T_zN7ZC0
