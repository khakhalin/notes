# On Markov blankets and hierarchical self-organisation

Palacios, E. R., Razi, A., Parr, T., Kirchhoff, M., & Friston, K. (2019). On Markov blankets and hierarchical self-organisation. Journal of Theoretical Biology, 110089.
https://www.sciencedirect.com/science/article/pii/S0022519319304588

Elsevier, behind paywall, no preprint.

#self-organization #freeenergy

Biology = spontaneous pattern formation. Markov blanket = separate of internal and external states. All linked to free energy minimization. 

Some background info from Wikipedia:
* "The free energy principle is that systems defined by their enclosure in a Markov blanket try to minimize the difference between their model of the world and their perception. This difference can be described as "surprise" and is minimized by continuous correction of the world model of the system." ([ref](https://en.wikipedia.org/wiki/Free_energy_principle))
* Ibid: In a 2018 interview, Friston acknowledged that the free energy principle is not properly falsifiable.
* The Markov blanket for a node A in a Bayesian network, denoted by MB(A), is the set of nodes composed of A's parents, A's children, and A's children's other parents. ([ref](https://en.wikipedia.org/wiki/Markov_blanket))
* The objective is to maximize model evidence p(sensory | model) or minimize surprise -log p(sensory | model)

> Wait, but would not curiosity maximize that? Isn't the point of curiosity to seek discrepancies with the model?

"A Markov blanket is a set of states that separates the internal or intrinsic states of a structure from extrinsic or external states."

Introduce a super-clunky term "hierarchal compositions of Markov blankets of Markov blankets" to describe hierarchical "configurations of configurations" - like, really?

Distinction between "Variational free energy" (that they use) and "Thermodynamic free energy" - without definitions, and claim that related anyways.

"Shannon entropy of sensory states is necessarily bounded". System bahaves "as if it is gathering evidence for its own existence" (I have no idea what it means tho)

Introduces some clunky formalism, and claims that to counter-act natural dissipation, live systems perform "gradient ascent on the ergodic density over the internal states and their Markov blanket". Define "Variational energy".

OK, at this point I give up. They follow with some more formulas, then with simulations in which blobs, themselves made of blobs of nodes, apparently emerge from these formulas. I'm not sure what it means and how it helps. At the very least, this needs to be restated in a more straightfoward way (Bayesian? Information theory? Representations and loss?) Otherwise it's completely unrelatable.