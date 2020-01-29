# Transformers
#attention #text #dl

# Background and history

An alternative to RNNs (summary ref: [1](https://towardsdatascience.com/transformers-141e32e69591)). Text is inherently a stream of signals (words), organized in time, so it makes sense that it should be analyzed by **RNNs**. The problem is that text has long-ish connections (say, objects are elliptically passed from one sentence to another), and RNNs have problems with that (forget, or rather, don't even learn). Then **LSTM** was invented, which improves things, but not enough, as the context still tends to decay exponentially. Also, hard to parallelize, as inputs need to be processed word-by-word. Then they came up with **attention**, by assigning every word a certain "hidden state", and passing it to the entire line (_??? I really not sure how it works_), but still it didn't help enough. The lack of parallelization was the main bottleneck.

But convolutional networks that process info in chunks are highly parallelizable! And the information depth is lower (instead of going through all N elements, activation from the furthest element climbs up the hirarchy through just log N layers). Transformers (developed by Google Research and Google Brain) combine the idea CNNs with attention.

# Structure

Transformers consist of several (original paper has N = 6) **encoders** and the same number of **decoders**. Each encoder has the same architecture, but different parameters, and the same is true for d coders. 

## Encoders

Each **encoder** consists of **two layers**: **Self-Attention** block, consisting of several self-attention operations performed in parallel (aka **Multi-Headed attention**), followed by a shallow **FF network**. 

### Self-Attention

First encoder receives words, and embeds each word in an **embedding** vector (say, length 512). Self-attention transforms this vector with 3 trainable matrices into 3 smaller vectors (length 64) **Query, Key, and Value**. 

* For each pair of words ij, we calculate a **score**, which is a **Query-Key interaction**: s_ij = ⟨q_i , k_j⟩, 
* scale it down roughly (s_ij/8), and 
* softmax (now ∑s_ij = 1). These **final scores** S quantify interactions between the words. 
* Then for each target word i we calculate the weighted sum of its scores s_ij with other words (j), using a set of **Values** v_j (each one – itself a vector) as weights: z_i = ∑ s_ij ∙ v_j. This set {z_i} becomes an output of the self-attention layer.

In practice of course, vector multiplications for each word happen not in a loop, but in matrices Q, K, and V, with columns corresponding to words. A full formula for attention, parallelized, for all words : $Z = Ⰿ(Q^T K / \sqrt{d}) V$, where Ⰿ stands for columnwise softmax operation. Remember that $Q = XW^Q$, $K = XW^K$, and $V = XW^V$, where X is the input to this layer.

Several (h=8) attentions are calculated in parallel, each with a different set of {W} for QKV. The outputs of different attention heads are then concatecated together, to form a longer vector of length 512, which is passed further. This is called **Multihead Attention**.

> So, this 512-long output for each word contains 8 attention outputs  concatenated, each representing a diffeerent "idea" (QKV values) about this word. And we have a matrix to represent all words in a bag.

### FF network

Each output Z from the attention layher is passed to a FF network. The **FF network** consists of a dense ReLu layer, followed by a dense linear layer, with coefficients W_1 and W_2.  that performs A (dim=512) → transform with W_1 (to dim=2048) → ReLu → transform with W_2 ( to dim=512 again).

### Residuals

Endoders were not just stacked on top of each other in a simple FF fashion, but used **residual connections**, which means that out = in + f(in). So for each encoder, they added its input to its output; then **normalized** the results (subtract mean, divide by sd).

## Position encoding

At the embedding stage for the original text input, they also **encode word positions**, using some sort of a concatenation of two chirp-like signals, one sin-, another cos-based, with exponentially decreasing frequency (see the formula below). This appears to be something like a kernel trick (?). To make a DL ANN relate to position, it's better not to encode it as a one-hot variable (as it would waste 2 layers just to quantify it), but instead encode different positions as different combinations of nicely distributed basis functions. (A visualization of these functions is shown [here](http://jalammar.github.io/illustrated-transformer/))

$f(k,p)=sin(p/τ^{(k/d)})$, where k is depth (from 0 to d) running down the embedding vector; p is the position of the word within a sentence, and τ is some arbitrary period (they used d=256, and τ=10000). And then they do the same with cos, and *interleave* these two embeddings elementwise, so that they go 1a 1b 2a 2b etc. 

Claim that this particular embedding is good, as "f(p+offset) is a linear function of f(p)". What they seem to mean here is that f(p+offset) = Af(p), where A is some matrix.

> Is it true? I'm still confused about why these particular functions were chosen. Why chirps? And where does this claim about linear combinations come from? Perhaps it's simple and obvious, but I'm somehow missing it.

And then it looks like they literally **added**, not in terms of concatenation, but **elementwise**, one 512-long vector to another, semantic embedding to position embedding. 

> How on Earth this is possible, and why it is a good choice, I cannot understand. Maybe that's why they used this exponential chirp, as it has a very slow, long, flat tail? So essentially, once they train the semantic embedding part, it mostly utilizes the tail part, and leaves the front intact? Still I don't understand why they wouldn't encode the position into few first bits, and then just concatenate it with semantic embedding.

> Also, wouldn't it mean that semantic embedding cannot be trained separately from the position embedding? Isn't it a debt liability, as it means that they cannot use pre-trained semantic embeddings, when, say, moving to another language?

## Decoders

Each **decoder** had **3 layers**: a similar **Self-Attention** (analyzing inputs from the previous decoder), followed by an **Encoder-Decoder Attention** layer, in which Q comes from the previous layer, whille K and V  come from an output of a matching encoder, followed by a **FF network**. Apparently, it mimics what RNNs were known to do well before transformers (aka "RNN encoder-decoder mechanism").

The **first decoder** receives **output at the previous time step** as the input to its Self-Attention layer. All other decoders receive inputs from previous encoders. 

**Self-Attention layers** in decoders are similar to that in encoders, with one difference: all transformations for a given word don't get access to word after it (they are **masked** with -inf).

>Why on Earth is a good idea?? Don't we prepare grammatically for words that are about to come? And the final output of the model is one word anyways, isn't it? I'm really confused.

**ED Attention** layers in all decoders always get inputs from the top (final) output of encoders. But not the final final output (not the Z vector), but rather Keys and Values inputs form the last encoder, while Quries are internally generated from the input to this layer (output of the self-attention layer).

At the very end of all decoders, a linear layer with ~10000 outputs, and a softmax to find the best word.

# Training

In the original paper, for the final model, they averaged last 20 checkpoints. _Why??_

# Application

For machine translation, they actually don't just apply the model, but use a thing called **beam search**: at each point they don't just take the max probability word for each word, but select first 4 options, and for each assume that it was taken, and re-run the model, generating a tree of solutions (depth 50, I think?). Finally, pick the most probable total solution out of all these solutions. And there's a punishment on length. 

>  So I don't think they explore the whole tree to full depth; I think they truncate branches if they exceed combined level of total inprobability. But not sure how it was done in practice. There's a link ("ref 31") below that explains beam search, apparently.

# Open questions I still have
* How many words do they process at once? What's the size of the bag? And why cannot I find this information in the original paper?
* Why position encoding is so weird
* Why residual connections are a good idea
* Why decoders are so weird, with this KV from one sourse, and Q from another?
* Why this masking of self-attention layers in decoders is ever a good idea?
* How come we ended up with this really weird architecture, and how have we convinced ourselves that it is beautiful and optimal in any sense? Is it really optimal in some way, as our concepts of simplicity and symmetry are wrong, or is it an example of ad-hoc "evolution of human thought", as wild and random as Darwinian evolution?

# Criticism
There are claims that transformers are overrated, and same results may be achieved with simple architectures, and much lower computer:
* Merity, S. (2019). Single Headed Attention RNN: Stop Thinking With Your Head. arXiv preprint arXiv:1911.11423. [link](https://arxiv.org/abs/1911.11423)

# References
* [[Vaswani2017attention]]  - the original Transformers paper, titled "Attention is all you need". Doesn't actually describe the motivation, nor how they came up with this, nor how it works, nor the philosophy of it. ([Direct pdf](https://papers.nips.cc/paper/7181-attention-is-all-you-need.pdf))
* For **word tokens → 512-vector embeddings**, "ref 24": Using the Output Embedding to Improve Language Models, 2017, Ofir Press, Lior Wolf. ([direct pdf](https://arxiv.org/abs/1608.05859))
* For how their **positions encoding** iks as good as learned position encoding, "ref 8":  Jonas Gehring, Michael Auli, David Grangier, Denis Yarats, and Yann N. Dauphin. Convolutional sequence to sequence learning. arXiv preprint arXiv:1705.03122v2, 2017.
* For **beam search**, "ref 31": Yonghui Wu, Mike Schuster, Zhifeng Chen, Quoc V Le, Mohammad Norouzi, Wolfgang Macherey, Maxim Krikun, Yuan Cao, Qin Gao, Klaus Macherey, et al. Google’s neural machine
translation system: Bridging the gap between human and machine translation. arXiv preprint
arXiv:1609.08144, 2016. 

**Comments, reviews, explanations:**
* Re-implementation of the original paper, in Python: http://nlp.seas.harvard.edu/2018/04/03/attention.html
* http://jalammar.github.io/illustrated-transformer/ - a very nice diagrammatic explanation from Jay Alammar
* https://towardsdatascience.com/openai-gpt-2-understanding-language-generation-through-visualization-8252f683b2f8
* https://towardsdatascience.com/deconstructing-bert-part-2-visualizing-the-inner-workings-of-attention-60a16d86b5c1
* [http://jalammar.github.io/illustrated-gpt2/](<http://jalammar.github.io/illustrated-gpt2/>)



