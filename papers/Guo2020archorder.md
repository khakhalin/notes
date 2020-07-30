# Revealing architectural order with quantitative label-free imaging and deep learning

Guo, S. M., Yeh, L. H., Folkesson, J., Ivanov, I., Krishnan, A. P., Keefe, M. G., ... & Nowakowski, T. J. (2019). Revealing architectural order with quantitative label-free imaging and deep learning. eLife 2020; 9:e55502

Now in eLife:
https://elifesciences.org/articles/55502

Older reference:
Guo, S. M., Yeh, L. H., Folkesson, J., Ivanov, I., Krishnan, A. P., Keefe, M. G., ... & Nowakowski, T. J. (2019). Revealing architectural order with quantitative label-free imaging and deep learning. bioRxiv, 631101.

#neuro #image

Some fancy microscopic imaging technique that doesn't use fluorescent labels, gene expression, or antibodies, but involves phase and polarization that they call **QLIPP**: quantitative label-free imaging with phase and polarization. Essentially they acquire many images of the same thing, that together contain lots of information about the specimen anisotropy. Then they train a network to reconstruct the geometry of cellular / subcellular structures (axons etc.), using fluorescent-labeled samples as a training set. Eventually, it gives them a throughput to process entire slices of a mouse brain, for example.

Seem to be using their own unique samples, with code developed by other labs:

https://github.com/mehta-lab/reconstruct-order

https://github.com/czbiohub/microDL