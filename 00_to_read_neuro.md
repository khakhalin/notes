# To-read: Neuro

## Network structure

**Todo**: mine Porter2019 for links (they are mentioned in the paper card, but now need to be transfered here)

Relating network connectivity to dynamics: opportunities and challenges for theoretical neuroscience Carina Curto, Katherine Morrison
https://www.sciencedirect.com/science/article/pii/S0959438819300443 
Review. Exactly what it should be (networks, motifs, dynamics)

## Criticality and dynamics

25 years of criticality in neuroscience — established results, open controversies, novel concepts J Wilting, V Priesemann
[https://www.sciencedirect.com/science/article/pii/S0959438819300248]
Very short, so a must read.

Cortical computations via metastable activity Giancarlo La Camera, Alfredo Fontanini, Luca Mazzucato
 https://www.sciencedirect.com/science/article/pii/S0959438818302198 
Interesting; includes a section of spiking models of those. It's like criticality, but better, and also super-relevant.

Vogels, T. P., Sprekeler, H., Zenke, F., Clopath, C., & Gerstner, W. (2011). Inhibitory plasticity balances excitation and inhibition in sensory pathways and memory networks. *Science*, *334*(6062), 1569-1573. 
About what tunes inhibitory neurons, that in turn make excitatory activity properly sparse.

Universality and individuality in neural dynamics across large populations of recurrent networks Niru Maheswaranathan, Alex H. Williams, Matthew D. Golub, Surya Ganguli, David Sussillo
 https://arxiv.org/abs/1907.08549 
 
## Reservoir computing

Pogodin, R., Corneil, D., Seeholzer, A., Heng, J., & Gerstner, W. (2019). Working memory facilitates reward-modulated Hebbian learning in recurrent neural networks. arXiv preprint arXiv:1910.10559. 
 https://arxiv.org/pdf/1910.10559.pdf 
Reservoir computer + a "working memory network"

## Credit assignment in bio networks

Lehmann, M. P., Xu, H. A., Liakoni, V., Herzog, M. H., Gerstner, W., & Preuschoff, K. (2019). One-shot learning and behavioral eligibility traces in sequential decision making. eLife, 8, e47463.
https://elifesciences.org/articles/47463

Aljadeff, J., D'amour, J., Field, R. E., Froemke, R. C., & Clopath, C. (2019). Cortical credit assignment by Hebbian, neuromodulatory and inhibitory plasticity. arXiv preprint arXiv:1911.00307.
[https://arxiv.org/pdf/1911.00307.pdf](<https://arxiv.org/pdf/1911.00307.pdf>)

## Bottom-up validation

Lim, S. et al. Inferring learning rules from distributions of fring rates in cortical neurons. Nat. Neurosci. 18, 1804–1810 (2015).

## Other

Harnessing behavioral diversity to understand neural computations for cognition Simon Musall, Anne E Urai, David Sussillo, Anne K Churchland
[https://www.sciencedirect.com/science/article/pii/S0959438819300285]
Potentially the most important section: "Relating rich behavior to neural activity by studying ANNs"

Raman, D. V., Rotondo, A. P., & O’Leary, T. (2019). Fundamental bounds on learning performance in neural circuits. Proceedings of the National Academy of Sciences, 116(21), 10537-10546.

Learning predictive structure without a teacher: decision strategies and brain routes Zoe Kourtzi, Andrew E Welchman
https://www.sciencedirect.com/science/article/pii/S0959438818302393 

The language of the brain: real-world neural population codes J Andrew Pruszynski, Joel Zylberberg
https://www.sciencedirect.com/science/article/pii/S0959438818302137 
Mostly (only?) about motor cortex, and how representation works there. Seems useful and accessible.

Data-driven models in human neuroscience and neuroengineering Bingni W. Brunton, Michael Beyeler. 
A review of current ML solutions for this general problem; still may be useful.

An argument for hyperbolic geometry in neural circuits Tatyana O Sharpee
Interesting, but not immediately clear. But intriguing. Not much math, so should be very readable.

Mind the last spike — firing rate models for mesoscopic populations of spiking neurons Tilo Schwalger, Anton V Chizhov
 https://www.sciencedirect.com/science/article/pii/S095943881930039X 
Review

https://www.biorxiv.org/content/10.1101/838383v1
Training deep neural density estimators to identify mechanistic models of neural dynamics
Gonçalves .. Macke 2019
About how to use deep learning to guess neuronal parameters to fit the actual activity of the network (?) They seem to be looking at actual V(t) though.

https://www.biorxiv.org/content/10.1101/837567v1 
Interrogating theoretical models of neural computation with deep inference
Bittner .. Cunningham 2019
Apparently a very similar paper (to Goncalves 2019), but independent.

https://www.biorxiv.org/content/10.1101/798553v1 
Recurrent neural network models of multi-area computation underlying decision-making Michael Kleinman, Chandramouli Chandrasekaran, Jonathan C Kao
It seems that they try recurrent networks (RNN) running in parallel, and compare their decision times to that from monkey cortex. Then show that one RNN doesn't match the distribution of times, but if you have several running in parallel, and then synthesizing info, you get similar results? Claim that different modes of distributed computation are experimentally testable like that.

Balleine, B. W. (2019). The Meaning of Behavior: Discriminating Reflex and Volition in the Brain. *Neuron*, *104*(1), 47-62. 
Review. Potentially an interesting paper on free will, behaviorally (and computationally?) defined.

https://www.frontiersin.org/articles/10.3389/fnsys.2019.00042/full 
The Dialectics of Free Energy Minimization Evert A. Boonstra and Heleen A. Slagter
Seems to be a review explaining the free energy minimization principle for an organism. It's not math though; looks almost like philosophy?

Classes of dendritic information processing Alexandre Payeur, Jean-Claude Béïque, Richard Naud

The quest for interpretable models of neural population activity Matthew R Whiteway, Daniel A Butts

Constraining computational models using electron microscopy wiring diagrams Ashok Litwin-Kumar, Srinivas C Turaga

## Reverse modeling

Deep neuroethology of a virtual rodent
Josh Merel, Diego Aldarondo, Jesse Marshall, Yuval Tassa, Greg Wayne, Bence Ölveczky
https://arxiv.org/abs/1911.09451
Apparently create a vidrual 3D rodent (like, with muscles, joints and what not), make it move in virtual environment, learn to move, then study its network using neuro methods.

Li, Z., Brendel, W., Walker, E., Cobos, E., Muhammad, T., Reimer, J., ... & Tolias, A. (2019). Learning from brains how to regularize machines. In Advances in Neural Information Processing Systems (pp. 9525-9535).
https://arxiv.org/abs/1911.05072