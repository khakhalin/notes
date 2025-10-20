# Transformers

Parents: [[text]], [[dl]], attention (in a neuro-sense)
Related: [[convnet]], [[residual]], [[llm]]

#attention #text #dl


Papers:
* [[Vaswani2017attention]] - the original Transformers paper, titled "Attention is all you need". Doesn't actually describe the motivation, nor how they came up with this, nor how it works, nor the philosophy of it.
* [[Marcus2019gpt2]] - on GPT2 and limitations of (its) intelligence


Note: right now the technical side of this paper mostly describes [[Vaswani2017attention]]. Maybe in the future it would make sense to move technical details inside the paper article, and keep a more high-level description here.

# Unprocessed bibliography

Transformers as Graph Neural Networks (a short Gradient article):
https://thegradient.pub/transformers-are-graph-neural-networks/

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

https://arxiv.org/pdf/2302.01107.pdf - A Survey on Efficient Training of Transformers

# Open questions I still have

* Why residual connections are a good idea in this case ([[residual]])
* Why decoders are so weird, with this KV from one source, and Q from another?
* How come we ended up with this really weird architecture, and how have we convinced ourselves that it is beautiful and optimal in any sense? Is it really optimal in some way, as our concepts of simplicity and symmetry are wrong, or is it an example of ad-hoc "evolution of human thought", as wild and random as Darwinian evolution?

# Background and history

