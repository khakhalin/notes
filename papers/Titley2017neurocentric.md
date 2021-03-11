# Toward a neurocentric view of learning

Titley, H. K., Brunel, N., & Hansel, C. (2017). Toward a neurocentric view of learning. Neuron, 95(1), 19-32.

Parents: [[dendritic_comp]], [[intrinsic_plasticity]]

#neuro #review #dendritic #intrinsic

A motto / research program of Hansel lab, U Chicago.

tldr: LTP is important for learning, maybe it is changes of membrane excitability (**intrinsic plasticity**) that actually form the engram. From synaptocentric - to neurocentric (kinda neurono-centric).

Key limitations of NMDA-dependent LTP and LTD:
* it is rarely single-trial, while learning is often single-trial.
* Moreover, in mice, freezing in the learned environment happens even in mice treated with anisomycin, that cannot synthesize proteins, and where long-term phases of LTP in the amygdala are supposedly blocked (Ryan et al., 2015).

Combined by the fact that learning is engram-mediated (which was tested with optogenetics), it suggests that the engram formation doesn't rely on LTP entirely.

Introduce a term: **synaptic penetrance**, which is an influence of a strength of a certain synapse on the probability of spiking in the postsynaptic cell.

A nice summary table of key studies in intrinsic plasticity changes in behavioral experiments (learning-induced, not just stimulation-induced).

BDNF is often involved. Changes in voltage- and calcium-dependent ion channels: BK (aka large-conductance calcium-dependent), SK (small-conductance), HCN (hyperpolarization-activated cyclic nucleotide gated channels). Change the AP threshold, and the duration of a burst (in Purkinje cells). AHP currents (?), A-type K channels (?).

In the cortex (mostly studied in the barrel cortex): sparse coding, low synaptic penetrance, non-linear dendritic integration. Different cortical regions have different coding characteristics tho: say, visual cortex has higher response rates, (less sparse coding?) compared to the barrel cortex (?).

As a result, one neuron can have very specific, highly tuned response profiles (Jennifer Aniston neuron, aka "grandma neuron", but better).

**Theoretical models**: Classic neuron (Hebbian; Oja rule) can do linear regression over inputs in an unsupervised manner. Cite from (Clopath et al., 2012) how perceptron fails. - _overall, not a very clear summary, this part._

Program for the future: try to isolate the part of learning that can be done without LTP/LTD.

### From the discussion in Silas' research proposal:

Quite often Purkinje cells have 2 branches, sometimes even sticking like horns from the soma, which seems almost as if they were doing two different things at the same time. Are calculations compartmentalized? Because PJC dendrites are actually actively pruned during development, and so do climbing fibers (CFs), with having 1 fiber per cell eventually. But cells with 2 part-trees are innervated by 2 CFs (about 20% of cells)! According to Silas' preliminary data, only 2-CF PCJs have "horned dendrite" morphology! Which means that the shape of PJC dendrites is really defined by its function actually!!!

If you record events in different branches of a PCJ, sometimes events are linear (and observed in all branches), but sometimes (within the same cell!!) they are strongly branch-specific. Seems dependent on CF activation! (Especially for multi-CF cells)