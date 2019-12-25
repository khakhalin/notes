# Texts and Language

## TFIDF
https://en.wikipedia.org/wiki/Tf%E2%80%93idf
TF-IDF = Text frequency - Inverse document frequency. A measure of how often a word is used in a document, compared to all other documents. tfidf() = tf()∙idf(). 

Here tf() = text frequency, which is usually this word frequency (% of all words), or log() of it, or normalized frequency, or just raw count, or maybe even binary (yes or now)

idf() = inverse document frequency = the share of documents having this word, but taken logirithmically: idf() = log(N/(1+n)) + 1, where N is the total number of documents, and n is the number of documents containing this word.

Can be linked (and justified through) conditional entropy.

## N-grams
https://en.wikipedia.org/wiki/N-gram
One of the simpler ways of text analysis: just take n consecutive words (or characters, DNA bases, aminoacids etc), and count the frequency of all n-pairs. It's obviously better than just word or letter statistics (that can be considered an 1-gram), but worse than actually understanding the meaning, or projecting larger bags-of words into semantic space DL-style.

Example: how to tell whether a text belongs to a corpus of texts? Cut the corpus into chunks of similar size, calculate cosine distances (inner products) of each bootstrapped chunk with the full corpus, find the sd. Then calculate cosine distance for the text in advance, go to z-score, and get a p-value that this text also belongs to the corpus. Or, alternatively, one can also calculate mean and sd for eacn n-gram independently, then find z-scores for text in question, and thus identify unusual n-grams.

Variants: **Skip-grams** - take a sequence of n words, but eliminate (skip) k words from it. Thus, (3,1) skip-grams from a sequence of 3 words would return 3 sequences: two 2-grams + one 1-skip-2-gram.

An obvious problem with noisiness (some n-grams would never be observed), so there exist various methods of smoothing. I haven't read about them yet, but for example:
https://en.wikipedia.org/wiki/Good%E2%80%93Turing_frequency_estimation