# Fashion DNA

Bracher, C., Heinz, S., & Vollgraf, R. (2016). Fashion DNA: merging content and sales data for recommendation and article mapping. arXiv preprint arXiv:1609.02489.
(Zolando research)

#representation #image #clustering #recommendation

Seems mostly a data-driven paper.
* Take product description (one-hot encoding of expert-based tags), transform (4-layer ReLu funnel with drop-out)
* separately take product image, transform (8-layers convnet, AlexNet-based: [[alexnet]])
* concat the two, 
* transform the result (1 layer), get internal representations.
* from them, predict purchases using a logistic regressor. Use actual purchases for backprop training.

> I'm not quite sure how they trained. At first it reads as if they trained the entire network from that actual order data, but then later it seems as if they only trained the last layer (post-concatenation).

Once the representation is there, it can also be used to find a set closest neighbors for every item, and also (using t-SNE) - to build a cloud of all items.

The use **Caffe deep learning framework** for all their models.

> Is it like an alternative to TF? According to wikipedia, Caffe2 was merged inty PyTorch, so not sure what the status of the standalone product is.

# Refs

Related (follow-up) publication:
* Lasserre, J., Bracher, C., & Vollgraf, R. (2018, January). Street2fashion2shop: Enabling visual search in fashion e-commerce using studio images. In International Conference on Pattern Recognition Applications and Methods (pp. 3-26). Springer, Cham.
A description of a **pipeline** that segments models from photos (removes background), then segments out the item, then projects the item into the space, thus allowing people to e-shop well.