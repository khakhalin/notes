# Artificial Neural Networks Accurately Predict Language Processing in the Brain

Martin Schrimpf,  Idan A Blank, Greta Tuckute, Carina Kauf, Eghbal A Hosseini, NANCY G KANWISHER, Joshua B. Tenenbaum, Evelina Fedorenko
https://www.biorxiv.org/content/10.1101/2020.06.26.174482v1

Twitterstorm:
https://twitter.com/martin_schrimpf/status/1276832601240731649

#neuro #language

Related: [[transformers]], [[10_Text]]

**TLDR**: Modern language models (such as transformers) are activated roughtly similarly to human brains, in terms of activation dynamics; and it all comes from the architecture (recurrence, convergence, divergence), not from actual tuning of weights.

Looked at previously published (their own) fMRI and ECoG data. Present same stimuli to models, and compare human activation to model activation. Also use some fancy statistical extrapolation of their MRI data to an "infinite number of subjects" - I don't understand that. Some kind of bootstrapping?

For models: 43 different models, including transformers, RNN, embeddings (GloVe). Models (like GPT-2) fit the data, while embeddings don't. What they mean is that if you feed different data to the model, look into the internal representation, and pretend that this vector of representations is actually a vector of activations form a fMRI experiment, you can actually find a match between the two. Like, **you can roughly predict fMRI activations from model representations** of the same stimulus. The relative time required to process each sentence is also roughly matched between humans and models (r~0.5), and so is nex-word prediction perplexity (r~0.5), but not GLUE benchmarks (?).

**Tuning is not necessary**: The most interesting claim though is that most of this activation matching doesn't come from model turning, but from its large-scale architecture, as it is present already in untrained models (with random weight), and it does not change much after training. (They call it "prediction", but what they mean is "prediction of neural data", or activation). 

This last statement is apparently similar to their other paper about vision, where they also show that in terms of ML activations of image-processing networks matching biology, it is the architecture that is important, and not the tuning:

* Geiger, F., Schrimpf, M., Marques, T., & DiCarlo, J. (2020). Wiring Up Vision: Minimizing Supervised Synaptic Updates Needed to Produce a Primate Ventral Stream. bioRxiv.
Protoid: Geiger2020ventralstream

They used **huggingface** transformer library.