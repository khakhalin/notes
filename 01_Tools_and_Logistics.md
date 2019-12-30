# Tools, coding, project management
All sorts of infrastructure stuff.

## Python

#### Core Python
* Tips from Chip Huyen: https://github.com/chiphuyen/python-is-cool
* Nice [list of Python gotchas](https://www.toptal.com/python/top-10-mistakes-that-python-programmers-make) from Martin Chilikian
* f-strings: http://zetcode.com/python/fstring/

#### Matplotlib
* Brief intro from Brad Solomon: https://realpython.com/python-matplotlib-guide/
* Cheatsheet: [https://github.com/rougier/matplotlib-cheatsheet](<https://github.com/rougier/matplotlib-cheatsheet>)

#### Pandas
* `[[]]` simply means "a list inside a `[]` call"
* Select columns by label: `d['x']`. Alternative spelling: `d.x`. Returns a series. 
* A series can then be further sliced by row: `d.x[1]`. Warning: this is a good way to read, but not to write, due to the **chained assignment** problem (view vs copy). Is equivalent to `d['x'][1]`. *It seems to work for some reason, somehow, even though it shouldn't really.*
* Row by label: `d.loc[]`. Works for both df (returns a row-series), and for series (returns a single value).
* One proper way to read and write: reference by name (index): `d.loc[1,'x']` - doesn't cause chained assignment.
* Another good way: `d.iloc[1,0]` - same as above, just by position.
* Both `d.x.iloc[1]` and `d.iloc[1].x`are OK for reading, but not good for writing. Row-Col never works for assignment; Col-Row seems to work, but produces a warning in older Pandas (no warning in newer version).

### Random Python tips:
* If in a module you start a method name with one underscore, like `_helper`, this method isn't imported on `from module import *`. Unfortunately is still acessible if you do `import module`and address it as `module._helper()`.
* Conventions: underscore for variables and functions; all-caps for constants, capitalized camelcase for objects, leading-underscore for local methods.
* Nested comprehensions: same syntax as in writing nested loops (even tho it looks unformulaic), e.g. `[j for i in range(5) for j in range(i)]
* To add += 1 to a dict when a key may not exist, use `get()` as it has a default value: `a[i] = a.get(i,0)+1`
* To get some (or rather, first) dict from a dict, do `next(iter(a.keys()))`
* Objects (including empty lists `[]`) should never be used as default arguments for functions, as they are evaluated only once per program (during object definition), not when methods are called! Insetad use `x=None`, then `if x is None: x=[]`. It sounds super-cumbersome, but that's just how it is. ([source](https://docs.python-guide.org/writing/gotchas/))

### Coding habits for data scientists
* Keep code clean (not smelly). Types of **smells** ([ref](https://www.thoughtworks.com/insights/blog/coding-habits-data-scientists)):
    * Dead code (commented, inconsequential)
    * Print statements everywhere
    * Bad variable names
    * Functions that do too many things instead of one thing
    * Code repetition
    * Magic values
* Smuggle code from Jupyter to classes as soon as possible (Jupyter only for protopying, reporting, and use case)
* Write unit tests ([link to a decent intro](https://www.freecodecamp.org/news/an-introduction-to-testing-in-python/))
* Make small and frequent commits

## Scikit-learn
* A bunch of notebooks that implement all key ML methods, by Aurélien Geron, to accompany his book ("Hands-On Machine Learning with Scikit-Learn and TensorFlow"): https://github.com/ageron/handson-ml2

## Tensorflow and Keras
* Links to several tutorials: https://github.com/sayakpaul/TF-2.0-Hacks/blob/master/README.md

#### Random notes:
* **Tensor object**:  type, shape, and a bunch of numbers. For example, when working with images, we have a 4D structure: image# × W × H × ColorChannels.
* TF relies on a function that iterates through (features, labels) as tuples. And instead of directly linking to data, it wants to receive a data-generating function (for lazy / parallel execution?).

## GIT
* Funny short cheatsheet "Dangit": http://dangitgit.com/

# ML Project Organization
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
* [Advice from Chip Huyen](https://github.com/chiphuyen/machine-learning-systems-design/blob/master/build/build1/consolidated.pdf)

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

## Glossary of useful and useless slang
* **Drop-in replacement:** replacing part of the code without rewriting anything else. AKA 	"bug for bug compatibility" (drop-in will only work if all idiosynctratic bugs match exactly)
* **Feature store:** A practical concept for data project implementation: a collection of curated features that is updated from new data, and can be tapped into.
* **Pickling:** dumping binary data into a database, to be loaded later, instead of processing it in some meaningful way. In Python, used for serializing Python object structure. A better alternative: JSON, as it's human-readable, while pickles aren't.
* **Tehnical debt**: going for an easy (and quick) but limited solution, even though statistically every limiting decision may have to be reingeneered (scaled up) later.