# Tools, coding, project management

#todo: Command Line crash course
https://learnpythonthehardway.org/book/appendixa.html

#todo: "The missing semester of CS education": 
https://missing.csail.mit.edu/
lots of useful practical bits and pieces: the shell, debugging, metaprogramming and what not. Also has [lectures on youtube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J).

Major topics:
* [[system_design]]

Cheatsheets, tips, and notes:
* [[git]] - GIT cheat sheet
* [[sql]] - SQL cheat sheet
* [[pandas]] - Pandas obviously :)

Advice on ML projects:
* [[ml_lore]] - a collection of rules of thumb about everything Deep Learning (which defaults to pick etc.)

Other #lifehack(s):
* [[Clear2019zettelkasten]] - how to properly create and support this knowledge base
* [How to do citations with Zettlr](https://docs.zettlr.com/en/academic/citations/) - #todo, this must be nice! 

# Python

Random facts and tips:
* If in a module you start a method name with one underscore, like `_helper`, this method isn't imported on `from module import *`. Unfortunately it is still accessible if you do `import module`and address it as `module._helper()`.
* **Ternary operator**: `x = 1 if condition else 2`.
* Empty arrays, strings, and dicts are False, so write `if foo:` instead of `if foo!=[]:` or `if len(foo)>0:`. It's considered good form, [apparently](https://google.github.io/styleguide/pyguide.html).
* To add += 1 to a dict when a key may not exist, use `get()` as it allows to set a default value: `a[i] = a.get(i,0)+1`
* To get some (or rather, first) key from a dict: `next(iter(a.keys()))`
* To split a long line one could use `\`, but it is advised to use implicit line joining instead, which happens if brackets were open and not closed. It's better to add "unnecessary" brackets than usie a backslash.
* **Docstring**: First constant in a declaration, starts and ends with triple double quotes `"""`, accessible via `object.__doc__` property. Minimum: one sentence, capitalized, with a full stop at the end, explaining what this function does. Don't include the name, or usage. Any other comments - lower, after an empty line. For modules, similar, at the very beginning, before any declarations. Refs: [1](https://www.python.org/dev/peps/pep-0257/), [2](https://www.pythonforbeginners.com/basics/python-docstrings)
* **F-strings**: `f"bla {x['a']:.2f}"` - this version (with `"`) supports dicts (because diff quotation marks), and formats the output (after `:`). Refs: [intro](http://zetcode.com/python/fstring/) , [specification](https://docs.python.org/3/library/string.html#format-specification-mini-language) (a mini-language of sorts!)
* Sweetest way to **iterate through a dictionary**: `for key,val in d.items():`.

**List comprehensions**
* Nested comprehensions: same syntax as in writing nested loops (even tho it looks unformulaic), e.g. `[j for i in range(5) for j in range(i)]
* If you want to sometimes return nothing, move `if` to the end: then you don't have to write `else`: `[x for x in y if a]`.

**Gotchas**
* Objects (including empty lists `[]`) should never be used as **default arguments** for functions, as they are evaluated only once per program (during object definition), not when methods are called! Insetad use `x=None`, then `if x is None: x=[]`. It sounds super-cumbersome, but that's just how it is. ([ref](https://docs.python-guide.org/writing/gotchas/))
* As strings are immutable, slicing a string creates a copy, and thus is O(N) rather than O(1). [ref](https://www.byte-by-byte.com/strings/)
* Crazy fact: a built-in `round()` (not the one from math / numpy) rounds both 1.5 and 2.5 to 2 (always **rounds edge cases towards even numbers**). Apparently that's to fight the bias of floating number representation ([ref](https://realpython.com/python-rounding/)).

**OOP**
* **Non-local variables**: when defining function within a function, we can make local variables of the outer function become sorta "global" for the inner function, which may be handy. If you only plan to read from this variable, just refer to it as if it's global. If you plan to update it, write `nonlocal var_name` inside the inner function, as if declaring it. After that it won't be masked. 
* **Closures**: Writing a function that returns another function. One way to use it: to bind data to the function, as if it was hard-coded inside the function, as an alternative to either global variables, or passing data as an argument. Have a function that receives the data, whips out a function that uses this data, and then returns the function itself. Refs: [1](http://www.trytoprogram.com/python-programming/python-closures/), [2](https://www.programiz.com/python-programming/closure)
* **Decorators**: a type of function that takes a function as an argument, writes a helper function around it, and returns this outer helper function.  May be used to wrap a closure around a function, to sorta "automate memoization": ([ref](https://www.python-course.eu/python3_memoization.php)). According to Google style code, while they may be helpful, they aren't exactly recommended, as they may complicate things.

**Refs:**
* Tips from Chip Huyen: https://github.com/chiphuyen/python-is-cool
* Nice [list of Python gotchas](https://www.toptal.com/python/top-10-mistakes-that-python-programmers-make) from Martin Chilikian

## Numpy
* For linear algebra, to start flipping vectors and use them as matrices, use `np.atleast_2d` - it turns vectors, and even scalars, into low-ranked 2D arrays. I think, rows for vectors.

## Matplotlib
* Brief intro from Brad Solomon: https://realpython.com/python-matplotlib-guide/
* Cheatsheet: [https://github.com/rougier/matplotlib-cheatsheet](<https://github.com/rougier/matplotlib-cheatsheet>)

# Good coding habits
* Keep code clean (not smelly). Types of **smells** ([ref](https://www.thoughtworks.com/insights/blog/coding-habits-data-scientists)):
    1. Dead code (commented out, inconsequential)
    2. Print statements everywhere
    3. Bad variable names
    4. Functions that do too many things instead of one thing
    5. Code repetition
    6. Magic values
* Smuggle code from Jupyter to classes as soon as possible (Jupyter only for protopying, reporting, and use case)
* Write unit tests ([link to a decent intro](https://www.freecodecamp.org/news/an-introduction-to-testing-in-python/))
* Make small and frequent commits

Some more Python-specific advice:
* Import entire modules or packages, not classes or functions, to retain `module.class.method()` structure, and prevent name collisions.
* Lines shouldn't be longer than 80 chars (Google style guide)
* `.py` files are not scripts, so even executables	 should  be written inside a main function: `def main():`. And then inside, add:
```python
if __name__ == '__main__':
    main()
```
The benefit of this construction is that this `.py` won't be auto-executed on import.

## Naming variables
* Python-specific:
    * Variables: **snake_case** underscore for variables and functions
    * Constants: all-caps
    * Objects: capitalized CamelCase
    * Local methods: leading-underscore
* Arrays are plural, elements of array are singular: for fruit in fruits
* Booleans: is, has, can (is_cat, has_hamburger, can_eat)
* Methods that return a boolean: check_is_cat()
* Numbers: n_cats, max_cats, min_cats, total_cats:
* Transformers: to_dollars, to_uppercase

Refs:
* https://hackernoon.com/the-art-of-naming-variables-52f44de00aad
* [Google style guide](https://github.com/google/styleguide), including that [for Python in particular](https://google.github.io/styleguide/pyguide.html).

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