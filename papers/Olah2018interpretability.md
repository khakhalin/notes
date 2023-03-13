# The building blocks of interpretability

Olah, C., Satyanarayan, A., Johnson, I., Carter, S., Schubert, L., Ye, K., & Mordvintsev, A. (2018). The building blocks of interpretability. Distill, 3(3), e10. https://distill.pub/2018/building-blocks/

Parents: [[interpretability]]
See also: [[convnet]]

#interpretability #convnet


Starts with a ~5-ref-for-each naming of existing techniques, such as:
* feature visualization
* attribution
Here they try to combine these methods to understand one [[convnet]]-based classification network.

Really nice visualization where you can hover mouse over an image, and see which neurons (visualized through their receptive fields) fire most at each location, and what labels they contribute too (things like "grass", "furry paws", and "dog snout"). Aka, a **semantic dictionary**. A problem here is that image-based categories don't necessarily map on language-like tags; sometimes a whole gradient corresponds to a single tag; sometimes natural language tags just don't exist.

Then they use a funny visualization technique:
1. First for each activated column (one location, several top filters) they generate a synthetic image that produces the same activation (or at least a very similar one, in terms of cosine distance = scalar product between 2 activations)
2. Then they replace the original image with a collage of small synthetic images. It helps a human to see how the ANN "sees" the image; where the "idea of a snout" ends, and "the idea of a cat" or that "of a grass" starts.

A parallel browser across 4 consecutive layers of a network: REALLY PRETTY!!!

One can then combine it with visualizing the strength of activation, and a saliency map. At different levels (again a browser!), including showing what positions from previous layers contributed to the "opinion" of this layer, and what opinions in consecutive layers current activity contributes to.

> A side note on their presentation methodology: Essentially, by making it interactive, they have an effective 5D visualization, where 2 dimensions + layer number are fixed, and the person samples through the other two. Because the person is smart, and the "exploration" dimension is not fully orthogonal to the spatial dimension, it actually works! A surprising amount of information transmitted through a single interactive app.

Then do Sankey-diagram-style visualizations for attributions of key neuronal groups. (Like some sort of clustering, simultaneously in the spatial and filter space? I wonder how they do it in practice...)

Finish with some discussion (with a different, diagrammatic type of illustrations) on optimal interfaces (among those they demonstrated) to solve different practical problems of "understanding a network".

> Overall it seems like a weird case of data exploration, where to "understand how a model works" a human would have to learn the internal language of this model first, similar to how we learn how a bad apple is different form a good apple, or how people working with some sort of data learn to identify questionable data "by eye". Because to a naive user lots of those visualizations looks surprisingly alike - all furry, spiky, colorful, and with random eyes and noses inserted here and there: even the visuals for "grass" has a bit of a dog in it (because its a neuron for "grass under an animal", I guess). Factoring and ordering dimensions by magnitude, and clustering neurons into groups helps a bit, but still, while it is VERY PRETTY, it feels like a far cry from a "human-expressible" understanding, [[Lillicrap2019understand]]-style.