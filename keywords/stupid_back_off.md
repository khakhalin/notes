# Stupid back-off
#text

**Stupid Back-off**: an official name for a simplistic Markovian text-generation model, where you learn a bunch of n-grams. Then when faced with a generation task, you match your seed n-gram with a dictionary of n-grams. If no match, or low match, look for (n-1)gram, and so on, until there's a match with decent count (and thus, probability). Some "Brants 2007" paper. The description is from the [Jurafsky Martin 2019 textbook](https://web.stanford.edu/~jurafsky/slp3/3.pdf) on language processing.
