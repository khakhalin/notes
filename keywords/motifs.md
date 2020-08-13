# Motifs

Parent: [[09_Graphs]] / [[graph_properties]]

* Repeat graph  laplacians, 
* Finish those graph lectures about laplacians and such

For the toy model:

* Mutiplying by A is equivalent to calculting a mean. So if we take vectors, multiply by A, and then apply W, it's equivalent to an update of X.
* A should either include loops, to preserve self-info, or there should be self-transform, and others-transform.

# Practice

`nx.triadic_census` - there seem to be standard funny names for all 16 triangular motifs, and networkx knows them. Stuff like 003, 

For graph embedding tools, we can calculate the number of triples every node participates in, and use them as properties.

