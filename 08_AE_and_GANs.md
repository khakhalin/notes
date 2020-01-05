# Autoencoders and GANs

# Embedding layers
Solves the problem of data sparsity (to avoid direct training on long one-hot encoded vectors). A bottleneck layer from an N-hot layer to learn an embedding. Number of units in the bottleneck = number dimensions in the enbedding. Empirical rule of thumb: n dim ~ (n possible values)^(1/4). _Where does it come from?_ With too few dimensions you won't capture the complexity; with too many - risk of overfitting, and longer to train. N dimensions should be treated as a hyperparameter, and optimized.

Classical training: get hold of some similarity or co-occurance measure; use to produce groups, then "withhold some", predicting missing members of a group based on those that are visible. But look also: [[Weng2019self-supervised.md]], triplet loss, #self-supervised .

## Word2vec
Developed by Google; works with "bags of words". See: [tensofrlow tutorial](https://www.tensorflow.org/tutorials/word2vec/index.html)

# GANs
#gan
Odena, "Open Questions about Generative Adversarial Networks", Distill, 2019.
A nice 	short review.
https://distill.pub/2019/gan-open-problems/
* Good in modeling within one type of object (CelebA), worse if data is diverse and sparse (Imagenet)
* Problems with discrete output (text), but doable through gradient estimation
* As of April 2019, they know only one application to graphs
* Training may diverge; convergence guarantees aren't clear (?)