An alternative to RNNs ([[rnn]]). Text is inherently a stream of signals (words), organized in time, so it makes sense that it should be analyzed by **RNNs**. The problem is that text has long-ranged connections (say, objects are elliptically passed from one sentence to another), and RNNs had problems with that (they would "forget" what's going on during learning, so they would never learn longer connections - gradients would vanish). Then **LSTM** was invented, which improves things, but not enough, as the context awareness with LSTMs still tends to decay exponentially, while naturalistic texts often have very long arcs. Also, RNNs are hard to parallelize, as inputs need to be processed word-by-word. 

To combat short "attention span", of RNNs, people came up with **attention**, where every word was assigned a certain "hidden state", and this hidden state served as a semi-permanent input to a RNN, as it kept crunching data. With attention the network can generate a **query** (a different kind of output), and use this query to "look up" among the accumulated **keys** by calculating columns-wise scalar products of query with these keys. The higher the value, the more similar this key is to the query (the better the match). Which means that ⟨q, k⟩ can be used as a mask, to weigh some **values** that these keys accompany. (_Not sure how all of it worked exactly in the context of RNNs - would be nice to check at some point..._).

Still, while attention was applied to RNN, it didn't help with paralleization. You know what is really nicely parallelizable? Cconvolutional networks ([[convnet]])! So people tried using them them [[wavenet]]-style, as a sliding window, and as they are hierarchical, we can use longer sequences (log N layers elements to cover N positions, as opposed to N layers for RNNs). The problem though is that these networks are too positional (too dependent on where exactly each word is), while IRL in many cases position within a sentence feels like an ordinal value, rather than categorical. 

Enter **Transformers** (developed by Google Research and Google Brain), as they combine the idea of convnets with attention. (That's why the paper was called "Attention is all you need" - they mean that RNNs may be left out, as it's not the RNNnness, but attention, that made previous models good). Attention, when applied to a sequence of words, works better than convolution, as the scalar product of query and keys creates a mask that is content-dependent: what each word is (keys), and what you are looking for (query), and not strictly position-dependent (all that a convolution can do). So instead of learning a coefficient for every position (a single convolution filter) we can now dynamically sample words from a bag (using key-query scalar products, that can be interpreted as "matches"), and then apply a transformation to these "dynamically sampled" vectors. Which allows you to look at much longer contexts (hundreds of words).

# Structure

Transformers consist of several (original paper has N = 6) **encoders** and the same number of **decoders**. Each encoder consists of 2, and each decoder - of 3 sub-blocks (see below). 1st encoder-decoder pair gets info from the embedded (see below) source text (for the encoder), and target text (for the decoder). And then there's a flow of info from encoder to decoder kinda mid-way. This pattern repeats N-1 times, except instead of referring to the original text, each next pair gets inputs from the previous i-1 set. So the overall architecture is highly hierarchical:
* Full network =  embedding→transformation→output, 
* transformation = ED (encoder-decoder) block × 6,
* ED block = E + D, where E and D have separate inputs and separate outputs, but also one of the outputs of E is fed into D
* E = SA + FF, while D = SA + EDA + FF (see below)
* Finally SA and EDA are built of 3 transformations (QKV, see below)
Five different layers of organization!

### Word representation
Words are ultimately represented as vectors of reasonable length (512 in GPT2), and is done in two steps. First we embed words simply: for conceptual purposes we can think of it as one-hot encoding, although for practical reasons a more efficient procedure is used, called a **Byte Pair Encoding** (BPE) tokenization. In it, not whole words, but tokens = common parts of words (prefixes, roots, suffixes etc.) are one-hot encoded. It makes the vector shorter, but still it's 50257 positions. We multiply it by a pre-learned $W^E$ matrix, and call it an **embedding**.

### Position encoding
At the embedding stage for the original text input, we also **encode word positions**, using concatenation of several chirp-like signals; some sin-, some cos-based, and with exponentially decreasing frequencies (see the formula below). This appears to be something like a kernel trick ([[kernels]]). This approach is better than a one-hot encoding for positions, as it makes similar positions be encoded by somewat co-aligned vectors, and so ANN can relate to word position in a fuzzy way. A visualization of these functions is shown here: http://jalammar.github.io/illustrated-transformer/

The formula: $f(k,p)=\sin(p/τ^{(k/d)})$, where k is depth (from 0 to d) running down the embedding vector; p is the position of the word within a sentence, and τ is some arbitrary period (they used d=256, and τ=10000). And then they do the same with cos, and interleave these two embeddings elementwise, so that they go 1a 1b 2a 2b etc. 

They claim that this particular embedding is particularly good, as "f(p+offset) is a linear function of f(p)". What they seem to mean here is that f(p+offset) = Af(p), where A is some matrix.

> How to feel it intuitively? Check on a piece of paper. #todo

And then it looks like they literally **added**, not in terms of concatenation, but **elementwise**, one 512-long vector to another, semantic embedding to position embedding. 

> How on Earth this is possible, and why it is a good choice, I cannot understand. Maybe that's why they used this exponential chirp, as it has a very slow, long, flat tail? So essentially, once they train the semantic embedding part, it mostly utilizes the tail part, and leaves the front intact? Still I don't understand why they wouldn't encode the position into few first bits, and then just concatenate it with semantic embedding. 

> Or is it like "we don't know how much space to allocate for each embedding, so to make it efficient we delegate it to the model to learn the best allocation of resources here? By making position-embedding and semantic embedding secretly orthogonal?"

> Or is it actually the other way around, and they want the model to see interactions between them, because word order can change the semantics? Like "To poke with a poke" or "buffalo buffalo"?

> Also, wouldn't it mean that semantic embedding cannot be trained separately from the position embedding? Isn't it a technical debt for the interpretation stage?

### Encoders
Each encoder has the same architecture, but different parameters, and the same is true for decoders.  Each **encoder** consists of two layers: a **Self-Attention** block, consisting of several self-attention operations performed in parallel (aka **Multi-Headed attention**), followed by a shallow **FF network**.  Each decoder is a bit larger (3 layers), but is built of similar elements, and we'll talk about them later.

### Self-Attention
The self-attention module (in both encoders and decoders - they are very similar) receives word-embeddings vectors (say, length 512). Self-attention uses 3 trainable matrices to transform this vector into 3 smaller vectors (each length 64) **Query**, **Key**, and **Value**. 
* For each pair of words ij, we calculate an attention **score**, or the **Query-Key interaction**: s_ij = ⟨q_i , k_j⟩, scale it down roughly (s_ij/8), and [[softmax]] (expontiate and normalize, so now ∑s_ij = 1). These **final scores** S quantify interactions between the words. 
* Then for each target word i we use these scores s_ij with other words (j) as weights, to calculate a weighted sum of **Values** v_j for other words (each one – itself a vector): z_i = ∑ s_ij ∙ v_j. This set of values {z_i} for each word becomes an output of the self-attention layer.

The intuitions here (it's easier to explain it listing these three values backwards):
* **Value** is the encoding of the token itself, of all the possible semantics behind it. This representation changes as you go along the hierarchy of attention blocks, but Value still represents the current local meaning.
* **Key** is the reported willingness of this piece of meaning to interact with other pieces of meaning. Like, "I can be useful in this way, and also this, and this". One can expect this to encode grammatical structures, but also semantic categories, topical classes etc. A resume, a linkedin profile.
* **Query** is the active attempt of a piece of meaning to find other target pieces of meaning. For example, an adverb may be looking for a noun, an object for a subject, a diminutive suffix for the root, the word "paint" for a name of a color etc.

So at first step, once we cross every query with every possible key, we let our "source" pieces of meaning find target pieces of meaning nearby. Like a company looking for a maching resume. The intensity of this QK multiplication (s_ij) is how well the key value matches the query. As position within the text is also originally encoded in the encoding, it is easy for queries for search for values nearby if they want to, or later in the text, or earlier in the text, if they need to. The model just needs to learn to encode it in the keys, and to request that through the queries.

And then in the second step we move information around. Say, if a token (or rather position) 1 interacted with itself decently, but also with position 9, but did not interact with anything else, then the output vector z_1 will be a weighted average of v_1 and v_9, as only s_11 and s_19 will be non-zero. if the encoding of v_1 and v_9 is good and their bases are orthogonal to each other, then the resulting average vector will encode both signals.

In practice of course, vector multiplication for each token / position does not happen a loop, word-by-word, but with matrices Q, K, and V, with columns corresponding to q, k, and v vectors for different words. A full formula for attention, parallelized, for all words : $Z = Ⰿ(Q^T K / \sqrt{d}) V$, where Ⰿ stands for columnwise [[softmax]] operation, inputs are generated by $Q = \tanh(XW^Q)$, $K = \tanh(XW^K)$, and $V = \tanh(XW^V)$, where X is the input. For the first layer, each row of X is an embedding representation of a different token. Note that the QᵀK product is the only part where we have interaction between different words; everything else happens with semantics flowing in parallel (a bit reminiscent of [[convnets]] actually!).

> Jallamar actualy doesn't mention a tanh() here, but without tanh you can open q∙k and XᵀX becomes a scalar, so it cannot be just a linear transformation; it has to go through some activation further.

Several (h=8) attentions are calculated in parallel, each with a different set of {W} for QKV. The outputs of different attention heads are then concatenated together, to form a longer vector of length 512, which is passed further. This is called **Multihead Attention** (the metaphor here is that each head "attends to a different thing").

### FF network
Each output Z from the attention layer is passed to a **FF network**. This network consists of a dense ReLu layer, followed by a dense linear layer, with coefficients W_1 and W_2 that perform: Z (dim=512) → transform with W_1 (to dim=2048) → ReLu → transform with W_2 ( to dim=512 again).

### Residuals
In both encoders and decoders, each sub-block works as a **residual block**: it means that these blocks aren't just stacked on top of each other in a simple FF fashion, but have their input added to their outputs (literally, out = in + f(in) ), and then the result is **normalized** (we subtract the mean and divide by sd).

### Decoders
Decoders are built similarly to encoders, but instead of having 2 layers (self-attention, followed by a FF), each **decoder** had **3 layers**:
1. **Self-Attention** (identical to that of an encoder, but analyzing previously generated outputs)
2. It is followed by an **Encoder-Decoder Attention** layer, in which Query comes from the previous layer, but Keys and Values come from a matching encoder. So the decoder "knows" what we are looking for (Query), and then uses available info (Keys) from the original text to find good meaning there (Values).
3. Finally, this is followed by a **FF network**. This architecture mimics what RNNs were known to do well before transformers (aka "RNN encoder-decoder mechanism"). Overall, the **first decoder** receives **previously generated text output** as the input to its Self-Attention layer. All other decoders receive inputs from previous encoders. 

**Self-Attention layers** in decoders are almost identical to that in encoders, with one difference: at training time, all transformations for a given word don't get access to word after it (they are **masked** with -inf). This models the logic of RNN during training: to translate (or in any other way transform, or generate) text you need to learn on something, so you work with pre-existed texts. But you should not allow the network to "cheat" during learning; it should not be able to look ahead into the training data; you can't let it see the next word, it needs to guess it! That's what masking does. For the source text (in case of translation) you see the entire text in the vicinity of the word, but for the output text each parallelized process sees only the text before the current word, and tries to predict the next word. That's what the mask does. It allows to parallelize the RNN logic.

### Output
At the very end of all decoders, we have a linear layer with ~10000 outputs, and a softmax to find the best word.

### Training
In the original paper, for the final model, they averaged last 20 checkpoints. _Why??_

# Application

For machine translation, they actually don't just apply the model, but use a thing called **beam search**: at each point they don't just take the max probability word for each word, but select first 4 options, and for each assume that it was taken, and re-run the model, generating a tree of solutions (depth 50, I think?). Finally, pick the most probable total solution out of all these solutions. And there's a punishment on length. 

>  So I don't think they explore the whole tree to full depth; I think they truncate branches if they exceed combined level of total improbability. But not sure how it was done in practice. There's a link ("ref 31") below that explains beam search, apparently.

# Practical examples

* GPT (Generative Pretrained Transformer) - first version by OpenAI
* Bert (2018, Google)
* GPT-2 (2019, OpenAI)
* GROVER (2019, U Washington)
* Distillbert (2019, HuggingFace)
* GPT-3 (2020, OpenAI)

# Criticism

There are claims that transformers are overrated, and same results may be achieved with simple architectures, and much lower computer (See S. Merity 2019). In terms of end-results and use cases, we also have some professional skeptics who just hate transformers in principle (like [[Marcus2019gpt2]], or this: https://www.technologyreview.com/2020/08/22/1007539/gpt3-openai-language-generator-artificial-intelligence-ai-opinion/ )

# References

Key, visual, readable explanations by Jay Alammar:
* http://jalammar.github.io/illustrated-transformer/ - a very nice diagrammatic explanation from Jay Alammar
* http://jalammar.github.io/illustrated-gpt2/
* https://jalammar.github.io/a-visual-guide-to-using-bert-for-the-first-time/
* https://jalammar.github.io/illustrated-bert/

A great, clear, and a bit more in-depth explanation by Daniel Dugas: https://dugas.ch/artificial_curiosity/GPT_architecture.html

A really nice, clear lecture about the [[Vaswani2017attention]] paper: https://www.youtube.com/watch?v=rBCqOTEfxvg

"X-ray transformer" infographics: https://losslandscape.com/wp-content/uploads/2020/09/x-ray-transformer-infographic-by-javier-ideami-2700h.gif

Re-implementation of the original paper, in Python: http://nlp.seas.harvard.edu/2018/04/03/attention.html

On early stage word endoding (BPE tokenization):
* Key citation: Using the Output Embedding to Improve Language Models, 2017, Ofir Press, Lior Wolf. https://arxiv.org/abs/1608.05859
* https://huggingface.co/transformers/tokenizer_summary.html
* https://github.com/huggingface/tokenizers

Visualizing BERT word embeddings: https://home.ttic.edu/~kgimpel/viz-bert/viz-bert.html

Self-attention:
* Explaining the math behind it: https://srome.github.io/Understanding-Attention-in-Neural-Networks-Mathematically/

Position encoding: 
* Jonas Gehring, Michael Auli, David Grangier, Denis Yarats, and Yann N. Dauphin. Convolutional sequence to sequence learning. arXiv preprint arXiv:1705.03122v2, 2017.

Beam search:
* Yonghui Wu, Mike Schuster, Zhifeng Chen, Quoc V Le, Mohammad Norouzi, Wolfgang Macherey, Maxim Krikun, Yuan Cao, Qin Gao, Klaus Macherey, et al. Google’s neural machine
translation system: Bridging the gap between human and machine translation. arXiv preprint
arXiv:1609.08144, 2016. 

Differences between BERT and GPT (as of June 2020): https://www.topbots.com/leading-nlp-language-models-2020/