# Hierarchical softmax

#classification #trees

Parents: [[softmax]], [[word2vec]]
Related: [[node2vec]], [[algos_trees]], [[03_Classification]]

**The idea**: In a normal softmax to figure out whether the point z is likel to go with the point x, you sum over all possible elements in the denominator: $P(z|x) = \exp(z ^\top x)/Σ_w \exp(w ^\top x)$. But what if the total number of points {w} is ridiculously huge (all words in a language, or all nodes in a social network?). It would be nice to reduce this number by not including those definitely won't make the cut (for which exp(wᵀx)~0 ). How to do it?

One possible approach: distribute all points over a bunch of heaps and for every target point first figure out whether it is likely to belong to a reasonable heap $P(C|x)$. Then make all "good candidates", all points that "made it" to the final pool, compete softmax-like. Essentially it forces a factorization $P(z|x) = P(C|x)\cdot P(z|C,x)$ for z ∈ C. It gives a nice saving, and all we need to make it happen is to learn P(C|x) for every x, which can be done using logistic regression. ("C" here originally stood for "Context")

Now, what is the theoretical limit of this approach? A theoretical limit would be a **binary search tree** that would offer a factorization of $P(z|x) = \prod_i P(c_i | x)$ . But how to make the calculation of these $P(c_i | x)$ efficient? Let's pick a basis so that each factor $P(c_i| x_i)$ could be calculated from a comparison of only i first coordinates of z and x. Make all vectors - **binary vectors**, with 0s and 1s at each digit marking whether they belong to the left or to the right subgroup of a tree. 

> In a [[word2vec]] situation, each of these binary dimensions corresponds to a semantic marker that any two words can either share or not. In other words, to work, it has to be a good meaningful embedding.

> I'm not quite sure they are really binary; Morin 2005 says they are binary, but word2vec paper doesn't seem to say so?

We can now collect good candidate elements stochastically via **stochastic sampling** by **descending the tree**, and at each branch - making a stochastic descision about whether to follow left or right. Each probabilistic factor is modeled as a sigmoid function ($\displaystyle σ(x) = \frac{1}{1+e^{-x}}$, like in logistic regression), so the resulting composite porobability looks like: $P(z|x) = \prod P_i = \prod σ(ξ_i)$. (page 3 of Mikolov 2013). These arguments ξ_i under σ() are a bit weird (and that's where the notation fails the authors, I think): they are defined as an inner product of the context variable $x_i$ and a special "node point" $γ_i$ that defines that tree. The coordinates of these node points essentially play the role of parameters in little logistic regressions, and stand in place of parameters of one giant softmax.

Notations for this same operation of going down the tree in hierarchical softmax:
$P_i=...$
* (Morin 2005):                         $σ\big(α_{node} + β'.\text{tanh}(c+Wx+UN_{node})\big)$
* (Mikolov 2013):                      $σ\big([n(w,j+1)=\text{ch}(n(w,j))]\cdot v'_{n(w,j)} {}^\top v_{w_I} \big)$
* Lilian Weng blog 2017:          $σ(𝕀_\text{turn}(n(w_O,k),n(w_O,k+1))⋅v'_{n(w_O,k)} {}^⊤ v_{w_I})$

$P_i(\text{right})=...$
* Benjamin Wilsonj blog 2017: $σ(γ_n ^\top α_C)$
* Sebastian Ruder:                    $σ(h ^⊤ v'_n)$

@lilianweng, @benwilson_ml

 In the original notation, this $[...]$ bracket is impenetrable, in my opinion. n is supposedly a node, and ch is a child, but beyound that I'm lost. Lilian Weng explains it better: 𝕀 is a special indicator function that returns 1 if $n(w,k+1)$ is a left child of $n(w,k)$, and -1 otherwise. All this horrible $[...]$ does reall is to encode the fact that $P(\text{left}) = σ(v_n^⊤v_w)$, while $P(\text{right}) = 1-σ(v,w) = σ(-v_n^⊤v_w)$. That's one of the most unfortunate notations known to human.
 
 The coordinates $v_{n(w,k)}$ of node-vectors $n(w,k)$ for $k=1..L(w)$ are learned by the model (they characterize words, and make up this learned embedding). The tree is partialy manually tuned, to make it as balanced as possible.
 
 (Mikolov 2013) shows that negative sampling is actually better than hierarchical softmax :) See [[word2vec]]

# Refs

Hierarchical softmax:
Morin, F., & Bengio, Y. (2005, January). Hierarchical probabilistic neural network language model. In_Aistats_(Vol. 5, pp. 246-252).
https://www.iro.umontreal.ca/~lisa/pointeurs/hierarchical-nnlm-aistats05.pdf
The paper is not particularly transparent, and honestly, the notation system they use is quite horrible.

Mikolov, T., Sutskever, I., Chen, K., Corrado, G. S., & Dean, J. (2013). Distributed representations of words and phrases and their compositionality. In Advances in neural information processing systems (pp. 3111-3119). - 22k citations https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf

Sebastian Ruder, "Approximating the softmax", a great introduction:
https://ruder.io/word-embeddings-softmax/index.html#hierarchicalsoftmax

A very decent explanation (and not just a rehash, which definitely helps!) by Benjamin Wilson, 2017
http://building-babylon.net/2017/08/01/hierarchical-softmax/

An even better (and also somewhat creative) explanation by Lilian Weng, 2017:
https://lilianweng.github.io/lil-log/2017/10/15/learning-word-embedding.html

An attempt to explain it in simple terms, by Halyna Oliiny; mostly a rehash of the original paper:
https://towardsdatascience.com/hierarchical-softmax-and-negative-sampling-short-notes-worth-telling-2672010dbe08