# Beam Search

#text

**Beam Search**: a somewhat alpha-go-like formally coded tree of solutions for picking the best sequence, not just the best word, during translation. At each step you consider several winners (not just winner take all), explore a tree of possibilites, prune bad ones, and end up with an overall winner for a few words ahead. Introduced in (Wu 2016 Google Search paper), and later used in the [[transformers]] paper.

See also:
* [[ab-pruning]] - similar gaming strategy

# References

Yonghui Wu, Mike Schuster, Zhifeng Chen, Quoc V Le, Mohammad Norouzi, Wolfgang Macherey, Maxim Krikun, Yuan Cao, Qin Gao, Klaus Macherey, et al. Google’s neural machine translation system: Bridging the gap between human and machine translation. arXiv preprint. arXiv:1609.08144, 2016. 