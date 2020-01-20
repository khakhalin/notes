# Transformers
#attention #text #dl

# Background and history

An alternative to RNNs (summary ref: [1](https://towardsdatascience.com/transformers-141e32e69591)). Text is inherently a stream of signals (words), organized in time, so it makes sense that it should be analyzed by **RNNs**. The problem is that text has long-ish connections (say, objects are elliptically passed from one sentence to another), and RNNs have problems with that (forget, or rather, don't even learn). Then **LSTM** was invented, which improves things, but not enough, as the context still tends to decay exponentially. Also, hard to parallelize, as inputs need to be processed word-by-word. Then they came up with **attention**, by assigning every word a certain "hidden state", and passing it to the entire line (_??? I really not sure how it works_), but still it didn't help enough. The lack of parallelization was the main bottleneck.

But convolutional networks that process info in chunks are highly parallelizable! And the information depth is lower (instead of going through all N elements, activation from the furthest element climbs up the hirarchy through just log N layers). Transformers combine the idea CNNs with attention.

# Structure

Transformers consist of several (original paper has N = 6) **encoders** and the same number of **decoders**. Each encoder has the same architecture, but different parameters, and the same is true for d coders. 

Each **encoder** consists of two layers: **Self-Attention** block, consisting of several self-attention operations performed in parallel (aka **Multi-Headed attention**), followed by a shallow **FF network**. First encoder receives words, and embeds each word in an **embedding** vector (say, length 512). Self-attention transforms this vector with 3 trainable matrices into 3 smaller vectors (length 64) **Query, Key, and Value**. Then, for each pair of words ij, we calculate s_ij = ⟨q_i , k_j⟩, scale it down roughly (s_ij/8), and softmax (now ∑s_ij = 1). These numbers S now quantify interactions between the words. Then we calculate the weighted sum of S for each word j related to the target word i, using vector v for this word as weights: z_i = ∑ s_ij ∙ v_j. This vector {z_i} becomes an output of the self-attention layer. 

In practice of course, vector multiplications for each word happen not in a loop, but in matrices Q, K, and V, with columns corresponding to words. A full formula for attention, parallelized, for all words : $A = Ⰿ(Q^T K / \sqrt{d})\cdot V$, where Ⰿ stands for columnwise softmax operation. 

Several (h=8) attentions are calculated in parallel, each with a different set of QKVW. The outputs are concatecated together, to form a longer vector of length 512, which is passed to the next encoder. This is called **Multihead Attention**.

Each A is then passed to a FF network. The **FF network** consists of a dense ReLu layer, followed by a dense linear layer, with coefficients W_1 and W_2.  that performs A (dim=512) → trasnform with W_1 (to dim=2048) → ReLu → trasnform with W_2 ( to dim=512 again).

> So, this 512 contains 8 attention calculations concatenated, each representing a diffeerent "idea" (QKV values), but these are all "ideas" about the same one word. And we process several words at the same time, right?

At the embedding stage, they also encode word positions, using some sort of overtone-like series of sin / cos functions of exponentially increasing frequencies. This appears to be something like a kernel trick: for a DL ANN relate to position, it's better not to encode it as a just an increasing variable (as it would waste 2 layers just to quantify it), but instead code different areas with different combinations of inputs. A bunch of increasingly fast sine waves does exactly that.

> What I still don't understand, is how this position is later injected into the data pipeline. Is it concatenated or something? Also I'm not yet sure how embeddings work.

Each **decoder** had 3 layers: a similar Self-Attention, followed by an **Encoder-Decoder Attention** layer, in which Q comes from the previous layer, whille K and V  come from an output of a matching encoder, followed by a dense layer W (FF network). Apparently, it mimics what RNNs were known to do well before (RNN encoder-decoder mechanism).

# Criticism
There are claims that transformers are overrated, and same results may be achieved with simple architectures, and much lower computer:
* Merity, S. (2019). Single Headed Attention RNN: Stop Thinking With Your Head. arXiv preprint arXiv:1911.11423. [link](https://arxiv.org/abs/1911.11423)

# References
* Re-implementation of the original paper, in Python: http://nlp.seas.harvard.edu/2018/04/03/attention.html
* https://towardsdatascience.com/openai-gpt-2-understanding-language-generation-through-visualization-8252f683b2f8
* https://towardsdatascience.com/deconstructing-bert-part-2-visualizing-the-inner-workings-of-attention-60a16d86b5c1
* [http://jalammar.github.io/illustrated-gpt2/](<http://jalammar.github.io/illustrated-gpt2/>)
* [[Vaswani2017attention]]  - the original Transformers paper, titled "Attention is all you need". Doesn't actually describe the motivation, nor how they came up with this, nor how it works, nor the philosophy of it. [Direct pdf link](https://papers.nips.cc/paper/7181-attention-is-all-you-need.pdf)
* For word tokens → 512-vector embeddings, the original paper cites [this preprint](https://arxiv.org/abs/1608.05859): Using the Output Embedding to Improve Language Models, 2017, Ofir Press, Lior Wolf.

