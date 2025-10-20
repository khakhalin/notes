# Porter 2019 Nonlinearity plus networks

Mason A Porter
https://arxiv.org/abs/1911.03805 

See also: [[graphs]], [[sir]], [[graph_properties]]

#review #networks #dynamic

Subjective review (rather short, but 201 references!). By "networks" they really mean stochastic graphs, and not ANNs. One para intro to basics (degrees etc.). Temporal network: changes over time. 

Intro to #multilayer representation, but not very clear. (What are those "aspects"? What does this figure show?) For time representation, each time point = one layer; nodes are typically connected between time-layers with undirected edges, but they claim that it may be more productive to use forward-facing directed edges, not necessarily between adjacent layers. This creates a "supra-adjacency matrix" $A_m$.

Introduces #centrality measures that were generalized to temporal networks: 
* Bonacich
* Katz
* communicability
* pagerank
* eigenvector. 
Eigenvector-based centralities are easy to calculate, and slightly diff matrices produce slightly diff centralities (hubs, authorities etc.) References **Perron–Frobenius theorem**. A bunch of nice links about stochastic block models, detection of communities (including their split / merge).

**Activity-driven models**: a name for when the network changes due to processes on it. References the "original model of Perra et al":  N. Perra, B. Gonçalves, R. Pastor-Satorras, and A. Vespignani. Activity driven modeling of time varying networks. Sci. Rep., 2:469, 2012. 

Continuous-time models _that I skipped_

**Dynamical processes on networks**: refs for:
* coupled oscillators, 
* games on graphs
* spread of diseases and opinions.

Notably doesn't even mention spiking networks!

Then moves to stochastic processes. "Reaction–diffusion equations and Turing patterns on networks" and "rich theory of Laplacian dynamics on graphs (and concomitant ideas from spectral graph theory)" - four links here (all technically harvested)

Finishes with brief overview of how simplicial complexes relate to that.


