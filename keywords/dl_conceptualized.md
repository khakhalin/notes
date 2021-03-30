# Why DL even works; limitations; meanings

Parents: [[06_DL]]

#dl 


# Place of DL within ML

> **Parallels with ensemble methods?** It is my impression that while deep DL may be considered a series of [[embedding]]-like transformations, wide DL also resemble ensemble methods ([[05_Ensembles]]), as essentially it's like having lots of subnetworks contributing different opinions over the answer. Which may be related to [[dropout]] and [[ticket]] theories as well. But I need to read more to be sure.

# Caveats and limitations of DL

**Overfitting** and **struggle with generalizing**: with that many parameters, and flexible expression, a wide DL is really succeptible to overfitting. So cross-validating for generalization is important. But also, sometimes overfitting is not a problem, as data is stereotypical, just scattered, and what you need is to memorize lots of examples. That's when DL shines.

Learning physics however is hard (inferring simple rules) from diverse data. And a related problem - **hard to encode priors**; say for physics, or stock markets, or self-driving cars. Need special architectures here, or some clever tricks.

**Interpretability** (see [[interpretability]]): tends to be a black box -like solution, which may be a problem ([[ethics]], [[fairness]]). Although recent research.

# Refs

Limitations of DL, by Steve Brunton, 2019:
https://www.youtube.com/watch?v=6VEPQ_jOJ-Q