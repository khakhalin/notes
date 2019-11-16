# Porter 2019 Nonlinearity plus networks

https://arxiv.org/abs/1911.03805 

Mason A Porter

#review #networks #dynamic

Subjective review. By "networks" they really mean stochastic graphs, and not ANNs; one para intro to basics (degrees etc.) Temporal network: changes over time. Intro to multilayer representation, but not very clear (what are "aspects"? What does this figure show?) For time representation, each time point = one layer; nodes are typically connected between time-layers with undirected edges, but they claim that it may be more productive to use forward-facing directed edges, not necessarily between adjacent layers. This creates a "supra-adjacency matrix" $A_m$.

Introduce centralities that were generalized to temporal networks: @Bonacich, Katz, @communicability, pagerank, and eigenvector. Eigenvector-based centralities are easy to calculate, and diff matrices produce diff centralities (hubs, authorities etc.) References @Perron–Frobenius theorem. A bunch of nice links about stochastic block models, detection of communities (including their split / merge).

**Activity-driven models**: a name for when the network changes due to processes on it. References the @ original model of Perra et al":  N. Perra, B. Gonçalves, R. Pastor-Satorras, and A. Vespignani. Activity driven modeling of time varying networks. Sci. Rep., 2:469, 2012. 

Continuous-time can probably be skipped.

**Dynamical processes on networks**: refs for coupled oscillators, games, spread of diseases and opinions. Notably doesn't even mention spiking networks!

Then moves to stochastic processes.

@ "reaction–diffusion equations and Turing patterns on networks" and "rich theory of Laplacian dynamics on graphs (and concomitant ideas from spectral graph theory)" - four links here.

Finishes with brief overview of how simplicial complexes relate to that.