# Introduction to ML models

## Model assessement

#todo: Precision and recall

## Project organization

#management

Basic ideas:
1. Define scope, feasibility, targets (accuracy, speed, size?)
2. Define ground truth
3. Validate data, swim in it, write data tests upfront.
4. Version your data
5. Identify baselines (stupid / simplistic models). Fit and overfit with them
7. Is there a SoTA model?
8. Start very simple (aka "Don't be a hero")
9. Develop model by iterating through a cycle ([ref](http://josh-tobin.com/assets/pdf/troubleshooting-deep-neural-networks-01-19.pdf)): 
                        * add complexity; 
                        * debug; 
                        * tune hyperparameters, 
                        * benchmark
10. Test on test data
11. Write tests for model performance (to be alerted if it unexpectedly drops on new data)

How to fight bad fit:
* Underfitting: bigger model (model capacity); less regularization; advanced architecture; hyperparameters; more features
* Overfitting: more training data; regularization; data augmentation; reduce model size

Refs:
* How to tell whether an ML solution is ready for production? [[Breck2017testing]]
* Blog of [Jeremy Jordan](https://www.jeremyjordan.me/ml-projects-guide/)
* Blog of [Aseem Bansal](https://medium.com/infinity-aka-aseem/things-we-wish-we-had-known-before-we-started-our-first-machine-learning-project-336d1d6f2184)
* [Presentation by Josh Tobin](http://josh-tobin.com/assets/pdf/troubleshooting-deep-neural-networks-01-19.pdf)
* [Andrej Karpathy on training NNs](http://karpathy.github.io/2019/04/25/recipe/)

## ML lore

**Deep learning maxims**: ([ref](https://pcc.cs.byu.edu/2017/10/02/practical-advice-for-building-deep-neural-networks/)):
* Use ADAM with 3e-4. Don't decay learning rate: ADAM takes care of that.
* ReLU are best units (except for LSTMs that use tanhs ([ref](http://josh-tobin.com/assets/pdf/troubleshooting-deep-neural-networks-01-19.pdf)))
* Never use activation function in the last layer
* Use bias in every layer
* Use variance-scaled weight initialization: $\mathcal{N}(0,\sqrt{2/n})$, aka "He et al." (after 2015 paper)
* Normalize (-m, /s) input data. Particularly critical for L1/L2 regularization ([ref](https://medium.com/ai%C2%B3-theory-practice-business/top-6-errors-novice-machine-learning-engineers-make-e82273d394db)))
* Compress (transform) variables if necessary (say, apply  tanh(x/C) )
* 128 filters in a convlution layer is a lot; if you need more, something is wrong
* Pooling (maxpooling) helps with transform invariance (? why ?)
* For very different inputs (say, images + words) project to low-dim (dozens) space first, then concat ([ref](http://josh-tobin.com/assets/pdf/troubleshooting-deep-neural-networks-01-19.pdf))

**Checkpoints**:
* Normal training (minutes to hours, while playing and debugging): test every n_epochs, save k best models, based on validation metrics ([ref](https://blog.floydhub.com/checkpointing-tutorial-for-tensorflow-keras-and-pytorch/))
* Slow training (hours to days): possibly a nest strategy, with infrequent unconditional checkpoints, and more regular performance-based checkpoints

**Debugging tricks**:
* Fix random seed, to have reproducability
* Try a rock-bottom simple model, iteratively add stuff back
* Switch to a simplified training set (fewer labels, classes, [ref](http://josh-tobin.com/assets/pdf/troubleshooting-deep-neural-networks-01-19.pdf))
* Verify that your loss starts at a theoretically expected value ([ref](http://karpathy.github.io/2019/04/25/recipe/))
* Overfit one batch (shoudl always be doable)
* Play with learning rate (higher to get the gist of dynamics, lower to assess the minima)
* Play with batch size (1 to get the gist of what's happening, very high for most gentle and careful descent)
* Remove bach normalization, check if explosions or vanishings are happening (batch normalization is so good and useful that it obscures problems, such as NaNs)
* Find a bunch of incorrect predictions, look for commonalities
* Make sure you don't softmax outputs to a loss that expects logits or something like that
* Check for class imbalance in the testing set (compared to training)

**Unit tests for ML**:
* Unit tests for ML: write a test that takes one step, and compared before and after, or at least check that it changed ([ref](https://medium.com/@keeper6928/how-to-unit-test-machine-learning-code-57cf6fd81765))
* Assert that loss != 0 (ibid)
* Test that params that need to be frozen at a particular stage of training actually don't change (ibid)

**Other advice:**
* If labels are hard to get, use manual **active learning**: label small part of the dataset, use it to provide (bad) labels to the full dataset; make the model report uncertainty, use it to identify observations that need to be labeled first to most improve model performance ([ref](https://www.jeremyjordan.me/ml-projects-guide/))
* Separate data pre-processing from the learning pipeline: at rearch phase you want to pre-process data once, then play with it repeatedly ([ref](https://medium.com/infinity-aka-aseem/things-we-wish-we-had-known-before-we-started-our-first-machine-learning-project-336d1d6f2184))
* You can always gain a few more % by using ensembles ([ref](http://karpathy.github.io/2019/04/25/recipe/))