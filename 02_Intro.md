# General ideas about models

## Model assessement

#todo: Precision and recall

## Project organization

#management

Basic ideas: (from https://www.jeremyjordan.me/ml-projects-guide/)
1. Define scope, feasibility, targets (accuracy, speed, size?)
2. Define ground truth
3. Validate data
4. Establish baselines (silly models)
5. Underfit with simplistic models
6. Overfit with simplistic models
7. Is there a SoTA model? If yes, apply.
8. Develop model by iterating through: add complexity; debug; tune hyperparameters, benchmark
9. Test on test data
10. Write tests for input data, model performance

**Active learning**: label small part of the dataset, use it to provide (bad) labels to the full dataset; use model uncertainty to identify observations that need to be labeled first to most improve model performance

How to fight various problems:
* Underfitting: bigger model (model capacity); less regularization; advanced architecture; hyperparameters; more features
* Underfitting: more training data; regularization; data augmentation; reduce model size

Longer reviews:
* [A rubric for production readiness](papers/Breck2017testing.md) - How to tell whether an ML solution is ready for production?

## Pieces of ML lore wisdom

https://pcc.cs.byu.edu/2017/10/02/practical-advice-for-building-deep-neural-networks/
General advice for deep learning:
* Use ADAM. Don't decay learning rate: ADAM takes care of that.
* ReLU are the best
* Make sure there's no activation function in the last layer
* Bias should be in every layer (but of course?)
* Use variance-scaled initialization
* Compress data if necessary (apply  tanh(x/C) or something similar)
* Normalize (-m, /s) input data
* 128 filters in a convlution layer is a lot
* Pooling (maxpooling) helps with transform invariance (? why ?)

Model debugging:
* First, try to overfit
* Play with learning rate (higher to get the gist, lower to assess the minima)
* Play with batch size (1 to get the gist of what's happening, very high for most gentle and careful descent)
* Remove bach normalization, check if explosions or vanishings are happening (it's so good, it obscures NaNs)
* Fix a random seed, to have reproducability
* Select incorrect predictions, look for commonalities