# Introduction to ML models

## Model assessement

#todo: Precision and recall

## Project organization

#management

Basic ideas: (from https://www.jeremyjordan.me/ml-projects-guide/)
1. Define scope, feasibility, targets (accuracy, speed, size?)
2. Define ground truth
3. Validate data
4. Identify baselines (stupid / simplistic models), fit and overfit with them
7. Is there a SoTA model? If yes, apply.
8. Develop model by iterating through a cycle: add complexity; debug; tune hyperparameters, benchmark
9. Test on test data
10. Write tests for input data, and model performance

If labels are hard to get, use **active learning**: label small part of the dataset, use it to provide (bad) labels to the full dataset; use model uncertainty to identify observations that need to be labeled first to most improve model performance

How to fight bad fit:
* Underfitting: bigger model (model capacity); less regularization; advanced architecture; hyperparameters; more features
* Underfitting: more training data; regularization; data augmentation; reduce model size

More reviews:
* How to tell whether an ML solution is ready for production? [[Breck2017testing]]

## Pieces of ML lore wisdom

General advice for deep learning ([ref](https://pcc.cs.byu.edu/2017/10/02/practical-advice-for-building-deep-neural-networks/)):
* Use ADAM. Don't decay learning rate: ADAM takes care of that.
* ReLU are best units
* Make sure there's no activation function in the last layer
* Make sure you use bias in every layer
* Use variance-scaled initialization
* Compress (transform) variables if necessary (say, apply  tanh(x/C) )
* Normalize (-m, /s) input data
* 128 filters in a convlution layer is a lot; if you need more, something is wrong
* Pooling (maxpooling) helps with transform invariance (? why ?)

Model debugging:
* First, try to overfit
* Play with learning rate (higher to get the gist, lower to assess the minima)
* Play with batch size (1 to get the gist of what's happening, very high for most gentle and careful descent)
* Remove bach normalization, check if explosions or vanishings are happening (it's so good, it obscures NaNs)
* Fix a random seed, to have reproducability
* Select incorrect predictions, look for commonalities