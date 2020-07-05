# Attention is all you need

Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., ... & Polosukhin, I. (2017). Attention is all you need. In Advances in neural information processing systems (pp. 5998-6008).
(Google Research and Google Brain)
https://papers.nips.cc/paper/7181-attention-is-all-you-need.pdf

#attention

Parent: [[transformers]]

The paper introducing **Transformers**

> **Note:** this summary below is realy rough. For a better explanation of transformers, see the main [[transformers]] entry. More info, asides, comments, detective work, etc.

5k citations as of 2020, so seminal. I find it quite dry: it doesn't really try to explain the philosophy behind the model (what makes it work), neither does it build the rationale for the model. It just describes the architecture rather systematically, but leaves all conceptual work to the reader.

Interestingly, authors claim "equal contribution with random listing order". I kinda like it.

The paper that took RNNs + attention, and got rid of the RNN part (thus the name). On a language task (en→de, en→fr), show improvement + easier parallelization. Training took 4 days on 8 GPUs, which was apparently a ridiculous improvement.

They didn't "invent" self-attention, and give 4 references for what it is.

What they claim they were the first to do, is to use attention only for sequential tasks, without RNNs or sequence-aligned convolution. It seems that they mean "convolution that is like unrolled RNN, that runs _within_ each bag", as opposed to bag-like approach that handles each bags in the same way (like convolution), and somehow encodes word position (like convolution), but has to encode word position explicitly.

**Architecture:** Auto-regressive model: input + existing output → new output. Inside, input: symbols → continuous representation {z} → encoder → decoder → output. 

**Encoder** has 6 identical layers, each made of 2 sublayers: self-attention, followed by dense. Output_of_a_layer(x) = Normalize(x + sublayer(x)), aka "residual connections". All layers and embeddings match in dimension (512).

**Decoder** also has 6 layers, each of 3 sublayers: 2 same as in the encoder, the third one with "multi-head". Also residual connections.

**Attention**:  to a vector of an output, from these 3 vectors (in practice, packed into matrices, for parallel processing):
* Query
* Key
* Value

Attention = softmax(QKᵀ / √dim)∙V . Then multihead attention just concats h different parallel alternative "attentions" together (they used h=8), and each attention had lower dimensionality (512/8=64).

For decoders, used "masked attention", to manually make sure future doesn't affect the past.

For positions, used a weird overtone series (they don't call it that way): sin and cos at geometrically increasing frequencies!

> The original Transformers paper says: "The positional encodings have the same dimension d_model as the embeddings, so that the two can be summed." I really don't know what it means. And for sin / cos, it says "We chose this function because we hypothesized it would allow the model to easily learn to attend by relative positions, since for any fixed offset k, PEpos+k can be represented as a linear function of PEpos." Also no idea.