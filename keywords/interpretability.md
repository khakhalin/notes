# Interpretability

For now, a stub, to collect good info on interpretability.

Parents: [[06_DL]]
See also: [[fairness]], [[dataviz]]

#interpretability #dim #datavis #bib


# Papers

* [[Hinton2006dim]] - used a PCA-like plot to understand representation in a deepest layer of the autoencoder, by training a separate 2-wide autoencoder
* [[Lillicrap2019understand]] - what does it mean to understand a neural network? (a plug for [[curriculum]]?)
* "Circuits" series from Google Brain: https://distill.pub/2020/circuits/
        * [[Olah2017visualization]] - interpreting visual classifiers
        * [[Olah2018interpretability]] - building blocks of interpretability
        * https://distill.pub/2019/computing-receptive-fields/
        * https://distill.pub/2020/circuits/zoom-in/
        * https://distill.pub/2020/circuits/curve-detectors/
        * https://distill.pub/2021/multimodal-neurons/
* [[Saxe2020interpretable]] - a blog post about additive ANN models (GAMs)

# Comparing networks

Maybe (probably) need to be moved to a separate section? But how to name it? Basically, it's about figuring out whether 2 networks are "equivalent" in some sort, or not.

* [[Yamins2016viscortex]] - comparing layers from their activations

Rolnick, D., & Kording, K. (2020, November). Reverse-engineering deep ReLU networks. In International Conference on Machine Learning (pp. 8178-8187). PMLR. https://arxiv.org/abs/1910.00744

Haber, A., & Schneidman, E. (2020). Learning the architectural features that predict functional similarity of neural networks. bioRxiv. https://www.biorxiv.org/content/10.1101/2020.04.27.057752v1

Maheswaranathan, N., Williams, A. H., Golub, M. D., Ganguli, S., & Sussillo, D. (2019). Universality and individuality in neural dynamics across large populations of recurrent networks. Advances in neural information processing systems, 2019, 15629. https://arxiv.org/abs/1907.08549

# Random thoughts

It's possible that ANNs don't feel like good "conceptual models" just coz the concept of a conceptual model was shaped by pre-ANN scientists. As quantum mechanics or chaos needed >30 yrs to get normalized, maybe ANNs will feel less opaque, not because of some honest progress in interpretability, but just because we'll get used to them? And will internalize some basic principles of their work, including representation, transformations, maybe even some consequences of lottery tickets and built-in ensemble effects? Understanding the rationale behid these effects may not feel like working on interpretability, but may lead to same eventual psychological effect.

# To read

https://distill.pub/2019/computing-receptive-fields/

https://distill.pub/2020/circuits/zoom-in/


https://distill.pub/2020/circuits/curve-detectors/

https://distill.pub/2021/multimodal-neurons/
