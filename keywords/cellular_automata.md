# Cellular Automata

#cellular #dynamic #chaos

See also: [[bib_neurostochastic]], [[randomness]]

**Wolfram code** for 1d automata (most often used with neighborhood of 3). As 3 bits → 1 bit, there are 2^3 = 8 combinations that come as in. Two outputs are possible, so the total space of rules is 2^8 = 256. If you write all outputs out as a binary sequence, the rule becomes a binary number. For example, "Rule 30" is cool (chaotic-looking). Wolfram wrote numbers backwards, so leading bit corresponds to 111, and trailing to 000, but from the last to the first you go in standard binary order.

Wolfram defined 4 classes:
1. Converge to a stable state (e.g. 4, 172)
2. Converge to periodic state (50, 173)
3. Chaotic (60, 150)
4. A large-scale chaotic (dynamic) interplay of periodic regions (62, 110)

# Refs

Cool simple visual intro
https://matplotlib.org/matplotblog/posts/elementary-cellular-automata/