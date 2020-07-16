# Word2vec

#text #embedding 

Parent: [[10_Text]]
See also: ???

#todo

Developed by Google; works with "bags of words". See: [tensofrlow tutorial](https://www.tensorflow.org/tutorials/word2vec/index.html)

# Skip-gram

Some sort of representation learning, where we train 2 layers - a linear (no activation!) dense and a softmax, to use the dense layer later for the latent-space projection, while the softmax layer is discarded (_Is it true??_). The thing we train it on is to predict the word nearby (one of the words nearby picked at random). Not necessarily the "nearest" word, but one somewhat close within the sentence.

_So it's like a correlation, right? Except a correlation on a giant sparse vector several thousand words-long…_

In practice, Word2vec uses a few tricks to make this learning work:
* For a given window (target word ± width), words are skipped with probability proportional to their frequency. Articles in English, for example, will be skipped.
* **Negative sampling**: in order not to update all words that were predicted incorrectly (which would have been computationallly prohibitive), we pick a few (2-10?) smalll words at random, and only update them. Say, 5 random words that we have NOT met in this sentence (or rather, within this word window).

Refs:
* https://towardsdatascience.com/word2vec-skip-gram-model-part-1-intuition-78614e4d6e0b