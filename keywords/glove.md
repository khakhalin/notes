# GloVe

Parents: [[10_Text]], [[embedding]]
See also: [[word2vec]]

#text


A word-level pre-trained embedding from Standord (2014).

# Refs

https://en.wikipedia.org/wiki/GloVe

Download link (822 Mb)
http://nlp.stanford.edu/data/glove.6B.zip
The file `glove.68.zip` then needs to be unzipped, and read to numpy with this code:
```python
path_to_glove_file = "~/DOWNLOADS/glove.6B.100d.txt"


embeddings_index = {}
with open(path_to_glove_file) as f:
    for line in f:
        word, coefs = line.split(maxsplit=1)
        coefs = np.fromstring(coefs, "f", sep=" ")
        embeddings_index[word] = coefs
        
embedding_vector = embeddings_index.get(word)
```
Source: https://keras.io/examples/nlp/pretrained_word_embeddings/