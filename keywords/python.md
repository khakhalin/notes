# Python

Path: [[01_Tools]]

Random notes on Python.

#tools #python #coding #oop #swe`


Subtopics and related:
* [[oop]] - a entry on OOP is very Python-specific for now. Exclusively Python topics include:
    * [[decorators]] - how to write them in Python
* Common packages:
    * The holy triad: [[numpy]], [[pandas]], [[matplotlib]]
    * [[py_dates]], [[regex]] - core utils
    * [[logging]] - subj (logging)
    * [[mypy]], [[pydantic]] - typing
    * [[linting]] - common packages for Python linting
    * [[sklearn]], [[tensorflow]] - DS
    * [[flask]], [[spark]], [[kedro]] - DE
    * [[environments]] - conda, pip
    * [[jupyter]] - notebooks
* [[unit_test]] - how to write unit tests correctly (in Python) 

# Sets

**Sets:** another hashed structure with O(1) for testing `i in a`. [ref](https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset)

* Create: `set()` or a comprehension `{a for a in whatever}`, but not `{}` (this makes a dict).
* Sets are unhashable, so they cannot be keys in a dict, and sets of sets are impossible. Thre's a sister type however `frozenset([])` that creates immutable sets that are hashable, and can be used as keys.
* Element-level operations: `a.add(1)`, `a.remove(1)` (raises an error if not found), `a.discard(1)` (doesn't raise an error if not found). `a.pop()` exists (removes and returns an element), but doesn't take arguments (coz no position). And of course the crowd-pleaser `x in a`
* Set-level checks: `a.issubset(b)` (whether a is a subset of b), `a.superset(b)` (the other way around). So these two comparisons should be read in the same sequence in which they are written, as in plain English. Also `isdisjoint()`. Alternatively, just do `a < b`, `a <= b` etc.
* Set-level operations: `a.union(b)` (alternatively, `|` as an operator), and similarly `intersection` (or `&` as an operator), `difference` (or `-`), `symmetric_difference` (`^`). All these operators work as update-operators (like `|=`) as well (unless it's a frozen set of course). Another version for `a |= b` is `a.update(b)` (makes for more readable code?). Single-digit operators work only on sets, while methods accept any iterables.
* Gotcha: note that if you do `a.update('string')`, it will get disassembled into letters!
* `a = list(set(a))` - a neat way to do unique(). Doesn't preserve the original order though.

# Comprehensions

* Nested comprehensions: same syntax as in writing nested loops (even tho it looks unformulaic), e.g. `[j for i in range(5) for j in range(i)]
* Conditional comprehension: `[x for x in y if a(x)]`
* Ternary operator: `[1 if a(x) else 0 for x in y]`

Repacking:
* `[*a]` - repacks an abstract iterable into a list.
* `{**a}` - repacks an abstract mapping into a dictinary. One common non-dict mapping object that people work with is a [[pandas]] series, so `{**df.loc[0,:]}` repacks it in a dict, for example.

# Zipping

* `zip(a, b)` for two iterables `a` and `b` creates a lazy iterable of corresponding pairs, starting with `(a[0], b[0])`. To eagerly compute it, either loop through it, or take a `list()` or `tuple()`.
    * It is also possible to call a `dict()` on it, in which cases it creates a dictionary mapping elements to each other, as in `a[0]:b[0]` etc.
* A reverse operation is also a zip: if `c=zip(a,b)`, then `tuple(zip(*c))` evaluates to `(a,b)`. 
* Because tuples are sorted lexicographically, zipping objects with some numerical measure, sorting this list with `.sort`, then unzipping again, may be a decent way to sort objects.

# Strings

* **F-strings**: `f"Bla {x['a']:.2f}"` - this version (with `"`) supports dicts (because diff quotation marks), and formats the output (after `:`). Refs: [intro](http://zetcode.com/python/fstring/) , [specification](https://docs.python.org/3/library/string.html#format-specification-mini-language) (a mini-language of sorts!). Good examples of formats (only that part that goes after `:` but before closing `}`):
    * `d` - decimal, `b`- binary (transforms to binary), `x` and `X` - Hex (with small and capital letters for abcdef respectively), `f`- float (6 digits default), `e` - exponential. `g` - smart number format that tries to guess what you want. `s` - string.
    * Left, right, and center align: `<>^`
    * Examples: `10.3f` - 10 wide with three digits after point. `010d` - 10-wide and filled with zeros. `<+20,d` - left-aligned, 20 chars wide, with comma separating thousands, and a mandatory sign upfront (+ or - depending on the sign of the value). `=>20d` - 20-wide, right-aligned, and with equality signs used instead of spaces.
    * `var!r` generates a string representatoin of the `var` variable by running a `repr(var)` on it

# Random tips and Gotchas

* If in a module you start a method name with one underscore, like `_helper`, this method isn't imported on `from module import *`. Unfortunately it is still accessible if you do `import module`and address it as `module._helper()`.
* **Ternary operator**: `x = 1 if condition else 2`.
* Empty arrays, strings, and dicts are False, so write `if foo:` instead of `if foo!=[]:` or `if len(foo)>0:`. It's considered a good form ([proof](https://google.github.io/styleguide/pyguide.html)).
* To add += 1 to a dict when a key may not exist, use `get()` as it allows to set a default value: `a[i] = a.get(i,0)+1`
* To get some (or rather, first) key from a dict: `next(iter(a.keys()))`
* To split a long line one could use `\`, but it is advised to use implicit line joining instead, which happens if brackets were open and not closed. It's better to add "unnecessary" brackets than usie a backslash.
* **Docstring**: First constant in a declaration, starts and ends with triple double quotes `"""`, accessible via `object.__doc__` property. Minimum: one sentence, capitalized, with a full stop at the end, explaining what this function does. Don't include the name, or usage. Any other comments - lower, after an empty line. For modules, similar, at the very beginning, before any declarations. Refs: [1](https://www.python.org/dev/peps/pep-0257/), [2](https://www.pythonforbeginners.com/basics/python-docstrings)
* With big numbers, we can write `100_000`, and it's still a number :)
* We can do chain comparisons: `1 <= x < 3`, which is equivalent to `1<=x and x<3`.
* To check type: either `if isinstance(x, str):` or `if type(x) is str:`

How to iterate properly:
* Sweetest way to **iterate through a dictionary**: `for key,val in d.items():`.
* With iterable, but also having a counter: `for i,val in enumerate(vals):`
* A mix of two is possible: `for i,(key,val) in enumerate(d.items()):`

**Gotchas**
* Objects (including empty lists `[]`) should never be used as **default arguments** for functions, as they are evaluated and instantiated only once per program (during object definition), not when methods are called! Instead use `x=None`, then `if x is None: x=[]`. It sounds cumbersome, but that's just how it is. ([ref](https://docs.python-guide.org/writing/gotchas/))
* Similarly `[{}]*3` will actually create 3 references to the SAME dict.  To create an array of empty dicts or arrays or something else, use a list comprehension.
* Sometimes, to check if the input is empty (either `None` or `[]` ), people use `if not a` (it returns True in these cases). It's not very safe, becasue `not 0` and `not False` also return True, yet these values are certainly not empty. So better to use `if a is not None` explicitly, to compare with None, and `if len(a)==0` to check for `[]`.
* As strings are immutable, slicing a string creates a copy, and thus is O(N) rather than O(1). [ref](https://www.byte-by-byte.com/strings/)
* Crazy fact: a built-in `round()` (not the one from math / numpy) rounds both 1.5 and 2.5 to 2 (always **rounds edge cases towards even numbers**). Apparently that's to fight the bias of floating number representation ([ref](https://realpython.com/python-rounding/)).
* In Python, logic operators (and, or) are **short-circuit**, which means that `(True or None.field)` will return True, even though on its own `None.field` is obviously a mistake (Nones don't have fields.)
* **Slices** don't throw index out of bounds exceptions, but evaluate to `[]` if they are out of bounds. so for `a=[0]`, `a[1]`would give an "index out of range" mistake, but `a[1:]` gives an empty list.
* Remember that `copy()` is a shallow copy, so for complex nested structures it may not be enough.
* Lazy iterables  (such as those created by `map`and `zip`) empty as you go through them. So once you run a `list(a)` on a lazy iterator `a` it becomes empty.
* If `__contains__` method isn't implemented for a class, but `__getitem__` (getting an element by its numerical position) is implemented, `in` operator triggers iteration through the class with elementwise comparisons. It means that depending on the class `in` may have very different complexity. The main practical gotcha if of course doing `in` with a list (which is O(N) slow) instead of a set or a dict (that are O(1) fast).

**Refs:**
* Tips from Chip Huyen: https://github.com/chiphuyen/python-is-cool
* Nice [list of Python gotchas](https://www.toptal.com/python/top-10-mistakes-that-python-programmers-make) from Martin Chilikian

# Error handling

To **raise an error** do: `raise KeyError('Error Message')`, except instead of a `KeyError` there are lots of "Base error types" to try. Some useful examples include:

* `Exception`: the most basic class for all exception types
* `ArithmeticError`, including `ZeroDivisionError`, `OverflowError` ...)
* `LookupError`, including `IndexError`, `KeyError`
* `AssertionError`: when assert fails
* And many more: [see the list](https://docs.python.org/3/library/exceptions.html)

To **handle an error**, do something like this code below. This whole traceback things creates a proper stack of exception handling (see also [[logging]])
```python
a = 0
try:
    x = 1/a
except Exception as error:
    # do something clean-up
    _log.warning(str(error).strip()) # Warn about the error without breaking
    raise SomeOtherException(...).with_traceback(tb) # Or break with a different error if you want to
finally:
    # Clean-up part that is always executed
    del a
```
# With

Is typically used with contexts and sessions. The approach relies on the fact that the corresponding object has methods `__enter__` and `__exit__`: one to set it up and return the instance; the other one to gracefully clean up aftewarderds. `__exit__` doesn't delete the object, so the object can be referenced after the `with` is over. Even an empty `with` part would execute three methods on an object: `init`, `enter`, and `exit`, in this order.

Typical use case:
```python
with Thing() as t:
    t.do_something()
```
A more rare use is one without `as` statement, but it's ok for context objects. In this case `with` still runs `enter` on the context object, so you can make it `active`. Then if you operations inside the `with` block reach out for the context object in some way, they will "find it" in this active state. But it won't happen automatically, by magic: you need to write the `enter` and `exit` methods properly, and then the content of the block should be actively context-dependent, to benefit from this syntax.

Sometimes can also be seen as an alternative to `try - except - finally`.

**Refs:**
* 2006 intro: https://effbot.org/zone/python-with-statement.htm
* official description with examples: https://docs.python.org/3/reference/compound_stmts.html
* another official description one: https://www.python.org/dev/peps/pep-0343/ 
* implementing a class to support `with` statement: https://preshing.com/20110920/the-python-with-statement-by-example/

# CodeStyle

* https://hackernoon.com/the-art-of-naming-variables-52f44de00aad
* Google style guide: https://google.github.io/styleguide/pyguide.html
* PEP: https://peps.python.org/pep-0008/

### Naming

All of these advices are Python-specific:
    * Variables: **snake_case** underscore for variables and functions
    * Constants: ALLCAPS
    * Objects: capitalized CamelCase
    * Local methods: one leading-underscore (like `_add()`)
* Arrays are plural, elements of an array are singular: `for fruit in fruits`
* Methods: use verbs (set, get, move)
* Booleans: is, has, can (is_cat, has_hamburger, can_eat)
* Methods that return a boolean: check_is_cat()
* Numbers: use a numerical prefix (n_cats, max_cats, min_cats, total_cats)
* Transformations: to_dollars, to_uppercase

* Import entire modules or packages, not classes or functions, to retain `module.class.method()` structure, and prevent name collisions.
* Lines shouldn't be longer than 80 chars (Google style guide)
* `.py` files are not scripts, so even executables	 should  be written inside a main function: `def main():`. The benefit of this construction is that this `.py` won't be auto-executed on import. And to make them callable script-like from the command line, add this inside:
```python
if __name__ == '__main__':
    main()
```
* Smuggle code from Jupyter to classes as soon as possible (Jupyter only for prototyping, reporting, and use case)

**Other links**

Creating scikit-learn -like packages: https://scikit-learn.org/stable/developers/develop.html
Some interesting info on exposing parameters, careful work with random generators (to ensure replicability) and related interfaces etc.

On dangers of naming: there's a legit package called thefuck: https://pypi.org/project/thefuck/

# File structure and importing

Main idea: it seems that one can think of `import`as of just a bunch of code being literally included from a file, at the point where `import` is called. (With a caveat that it's only loaded once, so if you call it twice nothing happens, and if you change the imported file after it was imported, also nothing happens). It means that with numpy and such, we call them in the beginning of the script, for the `np` prefix to become available as a public class, but for class methods we can call it within class declaration. And we can have class methods written in separate files, if we so desire.

A corollary statement about **numpy**: it's OK (and actually proper) to have `import numpy as np` in the beginning of every source file, as only the first one will be actually "run". What IS bad however is to do `from X import *` (aka **Wild import**), as it imports everything from `X`, including all of its imports. Also, it only works at the module level.

## Scripting

Python's approach to importing means that an `import` statement technically runs each `py` file, and so a file that is written like a script (not as a bunch of function and class declarations) will be run at importing! For modules and submodules, the act of importing would run  `__init__.py` in a similar fashion.

This fact has three consequences for scripting. One, it means that writing a script like a script is dangerous, as it can be imported by mistake, and executed (repeatedly) by mistake. The only things that can be written "just so", as "no-indented commands", should be constant declarations, imports, and getting a logger handle (see [[logging]]). But none of the actual actionable code. 

Second, if we want a script work as a script, when called from a command line, we should use this trick:
```python
if __name__ == '__main__':
    some.code()
```
This part will only work if the script is really run on its own, as that's the only way to make `__name__` be equal to `__main__`.

But third, if later on you realize that you actually want to run this script from another python file, you won't be able to access this code in `__main__`. Ao an even better idea, actually, is to put all code in a function, and then call this function from `__main__`. This way, in the future, you would also be able later to import this script, and run `my_script.my_function()` on it.

Footnotes:
* https://stackoverflow.com/questions/47524054/calling-main-from-another-file-in-python

## Packages

How to **create a namespace** to compartmentalize some functions (in a style of `utils.fun()`)? Two options: a **static class** and a **Python package**.

The static class is self-explanatory: just create a `py` file, and define a class with many static functions, then import this class in the main init with `from .name import name`. But one can only go so far with this approach.

To create a **package**:
* Create a folder `package_name`. If you also have a [[git]] repo with the same name, first put a `src` subfolder in it, and then in this subfolder - `package_name` again. You end up with `mypack/src/mypack` which may look superfluous, but apparently fixes some potential issues downstream.
* In the inner `package_name` folder add a file `__init__.py` inside. That's also where all of your code.
* Optional (? 🔥) Make the `__init__.py` file contain imports of every function from each of the files (like, `from .fun import fun`), and so on.
* In the outer (repo root) `package_name` folder create files `setup.py` and `requirements.txt`. See below for format.

A minimal `setup.py` file looks like this:
```python
from setuptools import setup

setup(
   name='my_package',
   version='1.0',
   description='A useful module',
   author='Joe Doe',
   author_email='joe@foo.example',
   packages=['my_package'],
   install_requires=['pandas', 'numpy>=2.1.3'], #external dependencies
)
```

The file `requirements.txt` is very similar (🔥 not sure why there's this repetition actually?) and looks kinda like this:
```
pandas
numpy>=2.1.3
scipy==1.14.1
```

Once done with all of that, we need to set up a correct environment (see [[environments]]). There are several ways to do it.
* One, run `pip install -e .` to install this package as an editable package. From now on you'll be able to import this package by its name. And it will also install everything from `setup.py` (🔥 I believe? Not from requirements.txt?)
* Another option is to create a well-designed `environment.yml` 🔥 and then run `conda env create -f environment.yml` which will _both_ create the environment and install stuff specific from this package, and if you add a `pip` section at the end of this yaml, with `-e .` in it, then you'l also cover the point above.

Footnotes:
* https://stackoverflow.com/questions/1471994/what-is-setup-py

**Subpackages**

If you need to call one subpackage from another subpackage, it's a bit harder. There are two options here: good, and hacky.
* The good one: `from ..sub2 import smth` or `from ..sub2.smth import some_method`
* Sometimes this doesn't work however, giving an error "attempted relative import beyond top-level package". An alternative is to add the upper (`..`) folder in the path variable, by doing `import sys; sys.path.append('..')`

Now, if you need to import the entire package (parent package) from one of subpackages (as it happens when you do unit testing [[unit_test]]), and you don't want to use the "sys" hack, just make sure the package you are in has an `__init__.py` file (it may even be empty). Then do simple `from package_name import smth`. Apparently, the logic is the following: if you don't do relative import (`from .smth`), it tries the current folder, and the system folders (in some sequence? not sure what gets priority), but also it goes up from the current folder until the first folder without `__init__.py`, and tries this folder as well. Because in normally organized projects it corresponds to the top level `src` folder. And that's where `package_name` (the top level package) sits in this case.

> I am not 100% sure I got it right, and also it's quite possible that one can avoid the hacky hack described above by adding fake `__init__.py` to the subplackage folder as well. But that needs some testing. 🔥

Footnotes:
* https://stackoverflow.com/questions/47561840/python-how-can-i-separate-functions-of-class-into-multiple-files
* https://stackoverflow.com/questions/28440036/when-importing-my-class-i-lose-access-to-functions-from-other-modules
* https://stackoverflow.com/questions/25827160/importing-correctly-with-pytest
(check out last answer, about intentionally including `__init__.py`)
* https://docs.python.org/3/reference/import.html
* https://github.com/chiphuyen/python-is-cool

# Types and type-hinting

Links:
* https://kobzol.github.io/rust/python/2023/05/20/writing-python-like-its-rust.html - argues that the best strategy in reducing the number of run-time errors is to turn them into compilation-time errors. Which, in turn, is best achieved by "making eror states unrepresentable" by heavy type-hinting.

# Command line

To make scripts that are runnable from the terminal, use `import sys` and then refer to `sys.argv`. It contains a list, with the leftmost (0th) element being the name of the program, and all consecutive elements, parameters (kinda as if the command line was split by space). 

If you need something fancier (many different optional keys), use `ArgumentParser` module. It unpacks them for you, supports variable order, synonyms, auto-generates help messages and whatnot. Example use:
```python
import argparse

parser = argparse.ArgumentParser(description="Useful script")  
parser.add_argument("filename") # positional argument
parser.add_argument("-a", "--add", help="Add the next number") # option that takes a value
parser.add_argument("-b", "--block", help="Block asteroids", action='store_true') # switch
args = parser.parse_args()  
  
try:  
    value = args.add
except:   # Maybe some default behavior
    values = 100
    
if args.block:
    print('Go away, asteroids!')
```

* For value-taking keys, there's also a `default="my default value"` parameter that may be easier than a more verbose try-except block.

Refs:
* https://stackoverflow.com/questions/1009860/how-to-read-process-command-line-arguments
* https://docs.python.org/3/library/argparse.html


# General Refs

Downey, A. (2012). Think Python. " O'Reilly Media, Inc.".

Programming in Python: Fluent Python, Luciano Ramalho

Google Styleguide for Python: https://google.github.io/styleguide/pyguide.html