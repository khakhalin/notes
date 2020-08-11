# Modularity-Impact: Signed Group Centrality

Magelinski, T., Bartulovic, M., & Carley, K. M. (2020). Modularity-Impact: a Signed Group Centrality Measure for Complex Networks. arXiv preprint arXiv:2003.00056.
https://arxiv.org/abs/2003.00056

#centrality #networks

See also: [[09_Graphs]] / [[graph_properties]], [[sir]]

Like classic Newman's modularity, but signed, helping to distinguish hubs from bridges. Good for targeted interventions, such as **immunization strategies**, which are essentially **network attacks**. We want to maximally disjoint the network, by removing a minimal number of nodes.

Most common measure of modularity: **Newman modularity**. Community detection on a graph can be phrased as defining sets of nodes, such that they maximize modularity. The task is NP-hard, and the most popular approximate method is called **Louvain method**. It's not without a flaw however (doesn't guarantee connectedness?), and there's a better method called **Leiden grouping**.

**Community-centrality** is derived from modularity matrix, but actually doesn't measure how much each node contributes to its community, but rather how much it may contribute. Also, modularity matrix is dense, so working with it for large graphs is hard.

Older approach to immunization: (Masuda 2009), that looked at the change in eigenvalues for a group (subgraph) if each node is deleted from this subgraph, thus identifying nodes that disjoint the network…

> I don't understand their explanation. Would have to read the original paper if it proves to be important. Somehow they claim that "nodes that link communities will be rated highly".	

One of the best way to evaluate node vulnerability is via **SIR model** ([[sir]]): whether deletion of a set of node prevents transmission from one group to another. As a proxy: **betweenness centrality**.

#halfthere

# Refs

Naoki Masuda. Immunization of networks with community structure. New Journal of Physics,
11(12):123018, 2009.

Juan G Restrepo, Edward Ott, and Brian R Hunt. Weighted percolation on directed networks.
Physical review letters, 100(5):058701, 2008.
Node vulnerability: how much the first (largest) eigenvalue changes if this node is deleted.

Bruno Requio da Cunha, Juan Carlos Gonzlez-Avella, and Sebastin Gonalves. Fast Fragmentation of Networks Using Module-Based Attacks. PLOS ONE, 10(11):e0142824, November 2015.
https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0142824