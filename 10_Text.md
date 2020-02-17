# Texts and Language
#text

**Toolbox of methods and concepts:**
* [[ngram]] - N-grams: low-level feature for text analysis, beyond single words. "Bags" of several (2-3) words.
* [[tfidf]] - Text Frequency - Inverse Document Frequency. A basic ranking approach for text relevance.
* [[perplexity]] - main measure of language model quality, as well as a great objective function.
* [[beam_search]] - a way to go beyound a level of "one word at a time" by tree exploration.
* [[stupid_back_off]] - a simplistic Markovian approach to text generation

# To read:
https://ruder.io/unsupervised-cross-lingual-learning/index.html

https://ruder.io/state-of-transfer-learning-in-nlp/index.html

https://www.aclweb.org/anthology/P19-1334/ 

Two posts about BERT by Jesse Vig:
* https://towardsdatascience.com/openai-gpt-2-understanding-language-generation-through-visualization-8252f683b2f8
* https://towardsdatascience.com/deconstructing-bert-part-2-visualizing-the-inner-workings-of-attention-60a16d86b5c1

McCoy, R. T., Pavlick, E., & Linzen, T. (2019). Right for the Wrong Reasons: Diagnosing Syntactic Heuristics in Natural Language Inference. arXiv preprint arXiv:1902.01007.
https://www.aclweb.org/anthology/P19-1334/
Criticism of DL text prediction models.
https://thegradient.pub/nlps-clever-hans-moment-has-arrived/

Mathew, B., Sikdar, S., Lemmerich, F., & Strohmaier, M. (2020). The POLAR Framework: Polar Opposites Enable Interpretability of Pre-Trained Word Embeddings. arXiv preprint arXiv:2001.09876.
https://arxiv.org/pdf/2001.09876.pdf
Blog post:
https://deepai.org/publication/the-polar-framework-polar-opposites-enable-interpretability-of-pre-trained-word-embeddings
Improved word embeddings by somehow forcing (encouraging) the development of semantic opposites (aka "Semantic diffentials", like white-black, that are apparently well-known to be prominent in human cognition / language use). Not quite sure how it works, but there's some dimensionality reduction in the middle, trying to rotate the representation into a set of interpretable axes. Apparently, may be really helpful for the interpretation of language models.