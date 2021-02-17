# Reservoir computing

#echo #rnn #graphs #bib

Parents: [[07_RNNs]]
See also: [[criticality]], [[cellular_automata]], [[graph_symmetry]]

Related:
* [[synfire]] - biological networks encoding temporal sequences
* [[stdp]] - what shapes them probably
* [[hopfield]] - continuous Hopfield networks as a model for network self-organization
* [[sir]] - epidemiological models

# Papers

* [[Jaeger2004harness]] - first description of echo-state
* [[Lukosevicius2012echo]] - practical advice
* [[Carroll2019structure]] - playing with minimalistic structure (all weights +1, 0, or −1)
* [[Carroll2020chaos]] - is chaos good for ESN? Not always (but apparently "yes" for ODE ESN)

For echo-state networks in actual brains, see: [[synfire]] chains

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

* When quantifying networks, can we use richness of echos as a proxy for actual performance, to avoid testing the performance?
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
* As a mix between this last idea and the idea of unsupervised weight flipping, we can rephrase it as advanced intrinsic plasticity of neurons (unsupervised).

# To-revisit and describe

Murray, J. M. (2019). Local online learning in recurrent networks with random feedback. eLife, 8, e43299. x>Win>echo>Wou>y, dh/dt = -h/tau stable solution for each element, tanh() activation. Minimizes square distance, applies gradient descent. Then tries to make biologically plausible: drops non-local term (refuses to optimize activity of other nodes that feed _from_ this node by changing inputs _to_ this node). And instead of reverting weights of Wout, uses random weights to communicate the error back. With this, weight change is proportinal to an accumulated product of pre- to the (derivative of) post-activity for eacn synapse (eligibility traces!). If only Wout are trained - learning is worse. If only Wrec - completely unsuccessful (not surprising as they use random error instead of flipped Wout). Use "ready-set-go" (interval matching) as a test. Then stiches an architecture of cortex-basalganglia-thalamus, to learn "behavioral syllabi" for a longer behavior.

# To-read

Rodan, A., & Tino, P. (2010). Minimum complexity echo state network. IEEE transactions on neural networks, 22(1), 131-144.
https://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.739.7993&rep=rep1&type=pdf

Carroll, T. L. (2020). Do Reservoir Computers Work Best at the Edge of Chaos?. arXiv preprint arXiv:2012.01409.
https://arxiv.org/pdf/2012.01409.pdf
Started here: [[Carroll2020chaos]]

Tanaka, G., Yamane, T., Héroux, J. B., Nakane, R., Kanazawa, N., Takeda, S., ... & Hirose, A. (2019). Recent advances in physical reservoir computing: A review. Neural Networks, 115, 100-123.
https://www.sciencedirect.com/science/article/pii/S0893608019300784

Ozturk, M. C., Xu, D., & Príncipe, J. C. (2007). Analysis and design of echo state networks. Neural computation, 19(1), 111-138.
http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.877.6880&rep=rep1&type=pdf

Carroll, T. L. (2020). Dimension of reservoir computers. Chaos: An Interdisciplinary Journal of Nonlinear Science, 30(1), 013102.
https://arxiv.org/pdf/1912.06472.pdf
Attempts to actually measure the dimensionality of signals in a ESN.

Ferreira, A. A., & Ludermir, T. B. (2009, June). Genetic algorithm for reservoir computing optimization. In 2009 International Joint Conference on Neural Networks (pp. 811-815). IEEE.

Zhong, S., Xie, X., Lin, L., & Wang, F. (2017). Genetic algorithm optimized double-reservoir echo state network for multi-regime time series prediction. Neurocomputing, 238, 191-204.

Wang, X., Jin, Y., & Hao, K. (2019). Evolving Local Plasticity Rules for Synergistic Learning in Echo State Networks. IEEE Transactions on Neural Networks and Learning Systems, 31(4), 1363-1374.
(no free text online)
It seems that they are really trying to evolve some biologically-inspired rules, inspired by Hebbian plasticity and synaptic interference.

Scardapane, S., & Uncini, A. (2017). Semi-supervised echo state networks for audio classification. Cognitive Computation, 9(1), 125-135.

Feldman, D. P., & Crutchfield, J. P. (1998). Measures of statistical complexity: Why?. Physics Letters-Section A, 238(4), 244-252.
http://www.disca.upv.es/pabmitor/FILES/Complexity/971111_GEN_4.pdf
On Crutchfield complexity measure (that peaks for meaningful networks)

Carroll, T. L. (2020). Path length statistics in reservoir computers. Chaos: An Interdisciplinary Journal of Nonlinear Science, 30(8), 083130.
https://www.researchgate.net/profile/T_Carroll/publication/343653866_Path_Length_Statistics_in_Reservoir_Computers/links/5f3680c8458515b7291f35ec/Path-Length-Statistics-in-Reservoir-Computers.pdf

Rodan, A., & Tino, P. (2010). Minimum complexity echo state network. IEEE transactions on neural networks, 22 (1), 131-144.

Huang, R., Li, Z., & Cao, B. (2020). A Soft Sensor Approach Based on an Echo State Network Optimized by Improved Genetic Algorithm. Sensors, 20(17), 5000.

Lymburn, T., Khor, A., Stemler, T., Corrêa, D. C., Small, M., & Jüngling, T. (2019). Consistency in echo-state networks. Chaos: An Interdisciplinary Journal of Nonlinear Science, 29(2), 023118.
https://arxiv.org/pdf/1901.07729.pdf
Also try to drive a network towards better performance.

Haluszczynski, A., Aumeier, J., Herteux, J., & Räth, C. (2020). Reducing network size and improving prediction stability of reservoir computing. Chaos: An Interdisciplinary Journal of Nonlinear Science, 30(6), 063136.
https://arxiv.org/pdf/2003.03178.pdf

Manjunath, G. (2020). Memory-Loss is Fundamental for Stability and Distinguishes the Echo State Property Threshold in Reservoir Computing & Beyond._arXiv preprint arXiv:2001.00766_.
[https://arxiv.org/pdf/2001.00766.pdf](https://arxiv.org/pdf/2001.00766.pdf)
Looks very relevant!!

Rodan, A. (2012). Architectural designs of Echo State Network (Doctoral dissertation, University of Birmingham). PhD thesis
https://etheses.bham.ac.uk/id/eprint/3610/1/Alrodan12PhD.pdf
He totally considered fixed weights here, and shows that it doesn't hurt. Calls it Simple Cycle Reservoir (SCR). Looks at simple architectures.

Cui, H., Liu, X., & Li, L. (2012). The architecture of dynamic reservoir in the echo state network. Chaos: An Interdisciplinary Journal of Nonlinear Science, 22(3), 033127.
(No obvious access online. But I'm not very hopeful about this one: it seems that they just tried small-world and sale-free networks, and described how ESN worked in them)

Mutual Information and the Edge of Chaos in Reservoir Computers
https://arxiv.org/pdf/1906.03186.pdf

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

Lukoševičius, M., & Uselis, A. (2019, September). Efficient Cross-Validation of Echo State Networks. In_International Conference on Artificial Neural Networks_(pp. 121-133). Springer, Cham.

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