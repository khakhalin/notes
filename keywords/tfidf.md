# TFIDF
#text

TF-IDF = **Text Frequency - Inverse Document Frequency**. A measure of how often a word is used in a document, compared to all other documents. tfidf() = tf()∙idf(). Here:

* tf() = **text frequency**, which is usually this word frequency in the document (% of all words), or log() of it, or normalized frequency, or just raw count of words, or maybe even binary (yes or no)
* idf() = **inverse document frequency** = usually, the share of documents having this word, but flipped and log-ed: idf() = log(N/(1+n)) + 1, where N is the total number of documents, and n is the number of documents containing this word ([wiki](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) has several more alternatives).

Apparently can be justified via conditional entropy. 
