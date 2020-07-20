# Word2vec

#text #embedding

Parent: [[10_Text]]
See also: [[node2vec]], [[triplet_loss]], [[softmax]]

Developed by Google; works with "bags of words". A representation learning, where we train 2 layers, - a linear (no activation!) dense layer followed by a softmax,  - to use the dense layer later for the latent-space projection. The thing we train it on is to predict the word nearby (one of the words nearby picked at random). Not necessarily the "nearest" word, but one somewhat close within the sentence.

In practice, Word2vec uses a few tricks to make this learning work:
* For a given window (target word ± width), words are skipped with probability proportional to their frequency (aka **skip-gram**). Articles in English, for example, will be skipped, as they are very frequent.
* To get more efficient, when predicting a word based on the context, instead of normal [[softmax]] (that would require comparison of all probabilities for all possible words) they use softmax approximation. It is similar to [[hierarchical_softmax]] (that reduces the number of terms in the sum in the determinant from the total number of words W to ~$\log_2 W$), but their approach is more efficient, and is called:..
* **Negative sampling**. In order not to update all words that were predicted incorrectly (which would have been computationallly prohibitive), pick a few (2-10?) wrong words at random, and only update them. Say, 5 random words that we have NOT met in this sentence (or rather, within this word window).

In practice, negative sampling works with softmax factorization similar to that of [[hierarchical_sofmax]], but considers a log of it, which turns the product into a sum. We are trying to maximize this:

$\displaystyle \log P(x|z) = \log σ(v'_x {}^⊤ v_z) + \sum_i 𝔼_{w_i~P_n(w)}[\log σ(-v'_{w_i} {}^⊤ v_z)]$

# Refs

Goldberg, Y., & Levy, O. (2014). word2vec Explained: deriving Mikolov et al.'s negative-sampling word-embedding method. arXiv preprint arXiv:1402.3722.
https://arxiv.org/pdf/1402.3722.pdf
(This paper explicitly doesn't even attempt to explain hierarchical softmax, but explains the negative sampling)

Original word2vec citations:
* Site: https://code.google.com/archive/p/word2vec/
* Paper 1: Tomas Mikolov, Kai Chen, Greg Corrado, and Jeffrey Dean. Efficient estimation of word representations in vector space. CoRR, abs/1301.3781, 2013. - 17k citations. Only results (tables, numbers), but no explanation of what's inside, and how it works. https://arxiv.org/pdf/1301.3781.pdf
* Mikolov, T., Sutskever, I., Chen, K., Corrado, G. S., & Dean, J. (2013). Distributed representations of words and phrases and their compositionality. In Advances in neural information processing systems (pp. 3111-3119). - 22k citations https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf

https://towardsdatascience.com/word2vec-skip-gram-model-part-1-intuition-78614e4d6e0b

Tensorflow tutorial:
https://www.tensorflow.org/tutorials/word2vec/index.html