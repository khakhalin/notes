# Tools, coding, project management

#tools

#todo: Command Line crash course
https://learnpythonthehardway.org/book/appendixa.html

#todo: "The missing semester of CS education": 
https://missing.csail.mit.edu/
lots of useful practical bits and pieces: the shell, debugging, metaprogramming and what not. Also has [lectures on youtube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J).

Major subtopics:
* [[system_design]]

Cheatsheets, tips, and notes:
* [[python]] - everything Python (but not numpy or pandas that have their own )
* [[pandas]] - Pandas obviously :)
* [[git]] - GIT cheat sheet
* [[sql]] - SQL cheat sheet

Project management and alike:
* [[scrum]]

ML projects, general advice:
* [[ml_lore]] - a collection of rules of thumb about everything Deep Learning (which defaults to pick etc.)

Other #lifehack(s):
* [[Clear2019zettelkasten]] - how to properly create and support this knowledge base
* [How to do citations with Zettlr](https://docs.zettlr.com/en/academic/citations/) - #todo, this must be nice! 

# Good coding habits
* Keep code clean (not smelly). Types of **smells** ([ref](https://www.thoughtworks.com/insights/blog/coding-habits-data-scientists)):
    1. Bad variable names (Remember: a long name is always better than a long comment!)
    2. Functions that do many things instead of one thing
    3. Code repetition
    4. Magic values (hard-coded values like 256, 0.9 etc.)
    5. Dead code (commented out, inconsequential)
    6. Print statements everywhere (ruins of abandoned debugging)
 
Instead:
* Write unit tests, and start with tests (aka **test-driven development**; [ref](https://www.freecodecamp.org/news/an-introduction-to-testing-in-python/))
* Make small and frequent commits
* Functions ([ref](https://drive.google.com/file/d/1TraVwRkbkCbHq-s_-NS69ZEbRNwH8XNh/view)):
    * should be small (~20 lines = 1 screen; better lesst than that), 
    * do one thing, 
    * at one level of abstraction (like, it shouldn't combine high-level processing of an object, and low-level processing of its parts; it should be dealt with by a hierarchy of functions, not raw code in one function)
    * have few arguments (ideally, 0-2)
    * avoid flags (`also_do_this=True`); consider writing 2 functions instead

# Numpy
* For linear algebra, to start flipping vectors and use them as matrices, use `np.atleast_2d` - it turns vectors, and even scalars, into low-ranked 2D arrays. I think, rows for vectors.

# Matplotlib
* Brief intro from Brad Solomon: https://realpython.com/python-matplotlib-guide/
* Cheatsheet: [https://github.com/rougier/matplotlib-cheatsheet](<https://github.com/rougier/matplotlib-cheatsheet>)

# TF and Keras
* Links to several tutorials: https://github.com/sayakpaul/TF-2.0-Hacks/blob/master/README.md

Random Notes:
* **Tensor object**:  type, shape, and a bunch of numbers. For example, when working with images, we have a 4D structure: image# × W × H × ColorChannels.
* TF relies on a function that iterates through (features, labels) as tuples. And instead of directly linking to data, it wants to receive a data-generating function (for lazy / parallel execution?)

**Data Pipeline**: TF wants a Python iterable. If data can be loaded in memory, use `tf.data.Dataset.from_tensors()` or `from_tensor_slices()`. Later, a dataset object may be transformed using various TF methods, like `shuffle()` for reshuffling, or `batch(bath_size)` to create a batched dataset.

When building the model, for the first layer, set `input_shape` to a tuple describing the dimensions, assuming that the first (silent, unspecified) dimension will be the batch size. TF can also directly 'cosume' Python generators (functions that produce values lazily, using `yield` instead of `return`), although documentation warns that there are some potential traps with this.

Three different ways to fit:
* `fit` - best choice for small datasets. Takes x and y explicitly; you specify `batch_size` as a parameter, as it is the fit that will do batching. Good if entire set fits in RAM, and there's no data augmentation (no on-the-fly data, so to say). [ref](https://www.pyimagesearch.com/2018/12/24/how-to-use-keras-fit-and-fit_generator-a-hands-on-tutorial/)
* `fit_generator` - accepts a data generator instead of static data, and in this case it is the Dataset generator object (or your custom function) that specifies the shape, including batch size. With `steps_per_epoch`, you can tell it just how many new data points to generate per epoch. Note that with fit_generator, you'll have an extra dimension for batch in the model, at the inputs level, so for prediction you'd either have to use `predict_generator` (which makes sense if you generate data for it on the fly), or expand x with a leading empty axis using numpy (np.reshape(1, … )), and then np.squeeze(y) the output back to normal.
* `train_on_batch` - trains on one batch and updates the mode.

# ML Project Structure
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
* Underfitting: bigger model (increase model capacity); less regularization; advanced architecture; tune hyperparameters; add more features
* Overfitting: more training data; regularization; data augmentation; reduce model size

Interesting point: in ML research, one kinda cannot afford to skip hyperparater tuning and data augmentation, as many changes to standard approaches that are supposedly helpful, may be just bad proxies for either regularization, or augmentation. So these additional directions shoudl be saturated for both "control" and "newsly developed" methods, to make all comparisons fair. (I'm guessing, unless the new methods are easier to run than regularization, but it is kinda unlikely.) Ref: [tweet by Jeremy Howard](https://twitter.com/jeremyphoward/status/1215320099134984194)

Refs:
* [Data Science Checklist](https://www.fast.ai/2020/01/07/data-questionnaire/) by Jeremy Howard
* How to tell whether an ML solution is ready for production? [[Breck2017testing]]
* Blog of [Jeremy Jordan](https://www.jeremyjordan.me/ml-projects-guide/)
* Blog of [Aseem Bansal](https://medium.com/infinity-aka-aseem/things-we-wish-we-had-known-before-we-started-our-first-machine-learning-project-336d1d6f2184)
* [Presentation by Josh Tobin](http://josh-tobin.com/assets/pdf/troubleshooting-deep-neural-networks-01-19.pdf)
* [Andrej Karpathy on training NNs](http://karpathy.github.io/2019/04/25/recipe/)
* [Advice from Chip Huyen](https://github.com/chiphuyen/machine-learning-systems-design/blob/master/build/build1/consolidated.pdf)