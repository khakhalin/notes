# Texts and Language

**Basic methods**
* [[ngram]] - N-grams: low-level feature for text analysis, beyond single words. "Bags" of several (2-3) words.
* [[tfidf]] - Text frequency - Inverse document frequency. A basic ranking approach for text relevance.

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

**Beam Search**: alpha-go-like (lol) formally coded tree of solutions for picking the best sequence, not just the best word, during translation:
Yonghui Wu, Mike Schuster, Zhifeng Chen, Quoc V Le, Mohammad Norouzi, Wolfgang Macherey, Maxim Krikun, Yuan Cao, Qin Gao, Klaus Macherey, et al. Google’s neural machine
translation system: Bridging the gap between human and machine translation. arXiv preprint
arXiv:1609.08144, 2016. 