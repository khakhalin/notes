# Engineering a less artificial intelligence

Sinz, F. H., Pitkow, X., Reimer, J., Bethge, M., & Tolias, A. S. (2019). Engineering a less artificial intelligence. Neuron, 103(6), 967-979. http://xaqlab.com/wp-content/uploads/2019/09/LessArtificialIntelligence.pdf

Parent: [[deepneuro]], 
See also: [[Zador2019pure]], [[vision]], [[convnet]], [[archsearch]]

#deepneuro


> Something that feels almost like a Bayesian reasoning, at least in spirit. Every architecture has an "inductive bias" towards finding particular types of solutions, sorta of a prior. Our brains run on networks that have useful biases, while DL systems often seem either Tabula Rasa-y, or incorporate inductive biases that aren't necessarily helpful.

**Moravec’s paradox** (Moravec, 1988): computers are good in learning things that adult humans learn to do, but are really bad in things that we learn as kids (infants, even).

Describe how in [[vision]] domain ANNs have a **texture bias**: changing a pattern (texture) of an image (e.g. using style transfer) typically fools classifiers, while humans go by shape, not by texture. Texture scramble hurts human performance, but not ANN performance, while the opposite is true for abstract shapes (humans have a **shape bias**).

In general, ANNs are sussceptible to **minimal adversarial perturbations**, while doing something like that to humans is much harder (under normal viewing conditions). Ref: Elsayed et al., 2018. It seems that we (humans) integrate context much stronger. Because of that, it's hard to train networks against adversarial attacks. You may do it explicitly, if you anticipate the nature of the attack, but it's obviously not a good method in the long-term, as it doesn't automatically generalize.

At the same time (maybe for related reasons, but maybe not) humans are much better in fighting **distortions** like snowflakes and rain (patterns and local loss breaking an image). It is relatively easy to train an ANN network explicitly to fight a certain type of distortion, but there's no transfer to other types of distortion (while in humans there's obviously transfer). And there's a hint that the amount of training data available to humans is much lower (we don't spend our childhoods looking through the snow or leaves), while for ANN to perform, we kinda have to raise it in this weird distorted environment.

Quite **bad in non-local integration**. While in [[convnet]]s local information is ultimately integrated globally, it is all ventral path, a **bag of features** (a network typically "knows" that there was a puppy, but doesn't "remember" the position of this puppy too well).

> Didn't recent synthesis experiments with CLIP have a similar problem with extreme localization? Or was it specific to CLIP algorithm in particular?

Their solution: **inductive bias** or **model bias**; a type of meta-regularization, where you **constrain the space of solutions** for a network. And, they think, looking at the brain may help, as by making models "brain-like" we may inadvertently make them smarter, just because brain-likeness may correspond to some cool evolutionary findings. Two ways to constrain the network through the data, and arguably model how actual bio brains work:
* **Data augmentation** (see [[augmentation]])
* **Multi-task learning**: training same network on different tasks at the same time (essentially, **weights sharing**?)

Nice list of features in which ANNs are already brain-inspired:
* the obvious "neurons-weights-nonlinearity" architecture itself
* normalization
* winner-takes-all (essentially, maxpooling - see [[pooling]])
* attention
* dropout (_except they totally don't explain, in what way dropout is neuro-inspired. Is it?_)

But neurons are vastly more complex (see [[dendritic_comp]]). _Although as usual I'm not sure how this is relevant, as every non-linear neuron can be represented by an ANN, so all it means is that ANN neurons aren't one-to-one with bio neurons, but more with compartments. Which is important from the architecture point of view, of course, but not a game-changer._

Another (_and way more important in my view!!_) is that all bio brains are inherently recurrent, while modern ANN are not.

At the same time, there's no guarantee that just approximating bio elements will productively bias the networks. It's quite possible that we'll just get an inefficient network with similar patterns as a "vanilla" ANN. They give a good example here: ResNet differs from "classic" ANNs in a way that seems trivial (skip-connections), and mathematically, its expressiveness shouldn't be different from that of a "normal" ANN. And yet it learns better. Which suggests that some architectural changes are actually about learning dynamics, and not about expressiveness (and also that we cannot predict things like that at this point).