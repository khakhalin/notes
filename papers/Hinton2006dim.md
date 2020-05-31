# Hinton 2006: Dim reduction with RNNs

Hinton, G. E., & Salakhutdinov, R. R. (2006). Reducing the dimensionality of data with neural networks. science, 313(5786), 504-507.
https://dbirman.github.io/learn/hierarchy/pdfs/Hinton2006.pdf

#dim #representation

Seminal paper describing how RNNs can generate representations; 12k citations.

Basically built an autoencoder, but are saying that it's hard to train (it was pre- convolutional networks, and pre proper initializations, so their gradients vanish). So they pre-train weights connecting every two layers using a **Restricted Boltzmann Machine** (RBM). RBM is not a proper feedforward network, but instead you take 2 patterns of activation (they call them "images"), and calculate the **energy** of these two patterns, giving the parameters of the network:

$\displaystyle E(x,y) = -\sum_{i}{b_i x_i} - \sum_{j}{b_j y_j} - \sum_{i,j}{x_i w_{ij} y_j}$

Now one can sample through different X, and find an X that better corresponds to a given Y.

Then they train in a there-and-back-again fashion: pick training image as X; calcualte Y; threshold it to binary using logistic function, then "confabulate" the best Ξ that would correspond to this Y (again, using logistic function, but backwards). Then adjust the weights, bringing ⟨X,Y⟩ closer to ⟨X*,Y⟩