# ML lore

Parent: [[06_DL]], [[01_Tools]], [[hyperparameters]]
Related: [[ml_project_management]], [[Breck2017testing]], [[Huyen2019book]]

#ml #devops


**Deep learning maxims**: ([ref](https://pcc.cs.byu.edu/2017/10/02/practical-advice-for-building-deep-neural-networks/)):
* Use 80/10/10 split in training / validation / testing tests ([ref](https://medium.com/@staceysvetlichnaya/hyperparameter-search-with-iterative-sweeps-3799df1a4d45))
* Use [[adam]] with 3e-4 learning rate. Don't try to decay learning rate: ADAM takes care of that.
* ReLU are best units (except for LSTMs that use tanhs) ([ref](http://josh-tobin.com/assets/pdf/troubleshooting-deep-neural-networks-01-19.pdf))
* Never use activation function in the last layer (last layer needs to scale)
* Use bias in every layer
* Use variance-scaled weight initialization: $\mathcal{N}(0,\sqrt{2/n})$, aka "He et al." (after 2015 paper: [[He2015init]])
* Normalize (-m, /s) input data. Particularly critical if L1/L2 regularization is used! ([ref](https://medium.com/ai%C2%B3-theory-practice-business/top-6-errors-novice-machine-learning-engineers-make-e82273d394db))
* Compress (transform) variables if necessary (say, apply  tanh(x/C) )
* 128 filters in a convolution layer is a lot; if you need more, something is wrong
* Pooling (maxpooling) helps with transformation invariance (_why???_)
* For very different inputs (say, images + words) project to a relatively low-dim space first, then concat ([ref](http://josh-tobin.com/assets/pdf/troubleshooting-deep-neural-networks-01-19.pdf))

**Checkpoints**:
* Normal training (minutes to hours, while playing and debugging): test every n_epochs, save k best models, based on validation metrics ([ref](https://blog.floydhub.com/checkpointing-tutorial-for-tensorflow-keras-and-pytorch/))
* Slow training (hours to days): possibly a nested strategy, with infrequent unconditional checkpoints, and more regular performance-based checkpoints

**Debugging tricks**:
* Fix random seed, to have reproducability
* Try a rock-bottom simple model, iteratively add stuff back
* Switch to a simplified training set (fewer labels, classes, [ref](http://josh-tobin.com/assets/pdf/troubleshooting-deep-neural-networks-01-19.pdf))
* Verify that your loss starts at a theoretically expected value ([ref](http://karpathy.github.io/2019/04/25/recipe/))
* Overfit one batch (should always be doable)
* Play with learning rate (higher to get the gist of dynamics, lower to assess the minima)
* Play with batch size (1 to get the gist of what's happening, very high for most gentle and careful descent)
* Remove bach normalization, check if explosions or vanishings are happening (batch normalization is so good and useful that it obscures problems, such as NaNs)
* Find a bunch of incorrect predictions, look for commonalities
* Make sure you don't softmax outputs to a loss that expects logits or something like that
* Check for class imbalance in the testing set (compared to training)

**Hyperparameter tuning**
* Have a strategy (see [[hyperparameters]]), don't chaise idle hopes
* Come up with a time budget in advance. Ideally, automate, as it takes ~day, so maybe use this time to do something else ([ref](https://medium.com/@staceysvetlichnaya/hyperparameter-search-with-iterative-sweeps-3799df1a4d45))
* Analyze, visualize, take copious notes. Parallel coordinates with lines colored by end-result is a good way to grap the overall picture ([ref](https://medium.com/@staceysvetlichnaya/hyperparameter-search-with-iterative-sweeps-3799df1a4d45))

**Unit tests for ML** ([[unit_test]]):
* Unit tests for ML: write a test that takes one step, and compared before and after, or at least check that it changed ([ref](https://medium.com/@keeper6928/how-to-unit-test-machine-learning-code-57cf6fd81765))
* Assert that loss != 0 (ibid)
* Test that params that need to be frozen at a particular stage of training actually don't change (ibid)

**Other advice:**
* If labels are hard to get, use manual **active learning**: label small part of the dataset, use it to provide (bad) labels to the full dataset; make the model report uncertainty, use it to identify observations that need to be labeled first to most improve model performance ([ref](https://www.jeremyjordan.me/ml-projects-guide/))
* Separate data pre-processing from the learning pipeline: at rearch phase you want to pre-process data once, then play with it repeatedly ([ref](https://medium.com/infinity-aka-aseem/things-we-wish-we-had-known-before-we-started-our-first-machine-learning-project-336d1d6f2184))
* You can always gain a few more % by using ensembles ([ref](http://karpathy.github.io/2019/04/25/recipe/))

# Refs

How Microsoft runs AI / ML projects (best practices, convergent among several different teams):
Amershi, S., Begel, A., Bird, C., DeLine, R., Gall, H., Kamar, E., ... & Zimmermann, T. (2019, May). Software engineering for machine learning: A case study. In 2019 IEEE/ACM 41st International Conference on Software Engineering: Software Engineering in Practice (ICSE-SEIP) (pp. 291-300). IEEE.
https://www.microsoft.com/en-us/research/uploads/prod/2019/03/amershi-icse-2019_Software_Engineering_for_Machine_Learning.pdf