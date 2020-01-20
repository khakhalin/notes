# TFIDF
#text

TF-IDF = Text frequency - Inverse document frequency. A measure of how often a word is used in a document, compared to all other documents. tfidf() = tf()∙idf(). Here:

* tf() = **text frequency**, which is usually this word frequency (% of all words), or log() of it, or normalized frequency, or just raw count, or maybe even binary (yes or now)
* idf() = **inverse document frequency** = the share of documents having this word, but flipped and log-ed: idf() = log(N/(1+n)) + 1, where N is the total number of documents, and n is the number of documents containing this word. 

Can be thoroughly justified via conditional entropy. Refs: [wiki](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)
