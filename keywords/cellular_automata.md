# Cellular Automata

Parent: [[complexity]], [[00_articles_math]], [[modeling]]
See also: [[neurostochastic]], [[randomness]]

#automata #dynamic #chaos


Subtopics:
* [[neuro_cell_automata]] - deep dreaming, something in-between a CA and a recurrent network
* [[graph_automata]] - cellular automata on graphs

# To Read

Zenil, H., & Villarreal-Zapata, E. (2013). Asymptotic behavior and ratios of complexity in cellular automata. International Journal of Bifurcation and Chaos, 23(09), 1350159.
https://arxiv.org/pdf/1304.2816.pdf
Measures of complexity (Shannon's block entropy and Kolmogorov) applied to 1D cellular automata. They try to make some general statements. Give a look.

Cellular automata for music (a review from 2005, but seems fun)
https://web2.qatar.cmu.edu/~gdicaro/15382/additional/generative-music-ca-review.pdf

# Wolfram Code

**Wolfram code** for an 1d automaton (most often used with neighborhood of 3) is a compact way to fully describe this automaton. As 3 bits → 1 bit, there are 2^3 = 8 combinations that come as in. Two outputs are possible, so the total space of rules is 2^8 = 256. If you write all outputs out as a binary sequence, the rule becomes a binary number. For example, "Rule 30" is cool (chaotic-looking). Wolfram wrote numbers backwards, so leading bit corresponds to 111, and trailing to 000, but from the last to the first you go in standard binary order.

Wolfram defined 4 classes:
1. Converge to a stable state (e.g. 4, 172)
2. Converge to periodic state (50, 173)
3. Chaotic (60, 150)
4. A large-scale chaotic (dynamic) interplay of periodic regions (62, 110)

# Refs

Cool simple visual intro
https://matplotlib.org/matplotblog/posts/elementary-cellular-automata/