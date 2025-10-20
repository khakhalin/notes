# Typical Sample

Parents:
Related: [[curse_dim]], [[features]], [[metric]], [[smote]]

#dim #stats #interpretability


**Typical Sample** is an idea related to the Curse of Dimensionality: in higher dimensions the idea of "average point" becomes less useful. A mean of lots of colored noises is a gray square, which looks nothing like any of the data points.

"In very-high-dim a Gaussian is almost indistinguishable from a uniform distributinon on a sphere." (Huszar 2017)

Therefore, the only reasonable way to deal with very-high-d spaces is to find points that belong to the distribution, but have a high response, and show these points.

Corollaries: 
* Interpolation between points in high-d cannot be linear, as high-way you'll have an average of 2 points that doesn't lie in the distribution. Instead, use polar coordinates. _Would Manhattan interpolation be a case of polar coordinates, or is it different?_
* When interpreting detectors, be prepared that if you are optimizing in a space that is very different frm the space of stimuli, the "optimized stimulus" will look weird. (Like a visual illusion of sorts? See [[Olah2017visualization]])

# Refs

Ferenc Huszár 2017.
https://www.inference.vc/high-dimensional-gaussian-distributions-are-soap-bubble/

Golowasch J, Goldman MS, Abbott LF, Marder E (2002) Failure of averaging in the construction of a conductance-based neuron model. J Neurophysiol 87:1129–1131 
If one averages cell parameters, and plugs them in the model, the resultant "average cell" spikes entirely differently than original cells, as parameters are no longer coordinated. But does it mean by extension that comparing averages of underlying parameters across groups may not be the best way to detect effects of experiemental manipulations? See [[cpg]], [[intrinsic_plasticity]].