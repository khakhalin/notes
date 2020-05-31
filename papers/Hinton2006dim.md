# Hinton 2006: Dim reduction with RNNs

Hinton, G. E., & Salakhutdinov, R. R. (2006). Reducing the dimensionality of data with neural networks. science, 313(5786), 504-507.
https://dbirman.github.io/learn/hierarchy/pdfs/Hinton2006.pdf

#dim #representation

Seminal paper describing how RNNs can generate representations; 12k citations. Main reasons it is cited:
* That dimensionality reduction with RNNs is possible at all
* That in a deep network, deeper layers contain more abstract representation of the data
* That one can pre-train layers in an unsupervised way, then fine-tune

Basically they build a deep autoencoder (7 fully connected layers deep, going from 2000 to 30 units, then back), but are saying that it's hard to train (as it was pre- convolutional networks, and pre proper initializations, so their gradients vanished). So they pre-train weights connecting every two layers using a **Restricted Boltzmann Machine** (RBM). RBM is not a proper feedforward network, but instead you take 2 patterns of activation (they call them "images"), and calculate the **energy** of these two patterns, giving the parameters of the network:

$\displaystyle E(x,y) = -\sum_{i}{b_i x_i} - \sum_{j}{b_j y_j} - \sum_{i,j}{x_i w_{ij} y_j}$

Now one can sample through different X, and find an X that better corresponds to a given Y.

Then they train in a there-and-back-again fashion: pick training image as X; calcualte Y; threshold it to binary using logistic function, then "confabulate" the best Ξ that would correspond to this Y (again, using logistic function, but backwards). Then adjust the weights, bringing ⟨X,Y⟩ closer to ⟨X*,Y⟩, with a certain learning rate. And then once layer i learned, use its output (real ℝ, not binary) as a new input for layer i+1. 

> Essentially, it seems like a sequence of weird symmetric one-layer-deep autoencoders, trained in a greedy way.

Finally, as each sets of weights works both for encoding and for decoding, they can be used as proxies (initialization) not only for the endoding part of an autoencoder, but also for the reverse funnel. They used cross-entropy error $E = -\sum p_i \log q_i - \sum (1-p_i) \log (1-q_i)$ where p_i is i-th pixel intensity (in 0:1) and q_i is its reconstruction. And they also applied the same model to news articles of different topics, using top 2000 thematic words as a feature vector.

But the most interesting (for the time) part of the paper seems that they compared the representation to PCA, and noticed two things. One, is that PCA gives a much worse reconstruction (coz it's linear?). And second, that you can visualize it if you make an extreme two-dimensional autoencoder, and the visualization shows much clearer separation between categories in the latent space.

> I'm guessing, there should be a simple way to identify two top explanatory dimensions for a higher-dimension autoencoder. Would it be by the average amplitude of activation? Feels like it's probably a solved problem in [[interpretability]], no?