# Harnessing nonlinearity

Jaeger, H., & Haas, H. (2004). Harnessing nonlinearity: Predicting chaotic systems and saving energy in wireless communication. Science, 304(5667), 78-80.
Pdfs: [jstor](https://www.jstor.org/stable/pdf/3836613.pdf), [semanticscholar](https://pdfs.semanticscholar.org/8922/17bb82c11e6e2263178ed20ac23db6279c7a.pdf)

#rnn #echo

#halfthere

Original communication about reservoir computing (aka #echo state network). 2000+ citations. A randomly connected network that propagates signals. Some nodes are assigned inputs at random, some nodes are assigned outputs. Then you train one readout layer to produce something meaningful, using the original network randomness as a library of behavioral repertoirs (filters). 

Simple math: essentially regression. 

Good for predicting chaotic processes (for example, see: https://www.quantamagazine.org/machine-learnings-amazing-ability-to-predict-chaos-20180418/ )