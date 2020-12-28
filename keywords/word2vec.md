# Word2vec

#text #embedding

Parent: [[10_Text]]
See also: [[node2vec]], [[softmax]], [[neg_sampling]]

Developed by Google; works with "bags of words". A representation learning, where we train 2 layers, - a linear (no activation!) dense layer followed by a softmax, - to use the dense layer later for the latent-space projection. The thing we train it on is to predict the word nearby (one of the words nearby picked at random). Not necessarily the "nearest" word, but one somewhat close within the sentence.

In practice, Word2vec uses a few tricks to make this learning work:
* For a given window (target word ± width), words are skipped with probability proportional to their frequency (aka **skip-gram**). Articles in English, for example, will be skipped, as they are very frequent.
* To get more efficient, when predicting a word based on the context, instead of normal [[softmax]] (that would require comparison of all probabilities for all possible words) they use **softmax approximation**. It is similar to [[hierarchical_softmax]] (that reduces the number of terms in the sum in the determinant from the total number of words W to ~$\log_2 W$), but their approach is more efficient, and is called:..
* **Negative sampling** ([[neg_sampling]]). In order not to update all words that were predicted incorrectly (which would have been computationallly prohibitive), pick a few (2-10?) wrong words at random, and only update them. Say, 5 random words that we have NOT met in this sentence (or rather, within this word window).

> The paper is annoying in this regard, as it introduces hierarchical softmax, and also introduces it with some really annoying notation, but then does not use it. It is just something they tried earlier, and something that kinda informed their search for a "better solution", but, as Yannic Kilcher puts it, "it is a distraction". As it is not actually used.

In practice, **negative sampling** works with softmax factorization that kinda similar to that of [[hierarchical_sofmax]], but considers a log of factorized P(x|z), and this turns turns the product of factors into a sum of log-factors. We then try to maximize this sum:

$\displaystyle \log P(x|z) = \log σ(v'_x {}^⊤ v_z) + \sum_{i=1}^k 𝔼_{w_i\sim P_n(w)}[\log σ(-v'_{w_i} {}^⊤ v_z)]$ . Here:
* x - the word nearby the word of interest (the neighbor we're trying to predict)
* z - the word of interest itself (the ancor of sorts)
* v_x and v_z - their vector representations (and v_w for summation as well)
* σ() - a standard sigmoid function
* 𝔼 w_i ~ Pn(w) - sampling words (negative, noisy) from the vocabulary at random
* k - how many "negative word" we are sampling

> Why do they use sigmoids, in particular? It appears, mostly because they worked. Even tough they probabily spiritually inherit to hierarchical softmax.

And if everything is done correctly, then the vector representations {v_w} or words {w} acquire meaningful dimensions. The example they show is a 2D PCA of all words for countries and capitals that shows that there is a dimension that pretty much encodes which city is a capital of each country (Warsaw-Poland+Spain = Madrid; both the direction, and the length).

> Apparently there were some follow-up works that showed that these vector calculations are much less impressive than it was originally reported (especially the famous King-Queen example :)

How to sample negative samples? Uniformly? Or according to their frequency in the text? They use something in-between, as apparently it worked better than either extrme. In practice they used a **unigram distribution** (frequency of words) **raised to the power of 3/4**, and it worked magically better than all the alternative, and we still don't know why. But on top of that, they also discarded most frequent word using a yet another empirical and unproven formula that just happened to work.

# Refs

Goldberg, Y., & Levy, O. (2014). word2vec Explained: deriving Mikolov et al.'s negative-sampling word-embedding method. arXiv preprint arXiv:1402.3722.
https://arxiv.org/pdf/1402.3722.pdf
(This paper explicitly doesn't even attempt to explain hierarchical softmax, but explains the negative sampling)

Original word2vec citations:
* Site: https://code.google.com/archive/p/word2vec/
* Paper 1: Tomas Mikolov, Kai Chen, Greg Corrado, and Jeffrey Dean. Efficient estimation of word representations in vector space. CoRR, abs/1301.3781, 2013. - 17k citations. Only results (tables, numbers), but no explanation of what's inside, and how it works. https://arxiv.org/pdf/1301.3781.pdf
* Mikolov, T., Sutskever, I., Chen, K., Corrado, G. S., & Dean, J. (2013). Distributed representations of words and phrases and their compositionality. In Advances in neural information processing systems (pp. 3111-3119). - 22k citations https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf

Recent (2020) video-description of the paper by Yannic Kilcher:
https://www.youtube.com/watch?v=yexR53My2O4

https://towardsdatascience.com/word2vec-skip-gram-model-part-1-intuition-78614e4d6e0b

Tensorflow tutorial:
https://www.tensorflow.org/tutorials/word2vec/index.html