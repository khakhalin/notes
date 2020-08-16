#  Postsynaptic and Presynaptic NMDARs have opposite effects on cell morphology

#neuro

Kesner, P., Schohl, A., Warren, E. C., Ma, F., & Ruthazer, E. S. (2020). Postsynaptic and Presynaptic NMDARs Have Distinct Roles in Visual Circuit Development. Cell Reports, 32(4), 107955.
https://www.sciencedirect.com/science/article/pii/S2211124720309360

NMDA on pre- and post-synaptic neurons affect morphology in opposite ways: dendritics trees are driven to become less complex, while axonal arbor becomes more complex. They know it because they observed the opposite in experiments with KD with antisense morpholinos (in NMDA KD neurons, dendritic trees are more fancy, while axonal arbors are less fancy).

Interpretation: 
* Dendrites are hungry for information, so without NMDA input, they seek information sources, becoming content only once this source is found. For dendrites, NMDA = good, no NMDA = seek.
* With axons situation is the opposite: NMDA activation means spillover, immature synapses, and a need for a better connection to someone who will actually appreciate your glutamate. So for axons, NMDA = seek, no NMDA = good.  

ML interpretation: two types of normalization. Postsynaptic NMDA here act as weight normalization for input neurons (keeping Σw_ij for each j more constant), while presynaptic NMDA do weight normalization for pre-synaptic neurons.

> This all is true only if we assume that spatial spillover from neighboring connections doesn't play a role. If it does, I'm not sure what all of this means.

Methods: Xenopus tectum, DK via injections at 2-cell stage, that makes 2 sides of the body different.