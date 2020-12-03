# Graphs, Networks, and Graphical Methods

#bib #graphs #networks

**Basic graph techniques**
* [[graph_intro]] - Intro to graphs
* [[graph_th]] - Random notes in graph theory
* [[algos_graph]] - Classic algorithms on graphs
* [[la_net]] - Linear algebra on graphs
    * Spectral techniques - [youtube](https://www.youtube.com/watch?v=0eNQnc0eOB4&list=PL1OaWjIc3zJ4xhom40qFY5jkZfyO5EDOZ&index=5) #todo

**Network science**
* [[graph_properties]] - How to characterize graphs and networks
* [[centrality]]
    * [[pagerank]] - Pagerank and link analysis
* [[motifs]] - Motif analysis #todo

**Generative models for graphs**
* [[graph_generate]] - Generative models for graphs
    * [[erdos_graph]] - basis
    * [[small_world]]  - SW models
    * [[kronecker_graph]] - Fractal model for graph generation
    * [[generative_graphRNNs]] - generating graphs with DL RNNs

**Graph embeddings**
* [[graph_embedding]] - Learning graph representations, random walks
* Label Propagation - [youtube](https://www.youtube.com/watch?v=8szDxEU9u8g&list=PL1OaWjIc3zJ4xhom40qFY5jkZfyO5EDOZ&index=7) #todo
* [[node2vec]] - fancier use of random walks for graph embedding
* [[transE]] - Knowledge Graph method
* [[deep_graphs]] - Deep Learning on graphs
    * [[graphsage]] - a particular implementation of DL for graphs

**Processes on graphs**
* [[graph_cascades]] - Decision-based models and cascades
* [[sir]] - Pandemic-like models on graphs (SIR, SIS etc.)
* [[influence_maximization]] - greedy hill climbing, sketches method
* [[outbreak_detection]] - where to place sensors to monitor the network

Reviews and intros:
* [[Bacciu2019gentlegraph]] - Intro to Graph NNs
* [[Porter2019nonlinearity]] - Dynamics on graphs, including games on graphs, oscillators and what not

Other papers:
* [[Bojanek2020cyclic]] - using triplet motifs to classify (neural) activity on graphs

# To-Read: ML on Graphs

A collection of hot papers from Nov 2020:
https://www.reddit.com/r/MachineLearning/comments/j6wzut/r_latest_developments_in_graph_neural_networks_a/

Review:
Hamilton, W. L., Ying, R., & Leskovec, J. (2017). Representation learning on graphs: Methods and applications. arXiv preprint arXiv:1709.05584.
https://arxiv.org/pdf/1709.05584.pdf

https://medium.com/dair-ai/an-illustrated-guide-to-graph-neural-networks-d5564a551783

https://thegradient.pub/transformers-are-graph-neural-networks/

Kipf, T. N., & Welling, M. (2016). Semi-supervised classification with graph convolutional networks. arXiv preprint arXiv:1609.02907.
Graph convolutional networks (analogous to SAGE, but from a different group of people). 4.5k citations, so super-influential.
[https://arxiv.org/pdf/1609.02907.pdf](https://arxiv.org/pdf/1609.02907.pdf)

[[Li2017graph]] -  Gated graph sequence neural networks (I started summarizing it, but haven't finished yet)

Benchmarking Graph Neural Networks (2020). Vijay Prakash Dwivedi Chaitanya K. Joshi Thomas Laurent Yoshua Bengio Xavier Bresson
https://arxiv.org/pdf/2003.00982.pdf
Looking for which patterns seem to hold for successful graph neural networks. A review.

Billings, J. C. W., Hu, M., Lerda, G., Medvedev, A. N., Mottes, F., Onicas, A., ... & Petri, G. (2019). Simplex2Vec embeddings for community detection in simplicial complexes. arXiv preprint arXiv:1906.09068.
https://arxiv.org/pdf/1906.09068.pdf
Recommended by Irina. #algtop
	
Battaglia, P. W., Hamrick, J. B., Bapst, V., Sanchez-Gonzalez, A., Zambaldi, V., Malinowski, M., ... & Gulcehre, C. (2018). Relational inductive biases, deep learning, and graph networks. arXiv preprint arXiv:1806.01261.
https://arxiv.org/abs/1806.01261
Was described as practical and particularly well-written.

David Belli (2019). Generative graph transformer
https://davide-belli.github.io/generative-graph-transformer.html
Blog post with math and pretty pics: investigate!

Jin, W., Ma, Y., Liu, X., Tang, X., Wang, S., & Tang, J. (2020). Graph Structure Learning for Robust Graph Neural Networks. arXiv preprint arXiv:2005.10203.
https://arxiv.org/abs/2005.10203
Pytorch implementation:
https://github.com/ChandlerBang/Pro-GNN

Dehmamy, N., Barabási, A. L., & Yu, R. (2019). Understanding the Representation Power of Graph Neural Networks in Learning Graph Topology. arXiv preprint arXiv:1907.05008.
https://arxiv.org/abs/1907.05008

Bojchevski, A., Shchur, O., Zügner, D., & Günnemann, S. (2018). Netgan: Generating graphs via random walks. arXiv preprint arXiv:1803.00816.
https://arxiv.org/pdf/1803.00816.pdf

Yaron Lipman - Deep Learning of Irregular and Geometric Data (a clear 30-min presentation)
https://www.youtube.com/watch?v=fveyx5zKReo

Graph rewriting (related to generative approaches?)
https://en.wikipedia.org/wiki/Graph_rewriting
https://en.wikipedia.org/wiki/Clean_(programming_language)

Representation Learning on Graphs with Jumping Knowledge Networks
https://arxiv.org/abs/1806.01261

# Network Science

Rosvall, M., & Bergstrom, C. T. (2008). Maps of random walks on complex networks reveal community structure. Proceedings of the National Academy of Sciences, 105(4), 1118-1123.
https://www.pnas.org/content/pnas/105/4/1118.full.pdf

Clune, J., Mouret, J. B., & Lipson, H. (2013). The evolutionary origins of modularity. Proceedings of the Royal Society b: Biological sciences, 280(1755), 20122863.
https://royalsocietypublishing.org/doi/pdf/10.1098/rspb.2012.2863
500 citations: pretty cool for a bio paper

Go through publications of D. Krioukov (Northeastern):
https://web.northeastern.edu/dima/pub/
He has some publications (older, down the page) on percolation on networks.

Tiago P. Peixoto
https://scholar.google.com/citations?user=gQJ0a7YAAAAJ&hl=en&oi=ao
Among other things, works on 

Video: The physics of brain network architecture, function, and control
Lecture by Danielle Bassett
https://www.youtube.com/watch?v=u-l-rdKvd8U

# Spectral techniques

Tao, T. (2013). Outliers in the spectrum of iid matrices with bounded rank perturbations. Probability Theory and Related Fields, 155(1-2):231–263.

P. Van Mieghem. Graph Spectra for Complex Networks. Cambridge University Press, Cambridge, UK, 2013.

# Cliques and ensembles

P. Bonacich. Factoring and weighting approaches to clique identification. J. Math. Sociol.,
2:113–120, 1972.

# Network topology analysis

da Fontoura Costa, L. (2018). What is a Complex Network?(CDT-2).
[ResearchGate link](https://www.researchgate.net/profile/Luciano_Da_F_Costa/publication/324312765_What_is_a_Complex_Network_CDT-2/links/5aca49ba4585151e80a91abd/What-is-a-Complex-Network-CDT-2.pdf)
Hmm. A very short illustrated mini-review, and the topic is good, but is it a good paper? Check.

Mackevicius, E. L., Bahle, A. H., Williams, A. H., Gu, S., Denisenko, N. I., Goldman, M. S., & Fee, M. S. (2019). Unsupervised discovery of temporal sequences in high-dimensional datasets, with applications to neuroscience. Elife, 8, e38471.
https://elifesciences.org/articles/38471
Reconstructing graph from neural activity, using something that resembles graph convolution networks?

# Processes on networks

Watts, D. J., & Strogatz, S. H. (1998). Collective dynamics of ‘small-world’networks. nature, 393(6684), 440.
Seminal, very short (2 pages) work. 40k refs!! That's because this particular type of rewiring became known as the **Watts-Strogatz model**, and everyone who used it, referenced this original paper.

R. Pastor-Satorras, C. Castellano, P. Van Mieghem, and A. Vespignani. Epidemic processes
in complex networks. Rev. Mod. Phys., 87:925–979, 2015.
Sounds like a seminal review...

M. Asllani, D. M. Busiello, T. Carletti, D. Fanelli, and G. Planchon. Turing patterns in
multiplex networks. Phys. Rev. E, 90:042814, 2014.

N. E. Kouvaris, S. Hata, and A. Díaz-Guilera. Pattern formation in multiplex networks. Sci.
Rep., 5:10840, 2015.

N. Masuda, M. A. Porter, and R. Lambiotte. Random walks and diffusion on networks. Phys.
Rep., 716–717:1–58, 2017.

M. O. Jackson and Y. Zenou. Games on networks. In P. Young and S. Zamir, editors,
Handbook of Game Theory Vol. 4, pages 95–163. Elsevier, 2014.

# Graph-producing automata

Eckel, W. (2015). Creating an Artificial World with a New Kind of Cellular Automata. arXiv preprint arXiv:1507.05789.
https://arxiv.org/pdf/1507.05789.pdf

Praba, B., & Saranya, R. (2020). Application of the graph cellular automaton in generating languages. Mathematics and Computers in Simulation, 168, 111-121.

Mukhopadhyay, D. (2012, September). Generating Expander Graphs Using Cellular Automata. In International Conference on Cellular Automata (pp. 52-62). Springer, Berlin, Heidelberg.

Hassan, B. A., & Hiesinger, P. R. (2015). Beyond molecular codes: simple rules to wire complex brains. Cell, 163(2), 285-291.
A review of how simple developmental rules (in this case actual dendrites in 3D, growing and stopping) can produce different stochastic patterns, depending on the encoded rules.

Lieberman, E., Hauert, C., & Nowak, M. A. (2005). Evolutionary dynamics on graphs. Nature, 433(7023), 312-316.
Open version:
http://web.mit.edu/manoli/www/publications/Lieberman_RECOMB_05.pdf
Weird, but judging from the citations (~1k), very  popular! The 1-sentence summary is that some graphs can amplify the selection, while some graphs can suppress it.

# Dynamic on dynamic

Zambra, M., Testolin, A., & Maritan, A. (2019). [Emergence of Network Motifs in Deep Neural Networks](https://arxiv.org/pdf/1912.12244.pdf). arXiv preprint arXiv:1912.12244.

N. Perra, B. Gonçalves, R. Pastor-Satorras, and A. Vespignani. Activity driven modeling of time varying networks. Sci. Rep., 2:469, 2012. 

# Computations on graphs

Farhoodi, R., Filom, K., Jones, I. S., & Kording, K. P. (2019). On functions computed on trees. arXiv preprint arXiv:1904.02309. https://arxiv.org/abs/1904.02309

# Centrality measures

#centrality

J. Flores and M. Romance. On eigenvector-like centralities for temporal networks: Discrete
vs. continuous time scales. J. Comp. App. Math., 330:1041–1051, 2018.
Introduces eigenvector centrality for dynamic networks

S. Praprotnik and V. Batagelj. Spectral centrality measures in temporal networks. Ars Math.
Contemp., 11(1):11–33, 2015.

P. Grindrod and D. J. Higham. A dynamical systems view of network centrality. Proc. Royal
Soc. A, 470:20130835, May 2014.
Introduces Katz and Bonacich centrality measures for dynamic networks

K. Lerman, R. Ghosh, and J. H. Kang. Centrality metric for dynamic networks. In Proc.
Eighth Workshop on Mining and Learning with Graphs, pages 70–77. ACM, 2010.
Introduces Katz and Bonacich centrality measures for dynamic networks

P. Grindrod and D. J. Higham. A matrix iteration for dynamic network summaries. SIAM
Rev., 55:118–128, Jan 2013.
Introduces "communicatibility centrality"

Fagiolo, G. (2007). Clustering in complex directed networks. Physical Review E, 76(2), 026107.

# Process complexity

Toker, D., Sommer, F. T., & D’Esposito, M. (2020). [A simple method for detecting chaos in nature](https://www.nature.com/articles/s42003-019-0715-9). Communications Biology, 3(1), 1-13.
Seems practical, and reasonable. They first make sure the data is ready to be analyzed (noise, downsampling). Takes considerable part of the text. Then they use a "0-1 test", resulting in one value K that is 0 for periodical, and approaches 1 for chaotic. Seems to be based on predictions for brownian motion, but need to look more closely.

F. A. Rodrigues, T. K. DM. Peron, P. Ji, and J. Kurths. The Kuramoto model in complex
networks. Phys. Rep., 469:1–98, 2016

A Arenas, A Díaz-Guilera, J Kurths, Y Moreno, and C Zhou. Synchronization in complex
networks. Phys. Rep., 469:93–153, 2008.

O’Keeffe, K. P., Hong, H., & Strogatz, S. H. (2017). Oscillators that sync and swarm. Nature communications, 8(1), 1504.
https://www.nature.com/articles/s41467-017-01190-3
About collective self-organized behaviors. Can be useful for the modeling class maybe?

# Neuromorphic computing

Kendall, J. D., & Kumar, S. The building blocks of a brain-inspired computer. Applied Physics Reviews, 7(1), 011305.
https://aip.scitation.org/doi/10.1063/1.5129306
14 January 2020. A rather long review on the next steps in neuromorhic computing. With a review of what aspects of the brain are reflected, which ones aren't, and what to do with it?

# Misc

Perry Zurn and Danielle S. Bassett (2020). Network architectures supporting learnability
https://royalsocietypublishing.org/doi/10.1098/rstb.2019.0323
On how only some architectures can understand only some structures, and whether there's a link between the two? Something philosophy, not only math?

# Resources

Graphvis online:
https://dreampuf.github.io/GraphvizOnline/

Famous cs224:
http://web.stanford.edu/class/cs224w/
Sberloga's summaries on notion:
https://www.notion.so/Sberloga-with-Graphs-12fafe3224e1483eb435a16aa990e1a1