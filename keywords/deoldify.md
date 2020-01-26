# Deoldify: auto-colorizing old photos
https://github.com/jantic/DeOldify

Free online version: 
https://deepai.org/machine-learning-model/colorizer

#self-supervised #aisociety #gi

Colorization of old photos is not just a great example of self-supervized learning (see [[Weng2019self-supervised]]), but also a great test for **generalized intelligence**, separate from text. Which is nice.

For example, the current model (as of Jan 2019) always colors US flags right (as they have a recognizable pattern that it just learned). Some less common flags it fails to colorize (say, the Chinese Revolutionary Army, which was red with a little blue sun). But this is still a simple case of learning by correlation. Say, it seems (not sure, but it seems) to colorize flags with a Hammer-and-Sickle red (or at least reddish), suggesting that maybe it had seen them.

But show it a picture from some regional event in Soviet Russia, with Cyrillics, a portrait of Stalin and some flags, and it totally fails to color the flags red. Because this requires contextualization and generalization. A true understanding would still start with correlations, like clothes and photographic artifacts → era, letters → language → region. Even these are out of grasp of modern networks, as modern vision-procesing networks cannot really read, and also don't have access to everything learned by GPT2, for example. And even GPT2 only learned in one language, I think, didn't it? But even then, the final step would be a leap of faith, by guessing (Bayesian-style?) that this strage blurry photo from the 50s from somewhere in Tatarstan is likely too be Soviet, and thus the flags have to be colored red.

A sketch of a list that a model should be able to do to solve this puzzle correctly:
* Integrate across domains, such as images and text
* Be able to develop a hidden representation of image styles (retouche, objective type), integrate it with clues of time (clothes, postures, makeup, hair styles, car models etc) and region (on top of everything just listed, also vegetation, weather, inscriptions, cultural signs etc.)
* Maybe: somehow cross-reference it with historical information (learned by a history model), although this part is both ridiculously hard (how can you learn that a samovar is called a samovar?), and may not be necessary, except for most esoteric tasks
* Make cross-domain guesses: e.g. from knowing (in a statistical fashion) that Soviet photos from the 80s in Moscow had red everything, and from knowing (from texts?) that there was a cultural continuity for quite a few decades, infer that a much older photo from a different Soviet region should also be colored red.

Curiously, current DeOldify actually does a decent job with large-scale parades:
https://img-fotki.yandex.ru/get/15552/174326890.8b/0_f7b39_fabc4648_-1-XL.jpg
Which suggests that SOME Soviet photos made it into the training dataset.

Another intersting bias: how all Old Believers in kaftans in my photos were colored blue.

A consequence of that: if this sort of technology becomes widespread, it may shift our perception of the past. Instead of imagining that WW1 was BW, everybody will imagine uniforms and other clothes blue or violet, even if actually they were green, brown, or black. Kinda like what happened with Greek statues that we imagine white, even though they allegedly were all painted in bright colors (were they?)