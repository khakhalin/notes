# Long Short-Term Memory (LSTM)

#rnn

Path: [[09_Graphs]]
See also: [[gru]]

Introduced in 1997 by Schmidhuber. Solves the issue of vanishing gradients for recurrent networks.

**Formulas:**
#todo

**Core ideas:** 
* Each cell propagates not just one value (output), but two values: **Cell state** (C, that is sorta hidden), and **output** (h)
* The State can basically be held indefinitely, as long as z=1. But if  z is relaxed to lower values, old value is "forgoten" and new value is calculated. This is the **gating** idea, from LSTM. Traditionally (again, after LSTM), this signal is called **forget gate layer**, and not "remember", even though "forgetting" happens when it is set at 0. The gate is vectorized via Hadamard opearator, so some coordinates can be gated, while some others would be passing through at the same time.
* The "new value" (the actual informative stuff) is calculated by everything under tanh.
* Then another gating vector, similar to z, but called **input gate layer** (i)  gates how much of this tanh is fed into the state.
* And then yet another layer leading to a gate - **output gate layer** - decides how much of tanh(C) - of transformed state - is outputted as h. So some serious gating here: no guarantee that a cell would output anything non-zero!

There are also lots of variations: some "peepholes" (???), and what not (mostly, other types of mixing of these multiple streams of info).

# References

http://colah.github.io/posts/2015-08-Understanding-LSTMs/
Absolutely key reading by Christopher Olah - simple and clear explanation.