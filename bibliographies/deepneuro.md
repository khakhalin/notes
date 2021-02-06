# Deep learning and Neuroscience

#deepneuro #dl #neuro

Parents: [[12_Neuro]], [[06_DL]]
Related: [[network_neuro]]

Subplotics:
* [[stdp]] - unsupervised rules in the brain
* [[credit]] - credit assignment in the brain, and backprop in the brain

# Papers

Reviews
* [[Hassabis2017neuroai]]- neuro-inspired AI; past and future; 3-4 refs for every topic

#vet

Gao P, Ganguli S. (2015) On simplicity and complexity in the brave new world of large-scale neuroscience Current Opinion in Neurobiology
Very interesting review! What does it mean to "understand" how the brain works? We cannot even understand deep learning; lack ability to make predictions without simulations. They summarize their own research on "how many neurons from a set of N neurons you need to infer K-dim encoding, assuming a certain level of noise). Claim to prove and demonstrate that the number is small ~ K^2, or K^2/P if you have access to P independent behavioral states (?). Then summarize "evolvability": of all possible somethings (protein networks, proteins) that share a property only those can be realised that form clusters, as they are "evolvable", and robust to perturbations. Links to liquid state machines, and general properties of network encoding that is required for network functionality.

Marblestone, A. H., Wayne, G., & Kording, K. P. (2016). Toward an integration of deep learning and neuroscience. Frontiers in computational neuroscience, 10, 94.
Intro: great summary on ML getting smarter, and more brain-like (non-trivial cost functions, cool architectures, including adversarial). Different cost functions at different regions / parts of the network. A combo of local cost function (~Hebbian) and global signal can emulate backprop? Refs. On biologically plausible backprop. Interesting references on STDP and how it can simulate backprop. Good summary of how one can use modeling to figure out the internal logic of development (what are the cost functions, what is optimized). Good summary on the structure of the world (sparseness of objects, their continuity etc. - an assumption of continuity can be a part of a cost function!) Interesting thoughts and logic about simple detectors (say, for faces) bootstrapping info for second-level fancier detectors. Good refs on dendritic processing and neurons being more than a node in a graph. Overall, good for teaching, except that they don't need to read the middle part (way too detailed). Main difference brain-ourAI: very limited learning datasets / explicit reinforcement.

Linderman, S. W., & Gershman, S. J. (2017). Using computational theory to constrain statistical models of neural data. Current opinion in neurobiology, 46, 14-24.
Yet another paper to compare neuro to physics. We shouldn't be agnostic when analyzing data, as there are some general principles we could use. One can fit data to a specific model, or family of models, or compare several alternative models in how they fit the data. Fitting markov models to a behavior falls into the same category. Essentially claim that it's better to fit a bad mechanistic model than a good phenomenological one. Then show an example about recording from neurons after dopamine-mediated learning, with some technical references for probabilistic search techniques.

Wang, P., & Cottrell, G. W. (2017). Central and peripheral vision for scene recognition: a neurocomputational modeling exploration. Journal of vision, 17(4), 9-9.
Use an artificial network to reproduce some key results from human psychophysiology, and show that computationally they actually make sense. (We're talking about relative influence of fine central and coarse peripheral vision for natural scene recognition).

Ullman, S. (2019). Using neuroscience to develop artificial intelligence. Science, 363(6428), 692-693.
Opinion piece on how the next two big steps in Neuro helping AI will be in network architecture, and also pre-conceived domain-specific knowledge (either as a form of soemwaht hard-wired architecture, or as starting conditions for learning).

Barrett, D. G., Morcos, A. S., & Macke, J. H. (2019). Analyzing biological and artificial neural networks: challenges with opportunities for synergy?. Current opinion in neurobiology, 55, 55-64.
Summary of co-evolvement of ML and Neuro. Cool summary of ablation analysis (how critical individual neurons are, in deep nets). Reference how stimulus selectivity is not predictive of unit importance! (response to ablation). Network pruning as an area of research, with some interesting references. Dimensionality reduction (representation) in DN: ~85% of nodes can be removed without effect on the output. Then a summary of representation comparisons across networks. Canonical Correlation Analysis.