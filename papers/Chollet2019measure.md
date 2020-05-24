# The measure of Intelligence

#meaning #transfer #cellularautomata

Chollet, F. (2019). The Measure of Intelligence. arXiv preprint arXiv:1911.01547.
https://arxiv.org/abs/1911.01547

Endorsed by many great  people. Essentially, on how transfer and generalized intelligence may look, and how they may be tested. That's why I'm placing it in "Self-supervised learning" section: it is not really about it, but it is something that goes beyond approximating a data set; something like "transfer par excellence!""

# Related Kaggle challenge

The paper was followed by a Kaggle challenge developed after it:
https://www.kaggle.com/c/abstraction-and-reasoning-challenge

Nice set of examples: repeat the pattern, move one part relative to another, color according to a certain rule, fill the gaps, etc. Like a set of nice IQ tests on a grid. A human (me) has no problem with some, and can figure others after 30-60s of concentration. But it's quite obvious that modern types of AI would have enormous problems with it.

Some vizualizations with grids:
* https://towardsdatascience.com/an-impossible-ai-challenge-fdc9c2e3858c


Here's an interesting attempt to solve them using **cellular automata**:
* https://www.kaggle.com/arsenynerinovsky/cellular-automata-as-a-language-for-reasoning
* https://www.kaggle.com/teddykoker/training-cellular-automata-part-ii-learning-tasks

Their approach (may be similar to that of Mordvintsev?) - use a convolutional network as a cellular automaton that recurrently processes the image. They try to alternate between teaching it to transform the input image towards the output, and transforming output towards the output (stabilization). It works reasonably well.

I wonder if it can be combined with [[wave_function_collapse]].