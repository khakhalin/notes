# Reservoir computing

Parents: [[rnn]]
See also: [[criticality]], [[cellular_automata]], [[graph_symmetry]], [[intrinsic_plasticity]], [[complexity]]

#echo #rnn #graph #bib


Related:
* [[synfire]] - biological networks encoding temporal sequences
* [[stdp]] - what shapes them probably
* [[hopfield]] - continuous Hopfield networks as a model for network self-organization
* [[sir]] - epidemiological models

# Papers

For all echo-like networks in actual brains, see: [[synfire]] chains.

* [[Jaeger2004harness]] - first description of echo-state
* [[Lukosevicius2012echo]] - practical advice
* [[Carroll2019structure]] - playing with minimalistic structure (all weights +1, 0, or −1)
* [[Carroll2020chaos]] - is chaos good for ESN? Not always (but apparently "yes" for ODE ESN)
* [[Murray2019local]] - Hebb  + eligibility trace + random feedback weights approximate [[backprop]]

# Summary

Two very closely related terms / methods:
* **Echo-State Networks**: introduced by Jaeger, a continuous state network.
* **Liquid State Machines**: a version by Markram, a spiking network.

Highly parallelizable.

Interesting general statements:
* Regularization for the final layer (L2) helps (scholarpedia)
* Also according to scholarpedia, sparsity is overrated (nb: true only if edges are weighted!). A fully connect reservoir works quite well; if edges are weighted, sparse connectivity only helps with computational complexity. (but without references; just claims that "many authors report"). Also, obviously, if weights are allowed to be closer to 0, then fully connected isn't really quite "fully connected".

Interesting parallel with random feed-forward networks: [[Harris2020randomness]]. Random smooth kernels, like doing a kernel trick.

# Questions

* Can one use local logic to deliberately make the network less symmetric? (is the same as STPD? Basically, if two nodes are too similar, we want to make them dissimilar… Sketch a plasticity rule that would achieve that.)
* How does pruning work on echo networks? Two options for pruning here - in the output layer (which could also allow ensembling on the same reservoir), and in the reservoir itself (which could curiously interact with network properties). Use some sort of backprop-like calculation, find least useful nodes and rewire them?
* Role of intrinsic tuning?
    * Would randomizing intrinsic tuning make things better? - compare to that Goodman 2020 paper?
    * What about introducing intrinsic plasticity?
