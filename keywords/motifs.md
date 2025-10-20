# Motifs

Parent: [[graphs]] / [[graph_properties]]
See also:

#graph


For the toy model:

* Mutiplying by A is equivalent to calculting a mean. So if we take vectors, multiply by A, and then apply W, it's equivalent to an update of X.
* A should either include loops, to preserve self-info, or there should be self-transform, and others-transform.
* There should be 2 ways to write a toy model in TF: write graph art as a custom keras layer, or use multiplication by a matrix from TF (keras layer?) within a normal program, and then train it. Like, start tape, run net (only one TF object in it), calculate loss, apply. In a loop. Probably possible to write it in one cell, without far-reaching functions, to make it even more compact. The only difference from np is that this part is TFed, and then trained.
* Write elegant recurrent modular network creator? `link((pieces), p)` where pieces = sets of elements (potentially, defined as lists of edges on these nodes, or a dict), p is probability, and returns a list edges (or a dict). Make it recurrently create the union of all lists of edges (or dicts). Probably dicts.
* Model takes a network, calculates embeddings (from randomness or ones), then compares i and j and predicts (based on their embeddings) whether they are (should be) connected or not.
* Then compare model(edge_remover(network, i, j)) with "whether actually e_ij exists".
* Then test on a different network created by the same creator (transfer).
* Look at small graphs, count cycles. (Use my "all graphs", and count cycles manually; use it as a training set).

# Practice

`nx.triadic_census` - there seem to be standard funny names for all 16 triangular motifs, and networkx knows them. Stuff like 003, 

For graph embedding tools, we can calculate the number of triples every node participates in, and use them as properties.

