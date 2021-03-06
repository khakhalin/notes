# Neural control with deep synthesis

Bashivan, P., Kar, K., & DiCarlo, J. J. (2019). Neural population control via deep image synthesis. Science, 364(6439). https://www.gwern.net/docs/ai/2019-bashivan.pdf

Parent: [[vision]]
See also: [[superstimulus]], [[convnet]], [[Olah2017visualization]], [[interpretability]]

#vision #illusion

General idea: train a DL (convnet) for image classification. Record one neuron from the brain. Then use DL last layer to approximate responses of this neuron. Now, using this setup as a model for this neuron, find optimal stimuli that would drive the DL model strongly. Test it in the brain - see that indeed we found a super-stimulus for this neuron. Similarly, we can try to drive aribtrary populations of neurons, while suppressing some other populations.

Chronic implanted electrodes in 3 rhesus macaques. Recordings from V4, for responses to a bunch of synthetic (simple shapes) and natural stimuli.

ANN: 5 convolutional layers, followed by 3 dense, to classify ImageNet.

To find optimal stimuli, either performed greedy pixel-by-pixel descent from a random image (aka **stretch** procedure), or using a gradient descent with one-hot target (maximize this neuron, while suppressing others, with [[softmax]] formula as target (exp(y_i) over the Σ of exps for all units), using response variation and high-frequency suppression as additional regularization terms.

> That's not a good enough explanation to replicate the process, but they reference 3 papers, and since then I think we got more work on this topic from Olah Mordvintsev, no?

During super-driving, responses in brain neurons were weaker than predicted from the model, by some 20-30%, but still very strong (worse accuracy than for normal stimuli, but ~2-3 times stronger spiking than usual!). (That's based on Fig 2)

Superstimuli ended up being pressy similar to those generated Mordvintsev & Olah (e.g. [[Olah2017visualization]]) for individual neurons in convnets, except maybe also receptive-fielded to one part of the visual space only (judging from Figs 4-5)

# Refs

For image synthesis:
* D. Erhan, Y. Bengio, A. Courville, P. Vincent, Visualizing higher-layer features of a deep network (Departement d’Informatique et Recherche Opérationnelle, Université de Montréal, 2009); https://pdfs.semanticscholar.org/65d9/94fb778a8d9e0f632659fb33a082949a50d3.pdf
* M. D. Zeiler, R. Fergus, in European Conference on Computer Vision 2014 (Springer, 2014), pp. 818–833; https://cs.nyu.edu/~fergus/papers/zeilerECCV2014.pdf.
* I. J. Goodfellow, J. Shlens, C. Szegedy, Explaining and harnessing adversarial examples. arXiv 1412.6572  (20 March 2015)