* Use local unsupervised learning to de-correlate the network. STDP-style? Hoping that the network will become less symmetric?
    * A possible biology-inspired scenario: if 2 inputs come in synchrony, prune one of them. But if a unit has no outputs, connect at random. Or better yet, always rewire (degree-preserving pruning). (This is not STDP though; it's something inspired by it, but not quite it). Could it work as yet another stochastic optimization algorithm?
    * In principle, classic STDP seems to be about gathering, strengthening evidence, which is more how the last layer works (the last layer can work on stdp alone, huh), not how decorrelation works. Decorrelation should punish neurons that corroborate too much, not reward them with connections. As, apparently, being chaotic isn't that great for an echo-state-network, maybe "classic STDP" is sorta the voice of reason, and it is the synaptic competition + local inhibition that eventually decorrelates the network?
    * If we do that, and if we keep all weights at 1 (or −1), we'll have to track the "worth" of each potential input, and once an input drops below a certain amount of "worth", randomly rewire it.
* Is brain-style inhibition much worse than "distributed inhibition" (50% of negative weights)? If we do fixed some-to-some inhibition (a mix of feed-forward and feed-back; get average activity of random M neurons, project to another random M?), will it be similar? Just how fragmented does it have to be?
* Non-monotonous activation function, like sin, so that 2 inputs produce inhibition? As an excitatory substitute to real inhibition?
* As a mix between this last idea and the idea of unsupervised weight flipping, we can rephrase it as advanced intrinsic plasticity of neurons (unsupervised)?

# To-read

Gallicchio, C., Mantas, L., & Simone, S. (2020). Frontiers in Reservoir Computing. In ESANN 2020 (pp. 559-566). BEL. https://www.esann.org/sites/default/files/proceedings/2020/ES2020-7.pdf

Rodan, A., & Tino, P. (2010). Minimum complexity echo state network. IEEE transactions on neural networks, 22(1), 131-144.
https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.739.7993&rep=rep1&type=pdf

Carroll, T. L. (2020). Do Reservoir Computers Work Best at the Edge of Chaos?. arXiv preprint arXiv:2012.01409.
https://arxiv.org/pdf/2012.01409.pdf
* Started here: [[Carroll2020chaos]]
* See also: Carroll, T. L. (2019). Mutual information and the edge of chaos in reservoir computers. arXiv preprint arXiv:1906.03186. https://arxiv.org/pdf/1906.03186.pdf

Tanaka, G., Yamane, T., Héroux, J. B., Nakane, R., Kanazawa, N., Takeda, S., ... & Hirose, A. (2019). Recent advances in physical reservoir computing: A review. Neural Networks, 115, 100-123.
https://www.sciencedirect.com/science/article/pii/S0893608019300784

Ozturk, M. C., Xu, D., & Príncipe, J. C. (2007). Analysis and design of echo state networks. Neural computation, 19(1), 111-138.
http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.877.6880&rep=rep1&type=pdf

Initialization and self-organized optimization of recurrent neural network connectivity.
Joschka Boedecker, Oliver Obst, Norbert Michael Mayer, Minoru Asada
2009, HFSP journal

Haluszczynski, A., Aumeier, J., Herteux, J., & Räth, C. (2020). Reducing network size and improving prediction stability of reservoir computing. Chaos: An Interdisciplinary Journal of Nonlinear Science, 30(6), 063136.
https://arxiv.org/pdf/2003.03178.pdf

Laje, R., & Buonomano, D. V. (2013). Robust timing and motor patterns by taming chaos in recurrent neural networks. Nature neuroscience, 16(7), 925-933. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3753043/
Twitter demo: https://twitter.com/DeanBuono/status/1401714030994026496

Optimization and applications of echo state networks with leaky- integrator neurons
H. Jaeger, Mantas Lukosevicius, D. Popovici, U. Siewert
2007, Neural Networks

A Developmental Approach to Structural Self-Organization in Reservoir Computing
Jun Yin, Yan Meng, Yaochu Jin. 2012, IEEE Transactions on Autonomous Mental Development

Ferreira, A. A., & Ludermir, T. B. (2009, June). Genetic algorithm for reservoir computing optimization. In 2009 International Joint Conference on Neural Networks (pp. 811-815). IEEE. (Available on ResearchGate)

Zhong, S., Xie, X., Lin, L., & Wang, F. (2017). Genetic algorithm optimized double-reservoir echo state network for multi-regime time series prediction. Neurocomputing, 238, 191-204. ([link](https://www.sciencedirect.com/science/article/pii/S0925231217301273?casa_token=2eoUio9k_tgAAAAA:8wf8Prlo0WAP21_-XantLVhVp5pcRKIcd0xsPfq0XdcirJEC3sL3uzmOmNKMc-tFEx3SjctkTg))

Huang, R., Li, Z., & Cao, B. (2020). A Soft Sensor Approach Based on an Echo State Network Optimized by Improved Genetic Algorithm. Sensors, 20(17), 5000. (text saved locally)

Tallec, C., & Ollivier, Y. (2017). Unbiased online recurrent optimization. arXiv preprint arXiv:1702.05043. https://arxiv.org/pdf/1702.05043.pdf

Grigoryeva, L., & Ortega, J. P. (2018). Echo state networks are universal. Neural Networks, 108, 495-508.
https://www.sciencedirect.com/science/article/abs/pii/S089360801830251X

Lukoševičius, M., & Uselis, A. (2019, September). Efficient Cross-Validation of Echo State Networks. In_International Conference on Artificial Neural Networks_(pp. 121-133). Springer, Cham. https://arxiv.org/pdf/2006.11282.pdf

Schrauwen, B., Wardermann, M., Verstraeten, D., Steil, J. J., & Stroobandt, D. (2008). Improving reservoirs using intrinsic plasticity. Neurocomputing, 71(7-9), 1159-1171.
http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.162.1362&rep=rep1&type=pdf
* Also: Steil, J. J. (2007). Online reservoir adaptation by intrinsic plasticity for backpropagation–decorrelation and echo state learning. Neural networks, 20(3), 353-364. https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.87.6689&rep=rep1&type=pdf

Carroll, T. L. (2020). Dimension of reservoir computers. Chaos: An Interdisciplinary Journal of Nonlinear Science, 30(1), 013102. https://arxiv.org/pdf/1912.06472.pdf
Attempts to actually measure the dimensionality of signals in a ESN.

Mujika, A., Meier, F., & Steger, A. (2018). Approximating real-time recurrent learning with random kronecker factors. Advances in Neural Information Processing Systems, 31, 6594-6603.  https://papers.nips.cc/paper/2018/file/dba132f6ab6a3e3d17a8d59e82105f4c-Paper.pdf

Lukoševičius, M., Jaeger, H., & Schrauwen, B. (2012). Reservoir computing trends. KI-Künstliche Intelligenz, 26(4), 365-371. http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.709.514&rep=rep1&type=pdf

Hart, A., Hook, J., & Dawes, J. (2020). Embedding and approximation theorems for echo state networks. Neural Networks, 128, 234-247. https://people.bath.ac.uk/jhpd20/publications/HartHookDawes_preprint_JAN2020.pdf

Decoupled echo state networks with lateral inhibition
Yanbo Xue, Le Yang, S. Haykin
2007, Neural Networks

Wang, X., Jin, Y., & Hao, K. (2019). Evolving Local Plasticity Rules for Synergistic Learning in Echo State Networks. IEEE Transactions on Neural Networks and Learning Systems, 31(4), 1363-1374.
(no free text online, but saved pdf locally)
It seems that they are really trying to evolve some biologically-inspired rules, inspired by Hebbian plasticity and synaptic interference.

Lukoševicius, M. (2012). Reservoir computing and self-organized neural hierarchies. Doctoral dissertation, Jacobs University, Bremen. https://d-nb.info/1035433168/34

Lukoševicius, M. (2010). On self-organizing reservoirs and their hierarchies. Jacobs University Bremen, Technical Report (25). http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.721.4547&rep=rep1&type=pdf
* Related? : Lukoševičius, M. (2012, September). Self-organized reservoirs and their hierarchies. In International Conference on Artificial Neural Networks (pp. 587-595). Springer, Berlin, Heidelberg. https://www.ai.rug.nl/minds/uploads/Self-organized_reservoir_hierarchies_ICANN12.pdf

An experimental unification of reservoir computing methods
David Verstraeten, Benjamin Schrauwen, Michiel D'Haene, Dirk Stroobandt
2007, Neural Networks

Feldman, D. P., & Crutchfield, J. P. (1998). Measures of statistical complexity: Why?. Physics Letters-Section A, 238(4), 244-252.
http://www.disca.upv.es/pabmitor/FILES/Complexity/971111_GEN_4.pdf
On Crutchfield complexity measure (that peaks for meaningful networks)

Carroll, T. L. (2020). Path length statistics in reservoir computers. Chaos: An Interdisciplinary Journal of Nonlinear Science, 30(8), 083130.
https://www.researchgate.net/profile/T_Carroll/publication/343653866_Path_Length_Statistics_in_Reservoir_Computers/links/5f3680c8458515b7291f35ec/Path-Length-Statistics-in-Reservoir-Computers.pdf

Scardapane, S., & Uncini, A. (2017). Semi-supervised echo state networks for audio classification. Cognitive Computation, 9(1), 125-135.

Lymburn, T., Khor, A., Stemler, T., Corrêa, D. C., Small, M., & Jüngling, T. (2019). Consistency in echo-state networks. Chaos: An Interdisciplinary Journal of Nonlinear Science, 29(2), 023118.
https://arxiv.org/pdf/1901.07729.pdf
Also try to drive a network towards better performance.

Manjunath, G. (2020). Memory-Loss is Fundamental for Stability and Distinguishes the Echo State Property Threshold in Reservoir Computing & Beyond._arXiv preprint arXiv:2001.00766_.
[https://arxiv.org/pdf/2001.00766.pdf](https://arxiv.org/pdf/2001.00766.pdf)
Looks very relevant!!

Rodan, A. (2012). Architectural designs of Echo State Network (Doctoral dissertation, University of Birmingham). PhD thesis
https://etheses.bham.ac.uk/id/eprint/3610/1/Alrodan12PhD.pdf
He totally considered fixed weights here, and shows that it doesn't hurt. Calls it Simple Cycle Reservoir (SCR). Looks at simple architectures.

Cui, H., Liu, X., & Li, L. (2012). The architecture of dynamic reservoir in the echo state network. Chaos: An Interdisciplinary Journal of Nonlinear Science, 22(3), 033127.
(No obvious access online. But I'm not very hopeful about this one: it seems that they just tried small-world and sale-free networks, and described how ESN worked in them)

Ceni, A., Ashwin, P., Livi, L., & Postlethwaite, C. (2020). The Echo Index and multistability in input-driven recurrent neural networks. arXiv preprint arXiv:2001.07694.
https://arxiv.org/pdf/2001.07694.pdf
How to measure "Echo index" (the quality of a reservoir). Also, show that the echo index is input-dependent.

Schiller U.D. and Steil J. J. (2005) Analyzing the weight dynamics of recurrent learning algorithms. Neurocomputing, 63C:5-23
Backpropagation decorrelation. Apparently, also show that if you try to teach the entire RNN, most changes happen in the output layer anyways.

Maass, W., Natschläger, T., & Markram, H. (2002). Real-time computing without stable states: A new framework for neural computation based on perturbations. Neural computation, 14(11), 2531-2560.
Original publication on liquid state machines.

Maass, W., Natschläger, T., & Markram, H. (2004). Computational models for generic cortical microcircuits. Computational neuroscience: A comprehensive approach, 18, 575-605.

On Explaining the Surprising Success of Reservoir Computing
Forecaster of Chaos?
https://arxiv.org/pdf/2008.06530.pdf

Using reservoir computers to distinguish chaotic signals
https://arxiv.org/pdf/1810.04574.pdf

Testing Dynamical System Variables for Reconstruction
https://arxiv.org/pdf/1808.08897.pdf

Popular account: 
https://www.quantamagazine.org/machine-learnings-amazing-ability-to-predict-chaos-20180418/ 

Maass, W., & Markram, H. (2004). On the computational power of circuits of spiking neurons. Journal of computer and system sciences, 69(4), 593-616.

Sussillo, D. and Abbott, L. F. (2009). Generating Coherent Patterns of Activity from Chaotic Neural Networks. Neuron, 63(4):544–557.
https://www.sciencedirect.com/science/article/pii/S0896627309005479

Sussillo, D., & Barak, O. (2013). Opening the black box: low-dimensional dynamics in high-dimensional recurrent neural networks. Neural computation, 25(3), 626-649.
https://www.mitpressjournals.org/doi/full/10.1162/NECO_a_00409

Predicting slow and fast neuronal dynamics with machine learning (2019)
https://www.researchgate.net/profile/Rosangela_Follmann/publication/337359853_Predicting_slow_and_fast_neuronal_dynamics_with_machine_learning/links/5dd72523299bf10c5a26c2bc/Predicting-slow-and-fast-neuronal-dynamics-with-machine-learning.pdf

Predicting bursting in a complete graph of mixed population through reservoir computing (2020)
https://journals.aps.org/prresearch/pdf/10.1103/PhysRevResearch.2.033338

Pogodin, R., Corneil, D., Seeholzer, A., Heng, J., & Gerstner, W. (2019). Working memory facilitates reward-modulated Hebbian learning in recurrent neural networks. arXiv preprint arXiv:1910.10559. 
https://arxiv.org/pdf/1910.10559.pdf 
Reservoir computer + a "working memory network"

Rotermund, D., & Pawelzik, K. R. (2019). Biologically plausible learning in a deep recurrent spiking network. bioRxiv, 613471.
https://www.biorxiv.org/content/10.1101/613471v1.full

Stimberg, M., Goodman, D. F., & Nowotny, T. (2019). [Brian2GeNN: accelerating spiking neural network simulations with graphics hardware](https://www.nature.com/articles/s41598-019-54957-7). Scientific Reports.
Spiking network simulator that is apparently so optimized that it runs very fast. Check before writing any custom spiking models.

Sussillo, D., & Abbott, L. F. (2012). Transferring learning from external to internal weights in echo-state networks with sparse connectivity._PLoS One_,_7_(5).

Bürger, J., Goudarzi, A., Stefanovic, D., and Teuscher, C. (2015, July) Hierarchical composition of memristive networks for real-time computing. In Nanoscale Architectures (NANOARCH), 2015 IEEE/ACM International Symposium on (pp. 33-38)
Allegedly a link between reservoir computing and neuromorphic computing (I'm sure not the only one, but cited by Jaeger)

Paassen, B., & Schulz, A. (2020). Reservoir memory machines. arXiv preprint arXiv:2003.04793.
https://arxiv.org/abs/2003.04793
https://gitlab.ub.uni-bielefeld.de/bpaassen/reservoir-memory-machines
Seems like a hybrid of trainable turing machines and reservoir computing? But not really reservoir computing.

Holzmann, G. (2007). Echo state networks in audio processing. Internet Publication.
http://grh.mur.at/sites/default/files/ESNinAudioProcessing.pdf

# General Refs

People:
* T. L. Carroll: https://scholar.google.ru/citations?user=3MJYhIMAAAAJ
* Herbert Jaeger: https://scholar.google.ru/citations?user=0uztVbMAAAAJ

Jaeger, H. (2007). Echo state network. scholarpedia, 2 (9), 2330.
http://www.scholarpedia.org/article/Echo_state_network