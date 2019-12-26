# Tools, coding, project management
All sorts of infrastructure stuff.

## Python tips and tricks

**Python itself**
* Tips from Chip Huyen: https://github.com/chiphuyen/python-is-cool

**Matplotlib**
* Brief intro from Brad Solomon: https://realpython.com/python-matplotlib-guide/
* Cheatsheet: [https://github.com/rougier/matplotlib-cheatsheet](<https://github.com/rougier/matplotlib-cheatsheet>)

**Coding habits for data scientists** ([ref](https://www.thoughtworks.com/insights/blog/coding-habits-data-scientists))
* Keep code clean (not smelly). Types of **smells**:
    * Dead code (commented, inconsequential)
    * Print statements everywhere
    * Bad variable names
    * Functions that do too many things instead of one thing
    * Code repetition
    * Magic values
* Smuggle code from Jupyter to classes as soon as possible (Jupyter for protopying, and maybe reporting?)
* Write unit tests ([link to a decent intro](https://www.freecodecamp.org/news/an-introduction-to-testing-in-python/))
* Make small and frequent commits

**Random Python notes**
* If in a module you start a method name with one underscore, like `_helper`, this method isn't imported on `from module import *`. Unfortunately is still acessible if you do `import module`and address it as `module._helper()`.
* Conventions: underscore for variables and functions; all-caps for constants, capitalized camelcase for objects, leading-underscore for local methods.
* Nested comprehensions: same syntax as in writing nested loops (even tho it looks unformulaic), e.g. `[j for i in range(5) for j in range(i)]
* To add += 1 to a dict when a key may not exist, use `get()` as it has a default value: `a[i] = a.get(i,0)+1`
* To get some (or rather, first) dict from a dict, do `next(iter(a.keys()))`

## GIT

* Funny short cheatsheet "Dangit": http://dangitgit.com/

## Tensorflow 2.0 and Keras

* Links to several tutorials: https://github.com/sayakpaul/TF-2.0-Hacks/blob/master/README.md

#### Basics:
* **Tensor object**:  type, shape, and a bunch of numbers. For example, when working with images, we have a 4D structure: image# × W × H × ColorChannels.
* TF relies on a function that iterates through (features, labels) as tuples. And instead of directly linking to data, it wants to receive a data-generating function (for lazy / parallel execution?).

## Scikit-learn

* A bunch of notebooks that implement all key ML methods, by Aurélien Geron, to accompany his book ("Hands-On Machine Learning with Scikit-Learn and TensorFlow"): https://github.com/ageron/handson-ml2