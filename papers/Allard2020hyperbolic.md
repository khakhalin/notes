# Navigable maps of structural brain networks across species
Antoine Allard,M. Ángeles Serrano. Published: February 3, 2020
https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1007584

#net #neuro #connectome

To quantify brain connectomes from different species, they try to navigate them using greedy routing (visit the closest neighbor, in terms of distance). Then compare the ratio of successfull greedy paths to shortest paths. 

But then critically, they navigate not in Eucledian space (where it doesn't work at all), but in a some very special hyperbolic space! And there it apparently works great (two types of paths become very similar), and can be used to classify connectomes (flies, humans - everything together).

This hyperbolic space is based on these two papers:
* Boguná, M., Papadopoulos, F., & Krioukov, D. (2010). Sustaining the internet with hyperbolic mapping. Nature communications, 1(1), 1-8.
* Krioukov, D., Papadopoulos, F., Kitsak, M., Vahdat, A., & Boguná, M. (2010). Hyperbolic geometry of complex networks. Physical Review E, 82(3), 036106. _This second paper is review-like, but quite mathy, and goes in depth about hyperbolic topologies, how they work, and how a statistical model based on them differes from some alternative statistical models.._

The way it works is that each node is represented by a point in radial coordinates (θ,r), with θ ~ similarity, and r ~ popularity (not defined, but optimized; see below). Then each pair of nodes is linked with a probability p() given by some moderately fancy formula (with exponentiation) that depends only on the geodesic hyperbolic distance between each pair of nodes. And also some sort of inverse temperature as a parameter.

Parameters (θ,r) for each point are not calculated explicitly, but are optimized using max likelihood maximization. Essentially they plug the whole formula Network_instance(P_connection(distances(coordinates))))) into an optimizer, and find a distribution that produces networks as similar to the observed network as possible.

Lots of fun pictures with hyperbolic embeddings in 2D disks.

How that after optimization r is like hubness, and θ is like "similarity" in some way. Correctly infer neuroanatomical communities. _But is it strange at all, given their optimization setup? Wouldn't any model yield some results, if you set it up like that?_

Claim that their models perform "strkingly well" (correct predictin of edges at ~80-90%), and can reproduce topological properties, like "path motifs" (something like a network backbone? they have a ref), rich clubs, etc.