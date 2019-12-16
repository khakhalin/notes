# One-shot learning and behavioral eligibility traces in sequential decision making

Lehmann, M. P., Xu, H. A., Liakoni, V., Herzog, M. H., Gerstner, W., & Preuschoff, K. (2019). One-shot learning and behavioral eligibility traces in sequential decision making. eLife, 8.
https://elifesciences.org/articles/47463

#neuro #behav #rl

Claim that they can differentiate between eligibility traces (RL rewards entire sequence) and "classic learning" (aka "Temporal-Difference algorithms") that slowly propagate the reward backwards through the sequence) behaviorally, and that humans do the one-shot thing.

Start with a summary of RL models in humans (refs). Popular modern models contain a hidden memory about past states, aka "eligibility trace" (lots of refs, for both humans and computational models). Multi-step learning with delayed feedback allows a comparison based on qualitative data. Claim that without eligibility traces after 1 shot only the latest step woudl be reinforced, and it would take at least 2 shots to "propagate" (not the word they used) back to earlier steps.

Then test it on something like Markov sequences, except the first one is fake (the person is told that they guessed the sequence, but actually the choices they make define a 2-step sequence). And they make humans learn several (6?) schemas like that at once. They then check how they work in the future, and notice that they remember the 1st step, not only the 2nd one. Both behaviorally, and in terms of pupil dilation. Which is probably a more interesting result. About 20-50 episodes per participant; three types of stimuli (clipart, sounds, spatial), 12-22 participants in RL experiments, 9-12 in control (observe, but no RL). Interestingly, control was only added after reviewers required it :)

Then they claim that they can estimate eligibility trace decay from their data. Then they try different RL algorithms (8 in total): 4 with eligibility trace: Q-λ, Reinforce, 3-step-Q, SARSA-λ, and then 4 without it: Hybrid, Forward Learner, Q-0, SARSA-0, Biased Random. Claim that eligibility trace worked better (log-likelihood fit? I haven't read the methods too closely), and then the λ is around 0.7, 0.8, or 0.96 for diff stimuli. But these are some unpleasant values, related to how the signal decays from one presentation to another. So they transcribed it into time (mapping it to their specific experiment I guess), and got τ (decay in time) or around 10 s, corresponding to 2-3 inter-stimulus intervals. 

Apparently, no changes in control. The sample size was smaller, but the effect size seems much larger in RL session, so it's probably real (they don't report effect sizes tho, only p-values).

In the discussion say that 10 s is a cool value, as it's not that off than dopamine modulated plasticity in the striatum, or HT5/NE plasticity in the cortex (refs). Notably shorter than minutes reported in the hippocampus.