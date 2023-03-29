# Intrinsic plasticity

Parents: [[neuro]]
See also: [[dendritic_comp]]

#neuro #bib #intrinsic


Papers:
* [[Titley2017neurocentric]] - review of how neurono-centric is better than synapto-centric

# Bibliography

Synergies Between Intrinsic and Synaptic Plasticity Mechanisms. Jochen Triesch. 2007, Neural Computation

## Consequences

Roy, A., Osik, J. J., Meschede-Krasa, B., Alford, W., Leman, D. P., & Van Hooser, S. D. (2020). eLife 2020;9:e58509 DOI: 10.7554/eLife.58509
https://elifesciences.org/articles/58509
Show that intrinsic plasticity plays an important role in RF development in the visual cortex of ferrets after their eyes are open.

## Developmental traces

Achard, P., & De Schutter, E. (2006). Complex parameter landscape for a complex neuron model. PLoS computational biology, 2(7), e94. 
Evolutionary model to create 20 "very different" models of Purkinje cells. Same output; no clear continuum in the parameter space, but a set of "loosely connected hyperplanes".

Taylor, A. L., Goaillard, J. M., & Marder, E. (2009). How multiple conductances determine electrophysiological properties in a multicompartment model. Journal of Neuroscience, 29(17), 5573-5586.
They built 600 000 models of same neuron with various distribution of 11 currents across compartments. Selected ~0.2% that resembled actual neurons phenotypically. Did not find correlations observed in real neurons. Also a curious figure (F7) showing how each underlying parameter affected each phenotypical model property. This suggests that correlations observed IRL come from developmental rules, tuning.

Grashow, R., Brookings, T., & Marder, E. (2010). Compensation for variable intrinsic neuronal excitability by circuit-synaptic interactions. Journal of Neuroscience, 30(27), 9145-9156.
Recorded lots of parameters (6) for each neuron. Then used dynamic clamp to include this neuron into a virtual 2-neuron oscillatory network, both modeling the activity of the other neuron, and faking extra currents in this one. Show that for each neuron there's a combination of parameters that made it generate a nice periodical activity with correct frequency. Even though the parameters themselves are very different.

## Mechanisms

Kole, M. H., Letzkus, J. J., & Stuart, G. J. (2007). Axon initial segment Kv1 channels control axonal action potential waveform and synaptic efficacy. Neuron, 55(4), 633-647.
Inactivation of Kv1 in AIS during slow depolarization has uniquely strong effect on spiking. Mostly concentrated in the distal part of AIS. Potential implication: a small number of Kv1 receptors has huge impact on spiking, so it's not a given that we would see a change in transient K currents when spiking has changed. Maybe the bulk didn't change.

Kole MHP, et al. (2008) Action potential generation requires a high sodium channel density in the axon initial segment. Nat Neurosci 11(2):178–186.
AIS has very high density of channels as they are tightly linked to cytoskeleton.

Grubb, M. S., & Burrone, J. (2010). Activity-dependent relocation of the axon initial segment fine-tunes neuronal excitability. Nature, 465(7301), 1070.
AIS shifts depend on the temporal properties of stimulation (dissociated hippocampal culture). Bursts are particularly effective.

Kole, M. H., & Stuart, G. J. (2012). Signal processing in the axon initial segment. Neuron, 73(2), 235-247.
Review, including what channels are there and how. High variability of somatic AP thresholds, but low variability in the AIS (because of the distance).

Hamada, M. S., Goethals, S., de Vries, S. I., Brette, R., & Kole, M. H. (2016). Covariation of axon initial segment location and dendritic tree normalizes the somatic action potential. Proceedings of the National Academy of Sciences, 113(51), 14841-14846. See [[dendritic_comp]]
The larger the dendritic tree - the further is AIS, to enable same basic transfer function despite different somatodendritic capacitance. The further is the AIS, the stronger is the AIS current, to maintain same AP amplitudes in the soma! Double-co-regulation. This second part however may be specific for pyramidal neurons, as they rely on dendritic back-propagation. Also describes how AIS movement may be paradoxical: decreased coupling with the rest of the cell means higher threshold, but also more intense (independent) spiking, not shunted by the rest of the cell (refs).

Evans, M. D., Dumitrescu, A. S., Kruijssen, D. L., Taylor, S. E., & Grubb, M. S. (2015). Rapid modulation of axon initial segment length influences repetitive spike firing. Cell reports, 13(6), 1233-1245.
3 hours of elevated acritivity shortened AIS in DG granular cells, but upregulated voltage-gated sodium channels (through dephosphorylation). When dephosphorylation is blocked, leads to a drastic decrease in repetitive spiking.