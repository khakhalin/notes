# N-grams

#text

Parent: [[text]]
See also: [[trie]], [[beam_search]]

https://en.wikipedia.org/wiki/N-gram
One of the simpler ways of text analysis: just take n consecutive words (or characters, DNA bases, aminoacids etc), and count the frequency of all n-pairs. It's obviously better than just word or letter statistics (that can be considered an 1-gram), but worse than actually understanding the meaning, or projecting larger bags-of words into semantic space DL-style.

Example: how to tell whether a text belongs to a corpus of texts? Cut the corpus into chunks of similar size, calculate cosine distances (inner products) of each bootstrapped chunk with the full corpus, find the sd. Then calculate cosine distance for the text in advance, go to z-score, and get a p-value that this text also belongs to the corpus. Or, alternatively, one can also calculate mean and sd for eacn n-gram independently, then find z-scores for text in question, and thus identify unusual n-grams.

Variants: **Skip-grams** - take a sequence of n words, but eliminate (skip) k words from it. Thus, (3,1) skip-grams from a sequence of 3 words would return 3 sequences: two 2-grams + one 1-skip-2-gram.

An obvious problem with noisiness (some n-grams would never be observed), so there exist various methods of smoothing. I haven't read about them yet, but for example:
https://en.wikipedia.org/wiki/Good%E2%80%93Turing_frequency_estimation