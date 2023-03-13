# Deep features as a perceptual metric

Zhang, R., Isola, P., Efros, A. A., Shechtman, E., & Wang, O. (2018). The unreasonable effectiveness of deep features as a perceptual metric. In Proceedings of the IEEE conference on computer vision and pattern recognition (pp. 586-595). https://openaccess.thecvf.com/content_cvpr_2018/papers/Zhang_The_Unreasonable_Effectiveness_CVPR_2018_paper.pdf 1k+ citations in 2 years!

Parent: [[vision]]
Related: [[Bashivan2019control]]

#vision #convnet

Compared different network architectures to build predictors for what images seem similar to a human. As a ground truth, lots of responses from Mechanichal Turk, where people were either presented with an original image and 2 distortions, and needed to quantify which one is "less distorted" (aka **2AFC**), or were asked to detect if the distortion is present (minimally perceptible distortion, aka **JND**, just noticeable difference). Used both traditional (photoshop-style) and CNN distortions (more style transfer style, which is probably a neat vaccination against adversarial attacks).

Concluded that networks are actually pretty good; worse than humans, but still great. The specific architecture doesn't mean that much, but rather, it matters that the network works well on whatever it was trained to do (either labeled classification, or self-supervised task). Conclude it with a maxim that **a good feature is a good feature**, in the sense that if a feature describes an image well, then probably it will be g good feature for many different tasks, regardless of what the tasks are specifically.