# Gated Recurrent Unit (GRU)

#rnn

Parents: [[07_RNNs]]
See also: [[LSTM]], [[09_Graphs]]

Introduced in 2014; similar to LSTM, but fewer parameters (3 layers in GRU, compared to 4 in LSTM, as GRUs ahve no output gate). Wiki claims that LSTMs are  better and outperform GRUs (e.g. LSTM can count, while GRUs can't). _But there seem to be no clear trend on google trends, so it's not obvious if they are really outdated, or whether wiki article is just biased._

**The logic:**
1. Take input vector (x), multiply by weights matrix
2. Add previous otput (also a vector, $h_{t-1}$), multiplied by another weight matrix
3. Add bias, do sigmoid of it all, get **update gate vector** (z)
4. Repeat the same exact thing once again with different weights and bias, call it **reset gate vector** (r)
5. Calculate the new output:
    * Take x, multiply by yet another set of weights weights
    * Do Hadamard of reset vector and previous output r⨀$h_{t-1}$; then matrix-transform it
    * Sum the last two, add bias, calculate tanh of it. Assuming that GRU follow the idea of LSTM, this can be considered an "updated output" than now would be eighter gated or not.
 6. Hadamard the result of last operation with (1-z).
 7. Add z⨀$h_{t-1}$ to this all

**Core idea** is the same as for LSTM, but without a hidden state (state is not hidden, it is explicitly stored in the output). It can basically "hold" the value of the output indefinitely, as long as z is high, but then if z falls down, we switch to a different form of output.

_I am not sure why r is called "reseat gate vector", but we'll see._

# Refs
* [Wiki](https://en.wikipedia.org/wiki/Gated_recurrent_unit)
* [Undestanding GRU networks](https://towardsdatascience.com/understanding-gru-networks-2ef37df6c9be) - at towards data science