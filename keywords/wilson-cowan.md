# Wilson-Cowan model

#neuro


Parent: [[neuro]]
Related: [[complexity]], [[cpg]], [[sleep]]

A simplified and very old (1972) model of a mix of excitatory and inhibitory neurons that demonstrates bistability. Basically 2 differential equations on variables E and I (all excitatory and all inhibitory), that is kinda inspired by neuroscience. E population excites both itself and I, while I population inhibits both itself and E. if you solve the system, it converges on either one or another constant level (after a short oscillation). Has a hysteresis to it, and can be turned into an oscillatory model with some small modifications (apparently). Can be analyzed using classical phase portraits.

# UP-DOWN cortical states

Many neurons in the cortex switch between two "preferred" baseline membrane potential states, called up and down. The best and most obvious way to detect the presence of two distinct states from an intracellular recordings is to build a histogram of Vm, and see that it's bimodal. Both Vm are subthreshold, but UP is elevated above the true resting by some subthreshold activity, and is closer to the threshold, so most spiking occurs from the UP sate.

DOWN state is not only a state of lower baseline Vm, but also a state of suppressed cortical activity. And in colloquial use, obviously, most people use words UP and DOWN to put labels on the current situation with local spiking in the cortex, not with Vm, as more people record fro the cortex extracellularly, or at least think about cortex in terms of population activity.

Apparenlty, K-complexes in N2 non-REM [[sleep]] are DOWN states. They follow sensory stimulation, and it almost seems like they prevent cortex from getting activated, by triggering this DOWN states (_is it true?_). Similar events happen during emergence from anesthesia (Lewis 2018)

# Refs

http://www.scholarpedia.org/article/Up_and_down_states

https://en.wikipedia.org/wiki/Wilson%E2%80%93Cowan_model

Lewis, L. D., Piantoni, G., Peterfreund, R. A., Eskandar, E. N., Harrell, P. G., Akeju, O., ... & Purdon, P. L. (2018). A transient cortical state with sleep-like sensory responses precedes emergence from general anesthesia in humans. elife, 7, e33250.