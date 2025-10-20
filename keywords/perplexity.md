# Perplexity

Parents: [[text]]
See also: [[transformers]], [[information]]

#text #entropy


Mostly used with text. Intuitively, in its simplest form, just the inverse of geometric-average probability of words in a sentence:

per = P(combination of N words)^(-1/N)

If all words are independent and have the same probability p=1/k, for example, then perplexity = 1/(p^n)^1/n = 1/p = k. So in this case perplexity ≈ vocabulary size.

As multiplying probabilities is lame, usually people do log, sum them, and exp. Except that instead of e^x,  it's more fun to use 2^x.

$\displaystyle \text{per} = 2^{-\frac{1}{N}\sum \log_2 P(w_i)}$

This formula above is usually derived as a partial case of a more general definition, according to which perplexity is just 2 power [[entropy]]: P = 2^H, where H = −∑p∙log(p). The higher the entropy, the higher the perplexity. And we use log base 2. ([ref](https://stats.stackexchange.com/questions/10302/what-is-perplexity)). The lowest theoretical possible perplexity is that for complete certainty, 2^log(1) = 1.

The name originates from a different situation though: when a model is trained on probabilities {q}, but meets a sample with probabilities {q}. The measure of mismatch (the amount of information needed to encode a message form {p} using symbols from {q}) is equal to cross-entropy: H(p,q) = -∑p∙log(q). The higher H, the more confused, perplexed the model is. The better the match between p and q, the smaller the perplexity (as cross-entropy H(p,q) is always larger than entropy H(q) = H(q,q)).

From this pov, "vanilla" perplexity defined above is like calculating cross-entropy of model distribution with an equiprobable distribution, as if the model trained on q = P(w_i) encoutered and tried to encode an equiprobable sequence of words (or sentences), p = 1/N. Which maybe corresponds to a situation when a model engages with a text, and in this text each sentence is represented only once, so the probability of getting every sentence is p = 1/N_sentences? _Not sure about that._

When training models on text, **perplexity goes down for larger ngrams**. For a model trained on English journal articles with ~20000 vocab), for one-words (unigrams) perplexity ~1000, for two-words (bigrams) ~200, for three-words (trigrams) ~100 (ref: Jurafsky). The higher [[ngram]] we consider, the more structure there is, on average, as most P is concentrated in fewer combos, and so −∑log(P(combo_i)) becomes smaller compared to −∑log(P(all equiprobable combos)). In a random text, perplexity would have stayed constant with ngram length incease, but for a structured text, it goes down.

**Perplexity minimization** (for p from test data, and q from model predictions) is a great objective for training text generators. The lower perplexity, that is, the more certain the model is about each next word in the test dataset, the better. Low perplexity strongly correlates with human ratings of bot quality, such as the average of Sensibleness and Specificity measures (SSA), which in turn positively correlates with subjectively assessed "human-likeness" (ref: Adiranwana 2020). For a good model, vocab is ~10k, while perplexity is ~10 (ibid).

Perplexity of two models can only be compared if they use the same vocabulary, and tested on the same testing dataset (Jurafsky).

# Refs

* [Post by Lei Mao](https://leimao.github.io/blog/Entropy-Perplexity/) with some general formulas
* Adiranwana 2020: Towards a Human-like Open-Domain Chatbot. Google Research, Brain Team. Jan 2020.
    * https://arxiv.org/pdf/2001.09977.pdf
    * https://ai.googleblog.com/2020/01/towards-conversational-agent-that-can.html - summary
* Yale: http://www.cs.yale.edu/homes/radev/nlpclass/slides2017/213.pdf
* Standord: https://web.stanford.edu/class/cs124/lec/languagemodeling.pdf
* Both are based on this textbook by Jurafsky and Martin: https://web.stanford.edu/~jurafsky/slp3/3.pdf