# Deep Networks as Scientific Models

Deep Neural Networks as Scientific Models (2020). Radoslaw M. Cichy, Daniel Kaiser. Trends in Cognitive Sciences.
https://www.cell.com/trends/cognitive-sciences/fulltext/S1364-6613(19)30034-8
Seem to suggest that for some types of tasks data it may be easier to model a process with a DL and interpret a DL, instead of directly interpreting the data?

Parent: [[deepneuro]]
Related topics: [[interpretability]]
Related papers: [[Richards2019deepneuro]], [[Lillicrap2019understand]]

#deepneuro #modeling


What makes a good model? **Precision** (does it predict stuff?), **Realism** ("is it possible?", _maybe with a dash of "do we understand it?", as I guess one cannot assess realism without having enough understanding of how reality can project on a model_), and **Generality** (does it transfer to other, neighboring situations?). And these probably form some sort of trade-off system. Which implies that there is rarely "one best model", as the relative weight of desirables depends on the situation, and also as the phenomena we are trying to understand are hierarchically diverse (even for the same task, species, but also individuals). Also, there are non-theoretical desirables in play, such as cost, and ethics (of animal experimentation).

> They use a weird Latin word "disiderata" instead of a normal-ish English "desirables" :)

Practical uses of models in Neuro:
* Prediction - in prosthetics, experimental design, benchmarking (vetting models before other applications)
* Explanation - mechanistic; teleological (in terms of purpose)
* Exploration -  a still opaque, but easy  to manipulate "conceptual copy" of reality

Typical arguments against ANNs as a model of Brains:
* Explanation (don't match brains that well)
* Interpretation (opaque, black-box-y)
* Biological realism
* Scientific validity (we sorta still don't know why ANNs even work lol	)

Comp Neuro and CogSci use of ANNs is a bit like kitbashing in art (they call it ready-mades, which is similar, just older and more large-scale). A situation similar to "hardware lottery" (see my notes at the end of [[Richards2019deepneuro]]).

Describe a fantastic scenario of "non-invasive brain activity control" that relies on predictive ANNs as a tool.

The funny part about ANNs as a model is their **opaqueness**; the lack of [[interpretability]]. Moreover, it needs to be learned, not just "build" (see [[Lillicrap2019understand]]).

What do people usually mean by "understanding"?
* Ability to **deduce** from simple principles
* Probabilistic match between the behavior of a model, and of the object (**inductive**)
* Discovery of **causal** processes in the system (_do them mean a door to an intervention, experiment, control?_)
* **Unification** - unite several different phenomena under one explanation umbrella
* **Pragmatic** - if useful in any way, that's enough

From this pov ANNs are inherently teleological, and bias us towards pragmatism, as they are not about how something works, but about what goal it tries to achieve (and then, about the precise process of finding a system that achieves this goal). So in a way, for a cognitive person, ANN (as models) are closer to how experimental animals can be models, than to what people call a "conceptual model" (a lossy but intuitive idea).

> That said, ANNs did change the paradigm of how people thought about information processing. The ideas of representation, bottleneck, tickets and probabilistic convergence, learning rules and hyperparameters (the process of finding a solution) as more defining features of a system than its precise final state; a better understanding of relative merits of different error signals… It's quite possible that ANNs don't yet feel like "conceptual models" just because the concept of a "conceptual model" was formed by scientists that didn't yet work with ANNs (or modern scientists when they were younger :) As quantum mechanics or chaos needed 20-30 years (at least!) to be normalized, maybe ANNs wont' feel that bizarrely opaque just because ppl will get used to it?

Then a whole box on how models shape our perception of reality, of what counts as an explanation, of what's possible and impossible, and even social reality.

Interesting point: ANNs as **proof of principle demonstration**.