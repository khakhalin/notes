# Pulvinar

#vision #neuro

Related: [[vision]],  [[ene_070_vision]]

### Some personal musings

[[interpretability]] work by Olah and Mordvintsev shows that [[convnet]]s have neurons for dogginess, windowness, gearness, etc. But interestingly, humans seem to have pattern-sensitive neurons like that **hardwire** in the pulvinar (for insectness, teeth, holes, slime). How the heck do you genetically encode a pattern like that?

Now, part of this statement above is honestly an extrapolation. What's 100% confirmed is the presence of snake, face, and hand neurons in the pulvinar, both in macaques and humans. Hard-wired, subcortical, directly wired to amygdala, and relying both on shape and pattern. However it seems that like trypophobia-like patterns (holes, insect eggs, scales) activate some areas in humans similarly (strongly and quickly). In humans, spiders and insecty-centipedy patterns also evoke strong  innate responses, but that's not true monkeys (which probably makes research harder, but which is also understandable, as at this level our live histories are pretty different). I would guess that  humans may have spider detectors in the pulvinar, but I don't think it's proven.

In general, I think, most common human phobias would give a good list of candidates (with pre-existing amygdala connections of at least some sort). And also, these seem to be the stimuli infants learn to recotnise and be wary of, before they learn what they even mean. (And of speaking of hard-wired-ness, of course they can overcome it; many cultures eat insects, and there are even cultures that habitually eat spiders - some large spiders in Brazil, I think? But that's not that common)

This paper may be relevant, but I cannot find it online:
* Ren, C., & Tao, Q. (2020). Neural Circuits Underlying Innate Fear. In Neural Circuits of Innate Behaviors (pp. 1-7). Springer, Singapore.

Finally teeth and slime are my hypotheses (candidates for hard-wiredness), mostly based on the fact of how strongly (and early) we respond to both, and on how limited the pattern-vocabulary of horror is. Segments, repetitions, teeth, holes, small moving things, slime. That's it! And it feels so pattern-based, not shape-based! (See works of H.R. Giger, the "Alien" artist, as the most archetypical illustration)

Now a question, are these patterns actually easier to encode genetically, compared to some other, "good" patterns? I really don't have a good intuition here. Why some patterns would be harder than others? Judging from DL experiments, eyeness, flowerness, feathriness etc. are all about the same amount of complex, and develop in the same layer. It's just that some of them mean death, and some of them don't.

But at the same time, it's possible that it's the symmetry / high frequency repetition that is alarming. Leaves are fractal. But ribs, centipedes, teeth, are high-frequency periodical. Insect-y legs are also differently periodical in different spatial directions. Can this be enough? IRL they would also move in a peculiar fashion, but maybe it's enough to have a detector for this sort of periodicity?

Potential research program: 
* First train a basic convnet, with the goal to develop a reasonable first layer or two (early processing). Or take them pre-trained.
* then create a very primitive "hardwired" parametrized layer that looks for local spatial repetition at 2 frequencies, orthogonal to each other (or just at an angle to each other? To enable teeth?) Optimize these 2 parameters to detect threat.
* For this, collect some "horror" pics (just use the word "horror" as a keyword?), use normal images as negative examples.
* Collect some teeth and spiders. Check if horror-trained parametrized neurons react stonger on teeth, spiders, lotus fruits etc.
* If yes, claim that frequency filters like that can be hard-wired through a combination of 2 molecular cues, and maybe some local repulsion (to introduce frequency asymmetry, if it is needed)

Problems here:
* Horror is probably also dark and whatnot
* Bio filters should be rotation-invariant, and at least somewhat scale-invariant Although arguably very small and very large images, like classicist architecture, are not a problem. Only small\patterns are. So maybe being rotation-invariant is enough. But how to become rotation-invariant?