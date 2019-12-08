# Limitations of Deep Learning for Vision, and How We Might Fix Them

Alan L. Yuille, Chenxi Liu. 2019. The Gradient
https://thegradient.pub/the-limitations-of-visual-deep-learning-and-how-we-might-fix-them/

#blog

Three waves of deep learning: 1950-60, 80-90, and now (2000-on). Most ideas come from the 90s, but GPUs made all the difference. The 2nd wave went down due to success of support vector machines and kernel methods. But in 2011 AlexNet demolished all competitors in image recognition, and neural nets are the SOTA since. 

Main limitations:
* Very (arguably, unreasonably) data-hungry
* Tend to exploit context (e.g. learn from traces of image origin)
* Biased towards "rare events" and unusual representations even for well-known objects (like, a slightly unsual angle of view can completely throw a deep net off) _- this sounds as the other side of context exploitation: deep nets tend to build "wrong" generalizations, paying attention to semantically "wrong" cues. Lack ability to generalize and transfer properly, which is probably due to the lack of unsupervised statistical "knowledge" about the world._
* Succeptible to adversarial attacks (photoshop a guitar on a monkey; no longer identified as a monkey) _- This sounds like an inability to detect a radical change in data distribution that humans somehow manage to deal with._
* Don't deal with combinatorial explosion well (_and they imply, that is why combining two very different objects is so effective as an adversarial attack - something I'm not sure I agree with_)

Predict that DL is great for tasks that don't have a risk of combinatorial explosion, like medical imaging for example. But bad for IRL tasks, like autonomous driving. Or **IQ tests**: apparently, DL cannot do them! Fun!

### How to overcome combinatorial explosion?

**Compositionality**: basically, a built-in reductionism. The idea that novel stuff can be understood hierarchically, as a sum of its parts.

**Causality**: posit as anotherkey feature that is necessary. _I'm actually not sure about it; or maybe I can agree if causality is understood as another type of relationship, but it doesn't feel like explicit predictive power in a deconstructed space is critical for everyday thinking of humans. It feels a bit too science-like, rationality, which is important for progress, but not for everyday actions. It's not like we rely on processing unexpected what-ifs on a daily basis, are we? _

### References:

* Original AlexNet papers: Krizhevsky, Alex, Ilya Sutskever, and Geoffrey E. Hinton. "Imagenet classification with deep convolutional neural networks." Advances in neural information processing systems. 2012.
* and: Deng, Jia, et al. "Imagenet: A large-scale hierarchical image database." Computer Vision and Pattern Recognition, 2009. CVPR 2009. IEEE Conference on. Ieee, 2009.
* Monkey with guitar: Wang, Jianyu, et al. "Visual concepts and compositional voting." Annals of Mathematical Sciences and Applications 3.1 (2018): 151-188.
* IQ: Barrett, David, et al. "Measuring abstract reasoning in neural networks." International Conference on Machine Learning. 2018.