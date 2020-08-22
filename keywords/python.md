# Python

#tools #coding

Path: [[01_Tools]]
See also: [[anaconda]], [[pandas]], [[tensorflow]]

# Random tips

* If in a module you start a method name with one underscore, like `_helper`, this method isn't imported on `from module import *`. Unfortunately it is still accessible if you do `import module`and address it as `module._helper()`.
* **Ternary operator**: `x = 1 if condition else 2`.
* Empty arrays, strings, and dicts are False, so write `if foo:` instead of `if foo!=[]:` or `if len(foo)>0:`. It's considered a good form ([proof](https://google.github.io/styleguide/pyguide.html)).
* To add += 1 to a dict when a key may not exist, use `get()` as it allows to set a default value: `a[i] = a.get(i,0)+1`
* To get some (or rather, first) key from a dict: `next(iter(a.keys()))`
* To split a long line one could use `\`, but it is advised to use implicit line joining instead, which happens if brackets were open and not closed. It's better to add "unnecessary" brackets than usie a backslash.
* **Docstring**: First constant in a declaration, starts and ends with triple double quotes `"""`, accessible via `object.__doc__` property. Minimum: one sentence, capitalized, with a full stop at the end, explaining what this function does. Don't include the name, or usage. Any other comments - lower, after an empty line. For modules, similar, at the very beginning, before any declarations. Refs: [1](https://www.python.org/dev/peps/pep-0257/), [2](https://www.pythonforbeginners.com/basics/python-docstrings)
* Sweetest way to **iterate through a dictionary**: `for key,val in d.items():`.
* With big numbers, we can write `100_000`, and it's still a number :)
* We can do chain comparisons: `1 <= x < 3`, which is equivalent to `1<=x and x<3`.

# Sets

**Sets:** another hashed structure with O(1) for testing `i in a`. [ref](https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset)

* Create: `set()` or a comprehension `{a for a in whatever}`, but not `{}` (this makes a dict).
* Sets are unhashable, so they cannot be keys in a dict, and sets of sets are impossible. Thre's a sister type however `frozenset([])` that creates immutable sets that are hashable, and can be used as keys.
* Element-level operations: `a.add(1)`, `a.remove(1)` (raises an error if not found), `a.discard(1)` (doesn't raise an error if not found). `a.pop()` exists (removes and returns an element), but doesn't take arguments (coz no position). And of course the crowd-pleaser `x in a`
* Set-level checks: `a.issubset(b)` (whether a is a subset of b), `a.superset(b)` (the other way around). So these two comparisons should be read in the same sequence in which they are written, as in plain English. Also `isdisjoint()`. Alternatively, just do `a < b`, `a <= b` etc.
* Set-level operations: `a.union(b)` (alternatively, `|` as an operator), and similarly `intersection` (or `&` as an operator), `difference` (or `-`), `symmetric_difference` (`^`). All these operators work as update-operators (like `|=`) as well (unless it's a frozen set of course). Another version for `a |= b` is `a.update(b)` (makes for more readable code?). Single-digit operators work only on sets, while methods accept any iterables.
* Gotcha: note that if you do `a.update('string')`, it will get disassembled into letters!
* `a = list(set(a))` - a neat way to do unique(). Doesn't preserve the original order though.

# List comprehensions

* Nested comprehensions: same syntax as in writing nested loops (even tho it looks unformulaic), e.g. `[j for i in range(5) for j in range(i)]
* You can use ternary operator in LC: `[1 if i==0 else 0 for i in a]`. But if you want to sometimes return nothing, move `if` to the end: then you don't have to write `else`: `[x for x in y if a]`.

# Strings

* **F-strings**: `f"Bla {x['a']:.2f}"` - this version (with `"`) supports dicts (because diff quotation marks), and formats the output (after `:`). Refs: [intro](http://zetcode.com/python/fstring/) , [specification](https://docs.python.org/3/library/string.html#format-specification-mini-language) (a mini-language of sorts!). Good examples of formats (only that part that goes after `:` but before closing `}`):
    * `d` - decimal, `b`- binary (transforms to binary), `x` and `X` - Hex (with small and capital letters for abcdef respectively), `f`- float (6 digits default), `e` - exponential. `g` - smart number format that tries to guess what you want. `s` - string.
    * Left, right, and center align: `<>^`
    * Examples: `10.3f` - 10 wide with three digits after point. `010d` - 10-wide and filled with zeros. `<+20,d` - left-aligned, 20 chars wide, with comma separating thousands, and a mandatory sign upfront (+ or - depending on the sign of the value). `=>20d` - 20-wide, right-aligned, and with equality signs used instead of spaces.

# Gotchas

* Objects (including empty lists `[]`) should never be used as **default arguments** for functions, as they are evaluated only once per program (during object definition), not when methods are called! Insetad use `x=None`, then `if x is None: x=[]`. It sounds super-cumbersome, but that's just how it is. ([ref](https://docs.python-guide.org/writing/gotchas/))
* Similarly `[{}]*3` will actually create 3 references to the SAME dict.  To create an array of empty dicts or arrays or something else, use a list comprehension.
* As strings are immutable, slicing a string creates a copy, and thus is O(N) rather than O(1). [ref](https://www.byte-by-byte.com/strings/)
* Crazy fact: a built-in `round()` (not the one from math / numpy) rounds both 1.5 and 2.5 to 2 (always **rounds edge cases towards even numbers**). Apparently that's to fight the bias of floating number representation ([ref](https://realpython.com/python-rounding/)).
* In Python, logic operators (and, or) are **short-circuit**, which means that `(True or None.field)` will return True, even though on its own `None.field` is obviously a mistake (Nones don't have fields.)
* **Slices** don't throw index out of bounds exceptions, but evaluate to `[]` if they are out of bounds. so for `a=[0]`, `a[1]`would give an "index out of range" mistake, but `a[1:]` gives an empty list.
* Remember that `copy()` is a shallow copy, so for complex nested structures it may not be enough.
* If `__contains__` method isn't implemented for a class, but `__getitem__` (getting an element by its numerical position) is implemented, `in` operator triggers iteration through the class with elementwise comparisons. It means that depending on the class `in` may have very different complexity. The main practical gotha if of course doing `in` a list (which is slow) instead of a set or a dict (that are fast).

**Refs:**
* Tips from Chip Huyen: https://github.com/chiphuyen/python-is-cool
* Nice [list of Python gotchas](https://www.toptal.com/python/top-10-mistakes-that-python-programmers-make) from Martin Chilikian

# Errors

To **raise an error** do: `raise KeyError('Error Message')`, except instead of a `KeyError` there are lots of "Base error types" to try. Some useful examples include:

* `Exception`: the most basic class for all exception types
* `ArithmeticError`, including `ZeroDivisionError`, `OverflowError` ...)
* `LookupError`, including `IndexError`, `KeyError`
* `AssertionError`: when assert fails
* And many more: [see the list](https://docs.python.org/3/library/exceptions.html)

To **handle an error**, do something like this code below. This whole traceback things creates a proper stack of exception handling.
```python
a = 0
try:
    x = 1/a
except KeyError:
    # do something
    tb = sys.exc_info()[2]
    raise SomeOtherException(...).with_traceback(tb)
finally:
    # Clean-up part that is always executed
    del a
```

# With

An alternative to `try - except - finally`.	 Relies on the fact that many objects are shipped with methods `__enter__` and `__exit__`: one to create an object, set it up, and return the instance; the other one to gracefully clean up after the sensitive operation is performed. `__exit__` typically (or always?) doesn't delete the object, so the object can be referenced after the "with construction" is over. Even if the "with" part itself is empty, by that time three methods would have been executed: init, enter, and exit, in this order.

In practice, I most often see it used with files, and deep learning objects (TF and Pytorch).

Typical use case:
```python
with Thing() as t:
    t.do_something()
```

**Refs:**
* 2006 intro: https://effbot.org/zone/python-with-statement.htm
* official description with examples: https://docs.python.org/3/reference/compound_stmts.html
* another official description one: https://www.python.org/dev/peps/pep-0343/ 
* implementing a class to support `with` statement: https://preshing.com/20110920/the-python-with-statement-by-example/

# OOP in Python

**Inheritance** and **mixins** are fine. Use `super().method` or `super(className, self)` to invoke methods of a parent class. These two notations above are technically synonymous, so `super()` is preferred, as it's simpler. Note that it's not `self.super()`, bu just `super()`, as it's not strictly speaking a method of a class. https://realpython.com/python-super/

There's an agrument that composition (having a hierarchy of types of classes) is eaiser to maintain than class inheritance. Ref: https://realpython.com/inheritance-composition-python/

**Non-local variables**: when defining function within a function, we can make local variables of the outer function become sorta "global" for the inner function, which may be handy. If you only plan to read from this variable, just refer to it as if it's global. If you plan to update it, write `nonlocal var_name` inside the inner function, as if declaring it. After that it won't be masked. 

**Closures**: Writing a function that returns another function. One way to use it: to bind data to the function, as if it was hard-coded inside the function, as an alternative to either global variables, or passing data as an argument. Have a function that receives the data, whips out a function that uses this data, and then returns the function itself. Refs: [1](http://www.trytoprogram.com/python-programming/python-closures/), [2](https://www.programiz.com/python-programming/closure)

**Decorators**: a type of a function that acts as a meta-function: takes another function as an argument, writes a helper function around it, and returns this outer helper function. May for example be used to wrap a closure around a function, to sorta "automate memoization": ([ref](https://www.python-course.eu/python3_memoization.php)). According to Google style code, while they may be helpful, they aren't exactly recommended, as they may complicate things.

# Style habits

* Import entire modules or packages, not classes or functions, to retain `module.class.method()` structure, and prevent name collisions.
* Lines shouldn't be longer than 80 chars (Google style guide)
* `.py` files are not scripts, so even executables	 should  be written inside a main function: `def main():`. The benefit of this construction is that this `.py` won't be auto-executed on import. And to make them collable script-like from the command line, add this inside:
```python
if __name__ == '__main__':
    main()
```
* Smuggle code from Jupyter to classes as soon as possible (Jupyter only for protopying, reporting, and use case)

## Naming variables

* Python-specific:
    * Variables: **snake_case** underscore for variables and functions
    * Constants: ALLCAPS
    * Objects: capitalized CamelCase
    * Local methods: one leading-underscore (like `_add()`)
* Arrays are plural, elements of an array are singular: for fruit in fruits
* Methods: use verbs (set, get, move)
* Booleans: is, has, can (is_cat, has_hamburger, can_eat)
* Methods that return a boolean: check_is_cat()
* Numbers: use a numerical prefix (n_cats, max_cats, min_cats, total_cats)
* Transformations: to_dollars, to_uppercase

Refs:
* https://hackernoon.com/the-art-of-naming-variables-52f44de00aad
* [Google style guide](https://github.com/google/styleguide), including that [for Python in particular](https://google.github.io/styleguide/pyguide.html)

# List of special methods for pythonic objects

**General:**
* `__new__`:
* `__init__`: create an object
* `__del__`
* `__enter__`: create a risky temp object and return the instance, works with `with`
* `__exit__`: clean-up after `with`
* `__str__`: turn into a nice string, works with `print`. Human-oriented.
* `__repr__`: is supposed to return a python command (string) that, if evaluated, reconstructs the original object. A serialization of sorts. If `str` isn't defined, `print` defaults to it as well. In formatted strings, is triggered by `%r` command.
* `__format__`
* `__bytes__`

**Collection-like behaviors:** (all these shoudl still have `__` around them; I just got lazy and decided not to write them. But actually they are there!)
* `len`: length
* `next`
* `iter`
* `reversed`
* `getitem (self, position)`: works with `[]`, slices, iteration, `in`
* `setitem`
* `delitem`
* `contains`: if there's a fast way to check for the presence of an element (otherwise defaults to iteration)

**Conversion to numbers:**
* `abs`: absolute value or something like it?
* `bool`: if someone decides to run `if A` on our object.
* `int`
* `float`
* `hash`
* `index`
* `complex`

**Math-like behaviors:**
* `add (self, other)`: for `+`
* `mul (self, other)`: for  multiplying. Note that `other` may belong to a different type if you so desire (for example, for vectors, we may want to multiply by a scalar, but not use `*` for any sort of vector multiplication).
* `rmul (self, other)`:
* HERE I GAVE UP TEMPORARILY #halfthere
* `eq`, `ne`, `lt`, `le`, `gt`, `ge`: for all =, ≠, <, ≤, >, ≥ operations. If only some of these are given, Python tries to improvise (say, combine le from eq and lt), or gt from eq and lt.

**Callables**
* `call`

**Attribute management:**
* `getattr`
* `getattribute`
* `setattr`
* `delattr`
* `dir`
* `get`
* `set`
* `delete`

# General Refs

Downey, A. (2012). Think Python. " O'Reilly Media, Inc.".

Programming in Python: Fluent Python,  Luciano Ramalho.