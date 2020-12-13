# Push-pull competition of auditory attention

Huang, N., & Elhilali, M. (2020). Push-pull competition between bottom-up and top-down auditory attention to natural soundscapes. Elife, 9, e52984.

#neuro #attention

Attend to a periodic sound (are supposed to notice changes in tone; in this case, oscillatory modulation of the amplitude instead of a flat beep), all while being distracted by a soundscape (cafe). When loud distractors happen, people make mistakes, and also the brain activity (EEG) becomes less locked with the periodic sound. 

Apparently, auditory bottom-up attention is understudied (at least compared to vision). Which is partially because with vision, one can use natural behavior (eye movements, saccades) to directly measure attention, while with hearing (in humans) we don't have this luxury.

Pupilometry is sometimes used!!

The pulse of the signal is rather fast here (2.6 Hz), so it's not like they are "expecting it"; they are following the rhythm.

Right after a sudden salient effect in the background scene, we can reconstruct the volume (envelope) of the background noise from the EEG better. Which suggests that the attention switches to the scene.

_The very fact that we can reconstruct volume of the scene from the EEG, in the presence of other stimuli, is kinda non-trivial, but OK._

In the non-technical summary they claim that they observed same activation pattern in the brain for both types of attention, which means that they observed an attention shift, even though attending to a periodic sound attention is voluntary (aka top-down), while distraction is caused by the stimulus itself (bottom-up). Fig 4 however seems to show opposite effects on gamma-frequency between targets (voluntary attention) and after salient background events (involuntary). That's what they call "push-pull competition".

"Same activation" applies to the spatial reconstruction of what networks are activated for both types of attention. They call them "Target network" and "Salience network", and they mostly overlap, and interestingly aren't only in the temporal, but also in the parietal associative cortices.

To quantify "attention switching", they then are trying to predict (in the ML sense) the presence of salient stimuli from the EEG, using a neural net. But not directly: they feed features to the net (gamma, tone-locking), not the signal itself (which is a pity). Only achieve ~73%. What counted as a "salient distractor" seems to come from their own previous studies where people listened to 2 recordings at once, in each ear, and needed to report which one they are attending to. Those moments when they switched (derivative of average ear) are distractors.