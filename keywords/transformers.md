# Transformers

Parents: [[10_Text]], attention
Related: [[convnet]], [[residual]]

#attention #text #dl


Papers:
* [[Vaswani2017attention]] - the original Transformers paper, titled "Attention is all you need". Doesn't actually describe the motivation, nor how they came up with this, nor how it works, nor the philosophy of it.
* [[Marcus2019gpt2]] - on GPT2 and limitations of (its) intelligence


Note: right now the technical side of this paper mostly describes [[Vaswani2017attention]]. Maybe in the future it would make sense to move technical details inside the paper article, and keep a more high-level description here.

# Unprocessed bibliography

http://jalammar.github.io/illustrated-transformer/ - a very nice diagrammatic explanation from Jay Alammar

[http://jalammar.github.io/illustrated-gpt2/](<http://jalammar.github.io/illustrated-gpt2/>)

Brown, T. B., Mann, B., Ryder, N., Subbiah, M., Kaplan, J., Dhariwal, P., ... & Agarwal, S. (2020). Language models are few-shot learners. arXiv preprint arXiv:2005.14165. https://arxiv.org/abs/2005.14165 - gpt3 paper

Interfaces for Explaining Transformer Language Models
https://jalammar.github.io/explaining-transformers/
(explainability, neuron activation)

Peter Clark, Oyvind Tafjord, Kyle Richardson. (2020) Transformers as Soft Reasoners over Language https://arxiv.org/abs/2002.05867
Show how, once learned on texts, transformers can solve syllogisms (infer T/F of a statement, given a set of statements.)

Merity, S. (2019). Single Headed Attention RNN: Stop Thinking With Your Head. arXiv preprint arXiv:1911.11423. https://arxiv.org/abs/1911.11423 - the main idea is that transformers are overrated

Some super-hot recent transformer (for text) representation visualization (interpretability!)
Summary: https://syncedreview.com/2021/04/05/yann-lecun-team-uses-dictionary-learning-to-peek-into-transformers-black-boxes/
Paper itself: https://arxiv.org/pdf/2103.15949.pdf
Yun, Z., Chen, Y., Olshausen, B. A., & LeCun, Y. (2021). Transformer visualization via dictionary learning: contextualized embedding as a linear superposition of transformer factors. arXiv preprint arXiv:2103.15949.

Review of transformers in vision (how they surpassed convnets?)
Khan, S., Naseer, M., Hayat, M., Zamir, S. W., Khan, F. S., & Shah, M. (2021). Transformers in Vision: A Survey. arXiv preprint arXiv:2101.01169. https://arxiv.org/abs/2101.01169

# Open questions I still have

* Why residual connections are a good idea ([[residual]])
* Why decoders are so weird, with this KV from one source, and Q from another?
* How come we ended up with this really weird architecture, and how have we convinced ourselves that it is beautiful and optimal in any sense? Is it really optimal in some way, as our concepts of simplicity and symmetry are wrong, or is it an example of ad-hoc "evolution of human thought", as wild and random as Darwinian evolution?

# Background and history

An alternative to RNNs ([[07_RNNs]]). Text is inherently a stream of signals (words), organized in time, so it makes sense that it should be analyzed by **RNNs**. The problem is that text has long-ranged connections (say, objects are elliptically passed from one sentence to another), and RNNs had problems with that (they would "forget" what's going on during learning, so they would never learn longer connections - gradients would vanish). Then **LSTM** was invented, which improves things, but not enough, as the context awareness with LSTMs still tends to decay exponentially, while naturalistic texts often have very long arcs. Also, RNNs are hard to parallelize, as inputs need to be processed word-by-word. 

To combat short "attention span", of RNNs, people came up with **attention**, where every word was assigned a certain "hidden state", and this hidden state served as a semi-permanent input to a RNN, as it kept crunching data. With attention the network can generate a **query** (a different kind of output), and use this query to "look up" among the accumulated **keys** by calculating columns-wise scalar products of query with these keys. The higher the value, the more similar this key is to the query (the better the match). Which means that ⟨q, k⟩ can be used as a mask, to weigh some **values** that these keys accompanied. (_Not sure how all of it worked exactly in the context of RNNs - would be nice to check at some point..._).

Still, while attention was applied to RNN, it didn't help with paralleization. You know what is really nicely parallelizable? Cconvolutional networks ([[convnet]])! So people tried using them them [[wavenet]]-style, as a sliding window, and as they are hierarchical, we can use longer sequences (log N layers elements to cover N positions, as opposed to N layers for RNNs). The problem though is that these networks are too positional (too dependent on where exactly each word is), while IRL in many cases position within a sentence feels like an ordinal value, rather than categorical. 

Enter **Transformers** (developed by Google Research and Google Brain), as they combine the idea of convnets with attention. (That's why the paper was called "Attention is all you need" - they mean that RNNs may be left out, as it's not the RNNnness, but attention, that made previous models good). Attention, when applied to a sequence of words, works better than convolution, as the scalar product of query and keys creates a mask that is content-dependent: what each word is (keys), and what you are looking for (query), and not strictly position-dependent (all that a convolution can do). So instead of learning a coefficient for every position (a single convolution filter) we can now dynamically sample words from a bag (using key-query scalar products, that can be interpreted as "matches"), and then apply a transformation to these "dynamically sampled" vectors. Which allows you to look at much longer contexts (hundreds of words).

# Structure

Transformers consist of several (original paper has N = 6) **encoders** and the same number of **decoders**. Each encoder has the same architecture, but different parameters, and the same is true for decoders. 

Each **encoder** consists of **two layers**: **Self-Attention** block, consisting of several self-attention operations performed in parallel (aka **Multi-Headed attention**), followed by a shallow **FF network**. 

##### Self-Attention
First, encoder receives words, and embeds each word in an **embedding** vector (say, length 512). Self-attention uses 3 trainable matrices to transform this vector into 3 smaller vectors (each length 64) **Query**, **Key**, and **Value**. 
* For each pair of words ij, we then calculate an attention **score**, or a **Query-Key interaction**: s_ij = ⟨q_i , k_j⟩, scale it down roughly (s_ij/8), and [[softmax]] (expontiate and normalize, so now ∑s_ij = 1). These **final scores** S quantify interactions between the words. 
* Then for each target word i we use these scores s_ij with other words (j) as weights, to calculate a weighted sum of **Values** v_j for other words (each one – itself a vector): z_i = ∑ s_ij ∙ v_j. This set of values {z_i} for each word becomes an output of the self-attention layer.

In practice of course, vector multiplications for each word happen not in a loop, but in matrices Q, K, and V, with columns corresponding to words. A full formula for attention, parallelized, for all words : $Z = Ⰿ(Q^T K / \sqrt{d}) V$, where Ⰿ stands for columnwise softmax operation, and further $Q = XW^Q$, $K = XW^K$, and $V = XW^V$, where X is the input.

Several (h=8) attentions are calculated in parallel, each with a different set of {W} for QKV. The outputs of different attention heads are then concatenated together, to form a longer vector of length 512, which is passed further. This is called **Multihead Attention** (the metaphor here is that each head "attends to a different thing").

##### FF network
Each output Z from the attention layer is passed to a **FF network**. This network consists of a dense ReLu layer, followed by a dense linear layer, with coefficients W_1 and W_2 that perform: Z (dim=512) → transform with W_1 (to dim=2048) → ReLu → transform with W_2 ( to dim=512 again).

##### Residuals
Encoders are not just stacked on top of each other in a simple FF fashion, but used **residual connections**, which means that out = in + f(in). So for each encoder, they added its input to its output; then **normalized** the results (subtract the mean and divide by sd).

##### Position encoding
At the embedding stage for the original text input, they also **encode word positions**, using some sort of a concatenation of two chirp-like signals, one sin-, another cos-based, with exponentially decreasing frequency (see the formula below). This appears to be something like a kernel trick (?). To make a DL ANN relate to position, it's better not to encode it as a one-hot variable (as it would waste 2 layers just to quantify it), but instead encode different positions as different combinations of nicely distributed basis functions. (A visualization of these functions is shown [here](http://jalammar.github.io/illustrated-transformer/))

$f(k,p)=\sin(p/τ^{(k/d)})$, where k is depth (from 0 to d) running down the embedding vector; p is the position of the word within a sentence, and τ is some arbitrary period (they used d=256, and τ=10000). And then they do the same with cos, and *interleave* these two embeddings elementwise, so that they go 1a 1b 2a 2b etc. 

Claim that this particular embedding is good, as "f(p+offset) is a linear function of f(p)". What they seem to mean here is that f(p+offset) = Af(p), where A is some matrix.

> Is it true? I'm still confused about why these particular functions were chosen. Why chirps? And where does this claim about linear combinations come from? Perhaps it's simple and obvious, but I'm somehow missing it.

And then it looks like they literally **added**, not in terms of concatenation, but **elementwise**, one 512-long vector to another, semantic embedding to position embedding. 

> How on Earth this is possible, and why it is a good choice, I cannot understand. Maybe that's why they used this exponential chirp, as it has a very slow, long, flat tail? So essentially, once they train the semantic embedding part, it mostly utilizes the tail part, and leaves the front intact? Still I don't understand why they wouldn't encode the position into few first bits, and then just concatenate it with semantic embedding.

> Also, wouldn't it mean that semantic embedding cannot be trained separately from the position embedding? Isn't it a debt liability, as it means that they cannot use pre-trained semantic embeddings, when, say, moving to another language?

##### Decoders
Each **decoder** had **3 layers**: a similar **Self-Attention** (analyzing inputs from the previous decoder), followed by an **Encoder-Decoder Attention** layer, in which Q comes from the previous layer, whille K and V  come from an output of a matching encoder, followed by a **FF network**. This architecture mimics what RNNs were known to do well before transformers (aka "RNN encoder-decoder mechanism").

The **first decoder** receives **output at the previous time step** as the input to its Self-Attention layer. All other decoders receive inputs from previous encoders. 

**Self-Attention layers** in decoders are similar to that in encoders, with one difference: all transformations for a given word don't get access to word after it (they are **masked** with -inf). This models the logic of RNN during training: to translate (or in any other way transform, or generate) text you need to learn on something. And while learning, you should not "cheat"; you should not be able to look ahead into the training data; you should not see the next word, you need to guess it. That's what masking does. For the source text (in case of translation) you see the entire text, but for the output text each parallelized process sees only the text before the current word, and tries to predict the next word. That's what the mask does. It allows to parallelize RNN logic.

**ED Attention** layers in all decoders always get inputs from the top (final) output of encoders. But not the final final output (not the Z vector), but rather Keys and Values inputs form the last encoder, while Quries are internally generated from the input to this layer (output of the self-attention layer).

At the very end of all decoders, a linear layer with ~10000 outputs, and a softmax to find the best word.

##### Training
In the original paper, for the final model, they averaged last 20 checkpoints. _Why??_

# Application

For machine translation, they actually don't just apply the model, but use a thing called **beam search**: at each point they don't just take the max probability word for each word, but select first 4 options, and for each assume that it was taken, and re-run the model, generating a tree of solutions (depth 50, I think?). Finally, pick the most probable total solution out of all these solutions. And there's a punishment on length. 

>  So I don't think they explore the whole tree to full depth; I think they truncate branches if they exceed combined level of total inprobability. But not sure how it was done in practice. There's a link ("ref 31") below that explains beam search, apparently.

# Practical examples

* GPT (Generative Pretrained Transformer) - first version by OpenAI
* Bert (2018, Google)
* GPT-2 (2019, OpenAI)
* GROVER (2019, U Washington)
* Distillbert (2019, HuggingFace)
* GPT-3 (2020, OpenAI)

# Criticism

There are claims that transformers are overrated, and same results may be achieved with simple architectures, and much lower computer (See S. Merity 2019). In terms of end-results and use cases, see also everyting my Marcus (like [[Marcus2019gpt2]]).

# References

A really nice, clear lecture about the [[Vaswani2017attention]] paper: https://www.youtube.com/watch?v=rBCqOTEfxvg

https://towardsdatascience.com/transformers-141e32e69591

For **word tokens → 512-vector embeddings**, "ref 24": Using the Output Embedding to Improve Language Models, 2017, Ofir Press, Lior Wolf. ([direct pdf](https://arxiv.org/abs/1608.05859))

For how their **positions encoding** iks as good as learned position encoding, "ref 8":  Jonas Gehring, Michael Auli, David Grangier, Denis Yarats, and Yann N. Dauphin. Convolutional sequence to sequence learning. arXiv preprint arXiv:1705.03122v2, 2017.

For **beam search**, "ref 31": Yonghui Wu, Mike Schuster, Zhifeng Chen, Quoc V Le, Mohammad Norouzi, Wolfgang Macherey, Maxim Krikun, Yuan Cao, Qin Gao, Klaus Macherey, et al. Google’s neural machine
translation system: Bridging the gap between human and machine translation. arXiv preprint
arXiv:1609.08144, 2016. 

Re-implementation of the original paper, in Python: http://nlp.seas.harvard.edu/2018/04/03/attention.html