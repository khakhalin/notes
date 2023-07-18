# Texts and Language

#text #bib


# Methods and concepts

Oldschool:
* [[ngram]] - N-grams: low-level feature for text analysis, beyond single words. "Bags" of several (2-3) words.
* [[tfidf]] - **Text Frequency - Inverse Document Frequency**. A basic approach to ranking documents by their relevance for a given keyword.
* [[stupid_back_off]] - a simplistic Markovian approach to text generation

Word embeddings ([[embedding]]):
* [[word2vec]] - famous word embedding method developed by Google.
* [[transE]] - abstract knowledge graphs (not necessarily on words)
* [[debiasing]] - how to make word embeddings less biased

Modern DL:
* [[LLM]] - Large language models
* [[beam_search]] - beyond suggesting one word at a time, by exploring a decision tree.
* [[perplexity]] - main measure (objective function) for language model quality
* [[transformers]] - as of 2023, the most popular architecture for LLM

# To read:

Non-technical, but supposedly detailed, introduction into modern text methods:
* https://machinelearningmastery.com/statistical-language-modeling-and-neural-language-models/
* https://paperswithcode.com/task/language-modelling
* https://www.topbots.com/leading-nlp-language-models-2020/
* https://web.stanford.edu/~jurafsky/slp3/
* https://raohacker.com/why-the-new-ai-nlp-language-model-gpt-3-is-a-big-deal/
* https://jalammar.github.io/a-visual-guide-to-using-bert-for-the-first-time/
* https://jalammar.github.io/illustrated-bert/
* https://cacm.acm.org/magazines/2020/6/245162-contextual-word-representations/fulltext

Chiang, C. H., & Lee, H. Y. (2020). Pre-Training a Language Model Without Human Language. arXiv preprint arXiv:2012.11995. https://arxiv.org/pdf/2012.11995.pdf

* https://ruder.io/unsupervised-cross-lingual-learning/index.html
* https://ruder.io/state-of-transfer-learning-in-nlp/index.html
* https://www.aclweb.org/anthology/P19-1334/ 

Two posts about **BERT** by Jesse Vig:
* https://towardsdatascience.com/openai-gpt-2-understanding-language-generation-through-visualization-8252f683b2f8
* https://towardsdatascience.com/deconstructing-bert-part-2-visualizing-the-inner-workings-of-attention-60a16d86b5c1

BERT paper itself (a combo of linear and graphical embedding?)
https://arxiv.org/pdf/1810.04805.pdf

Attacking Bert: Weight Poisoning Attacks on Pre-trained Models
Kurita, K., Michel, P., & Neubig, G. (2020). Weight poisoning attacks on pre-trained models. arXiv preprint arXiv:2004.06660.
https://arxiv.org/abs/2004.06660
https://github.com/huggingface/awesome-papers/discussions/8
https://colab.research.google.com/drive/1BzdevUCFUSs_8z_rIP47VyKAlvfK1cCB?usp=sharing
https://github.com/QData/TextAttack

McCoy, R. T., Pavlick, E., & Linzen, T. (2019). Right for the Wrong Reasons: Diagnosing Syntactic Heuristics in Natural Language Inference. arXiv preprint arXiv:1902.01007.
https://www.aclweb.org/anthology/P19-1334/
Criticism of DL text prediction models.
https://thegradient.pub/nlps-clever-hans-moment-has-arrived/

Mathew, B., Sikdar, S., Lemmerich, F., & Strohmaier, M. (2020). The POLAR Framework: Polar Opposites Enable Interpretability of Pre-Trained Word Embeddings. arXiv preprint arXiv:2001.09876.
https://arxiv.org/pdf/2001.09876.pdf
Blog post:
https://deepai.org/publication/the-polar-framework-polar-opposites-enable-interpretability-of-pre-trained-word-embeddings
Improved word embeddings by somehow forcing (encouraging) the development of semantic opposites (aka "Semantic diffentials", like white-black, that are apparently well-known to be prominent in human cognition / language use). Not quite sure how it works, but there's some dimensionality reduction in the middle, trying to rotate the representation into a set of interpretable axes. Apparently, may be really helpful for the interpretation of language models.

**GloVe** - Global Vectors for Word Representation: a dataset from Standford, which is like word2vec, but pre-calculated, and shared as a dataset ([link](https://nlp.stanford.edu/projects/glove/))

# Analysis of code

(A related, but distinct topic, about using ML methods to analyze (and then check, improve, simplify, suggest) programming code)

Allamanis, M., Barr, E. T., Devanbu, P. T., & Sutton, C. A. (2017). A survey of machine learning
for big code and naturalness. CoRR.
Key review on ANN methods to the analysis, prediction, and debugging of code.