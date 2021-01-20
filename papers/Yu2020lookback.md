# Looking back to lower-level information in few-shot learning

Yu, Z., & Raschka, S. (2020). Looking back to lower-level information in few-shot learning. arXiv preprint arXiv:2005.13638.
https://www.mdpi.com/2078-2489/11/7/345

#embedding #self-supervised #transfer #graphs

Related: [[09_Graphs]], [[transfer]], [[08_Unsupervised]]

Nicely abbreviate Few-Shot Learning as **FSL**. Few examples per class. Most current approaches use **meta-learning** (learn to learn, fancy architectures), and **transfer learning**. Claim that **Graph Networks** may have a benefit here, as the number of labels is by definition small (and Graph Networks can only do small - that's the problem with them).

Interesting references:
* DN4 "Li Deep Nearest Neighbor Network" (30)
* Dense Classification network by Lifchitz (31)

It seems (from the intro) that in these 2 papers, they looked at pairwise similarity between data points based on the activation of the last layer. Then - **Iterative label propagation**. But here they seem to propose that in all FSL methods we should look not only at the representation at the last layer, but at representation at early layers as well, and optimize them somehow, preparing them for transfer.

**Meta-learning** is actually different than both self-learning and transfer learning. The idea is that you train the network to identify some classes, but then for the test, ask it to learn and identify a different, non-overlapping set of classes. The idea is that unsupervised learning (somethin similar to clustering or representation optimization) and prior info on projecting it into classes should help with learning new classes. It's called **N-way-k-shot FSL**.

Other related references:
* MAML, Reptile (Graph-based meta-learning)

_I skipped the math and graph stuff for now. They build graphs and graph Laplacians..._

# Refs

Original tweet:
https://twitter.com/rasbt/status/1278687522239983617

Two key citations:
* Li, W.; Wang, L.; Xu, J.; Huo, J.; Gao, Y.; Luo, J. Revisiting local descriptor based image-to-class measure for few-shot learning. In Proceedings of the 2019 IEEE/CVF Conference on Computer Vision and Pattern Recognition, Long Beach, CA, USA, 16–20 June 2019; pp. 7253–7260.
* Lifchitz, Y.; Avrithis, Y.; Picard, S.; Bursuc, A. Dense classification and implanting for few-shot learning. In Proceedings of the 2019 IEEE/CVF Conference on Computer Vision and Pattern Recognition, Long Beach, CA, USA, 16–20 June 2019; pp. 9250–9259.