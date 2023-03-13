# Local online learning in recurrent networks with random feedback

Murray, J. M. (2019). Local online learning in recurrent networks with random feedback. ELife, 8, e43299.
https://elifesciences.org/articles/43299

Parents: [[credit]], [[echo]]
Related: [[Lillicrap2020backprop]]

#echo #neuro


Classic [[echo]] architecture: x→W→echo→Wou→y, exponentially decaying neurons, tanh() activation. Like fancy-looking equations, even though the logic is simple.

But then tries to apply gradient descent to weights to minimize a difference between output $\hat y$ and actualy $y$. Then tries to make the resulting equation biologically plausible by two corrections:
1. drops non-local term. We refuse to optimize activity of other nodes that feed to this node. We just drop the chain rule term. If this node needs to be more active, and another node was active, increase the weight to this node. That's it. Which means that we just have a Hebb's rule with error as a learning signal, and integration of co-activation over time (aka **eligibility trace**).
2. And instead of looking at output weights, and using them to scale some nodes and not others (which is equivalent to each node "knowing" whether it is important), we use random weights to communicate the error back. 

This learning rule works (less effective than full backprop in time, but still works). If only W_out are trained - learning is worse. If only W (recurrent) - completely unsuccessful (not surprising as they use random error instead of flipped W_out, so signs don't match. This figure is in the supplementary data). But if both are allowed to change, then W_reverse (the random ones) essentially reshape the reservoir, fostering pro-y behavior in some nodes and anti-y in others. Which means that W_out learning, faced with this reservoir, promotes pro-y nodes, and takes anti-y nodes with a negative sign. Which means that over time W_out converges to reversed W_reverse!!

If a network is further constrained with a Dale's law, still works.

Use "ready-set-go" (interval matching) as a test. (One wave, another wave, then Y should predict where the 3d wave will be). Then stitch an architecture of cortex→basal ganglia→thalamus, to learn "behavioral syllabi" for a longer behavior.

# Relevant refs

Similar approaches: 
* Tallec, C., & Ollivier, Y. (2017). Unbiased online recurrent optimization. arXiv preprint arXiv:1702.05043. https://arxiv.org/pdf/1702.05043.pdf
* Mujika, A., Meier, F., & Steger, A. (2018). Approximating real-time recurrent learning with random kronecker factors. Advances in Neural Information Processing Systems, 31, 6594-6603.  https://papers.nips.cc/paper/2018/file/dba132f6ab6a3e3d17a8d59e82105f4c-Paper.pdf

