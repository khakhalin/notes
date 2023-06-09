# Generative algorithms and techniques

Parents: [[algo]], [[08_Unsupervised]]
See also:

#generative #bib


Subtopics:
* [[autoencoder]]
* [[gan]] - generative adversarial network
* [[augmentation]] - data augmentation
* [[wf_collapse]] - Wave Function Collapse
* [[perceptual_loss]] - a good loss to optimize image generators
* [[generative_graphRNNs]] - generating graphs with RNNs

# Taxonomy of generative models

* Maximum likelihood
    * Explicit density
        * Tractable Density
            * Fully visible belief nets, NADE, MADE, PixelRNN, Change of variables models (nonlinear ICA)
            * Auto-regressive models
        * Approximate density
            * Variational: [[vae]]
            * Markov Chains - Boltzmann machines
    * Implicit density
        * Direct: [[gan]]
        * Markov Chains - GSN
* Other…

Ref: Course materials by Jure Leskovec:
http://web.stanford.edu/class/cs224w/slides/10-graph-gen.pdf