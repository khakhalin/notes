# Type-hinting and -checking in Python

Parent: [[python]], linting
See also:

#python


As of Python 3.5, one can add type hints when setting variables and defining functions:
```python
age: int = 1 # typing at first assignment 
height: int  # if declared by not assigned (e.g. class properties)

def func(x: str) -> int:
	return 0
```
Built-in types: `int`, `float`, `bool`, `str`, `bytes`

To provide typing to iterables (starting Python 3.9) we do
* `x: list[int] = [1]` for lists
* `x: set[int] = {1}` for sets
*  `x: dict[str, int] = {'word': 0}` (key, val) for dictionaries
*  `x: tuple[int, float, str] = ...` for tuples (explicitly listing types of components).
*  There are also tuples of variable size (that I don't understand for now)
(Note that from python 3.5 to 3.8, the syntax was different)

Fancier stuff is typed via special objects:
* `f: Collable[int]` - a function that takes a single `int` argument
* `f: Iterator[int]` - an iterator that yields integers
* `f: Iterable[int]` - a list-like object (duck-typed) that supports `len()` and `__getitem__`, but not necessarily a list.
* `Mapping[int, str]` (or any other types in the brackets) - a dict-like object that supports `getitem` and `setitem`, but not necessarily a pure dict.
* `IO[str]` - for functions that work with objects created via `open()` call (not sure how it works, but ok)

For custom (user-created) classes, just use the name of the class there, so in theory it should be really simple: `f(x: MyClass) -> MyOtherClass`. The problem is both classes should be defined before `f()` is defined, which is fine if they are imported from custom packages, but may be weird within a package. To fight this one should run an extra row (really?) `from __future__ import annotations`, and then apparently everything works. Alternatively, one can put class name in quotation marks, and then apparently it also works.

# Typing package

It seems that originally most of these developments were a part of `typing` package, but then some of them migrated to main Python. You still need to import `typing` (or rather, individual methods and objects from `typing`) for fancier things. For example:

All of the above types disqualify `None` as a value, as it doesn't belong to corresponding types! Which may disrupt coding habits that are based on `None` as a default value. To allow `None`, instead of `x: str` (or similar) do `x: Optional[str]` (possible after doing `from typing import Optional`)

To make a variable accept either of several classes, do
```python
from typing import Union
x: int | str # At Python 10+
x: Union[int, str] # Before Python 10
```

If you don't provide a type to a function agrument, it can take a value of any type, and it will remain unchecked. Not sure it is true for variables though (🔥?). But you can also use a fake class `Any` that accepts any type.

# Mypy package

None of these declarations do anything on their own; they don't affect run-time, and they are not checked in any way by Python itself. Where they become useful is if you do some checks, linting-style, before running the script. `Mypy` is the package that _actually_ checks the consistency of our variable use, by doing some inference about what they _could be_ (that is, not in run-time)

... 🔥 

To make mypy ignore stuff:
* To inform mypy about some implicit type transformation, use a helper function `b = typing.cast(type1, a)`. It does not actualy do anything!! No actual transformation, just hinting to mypy that the type should have been changed.
* To make it completely ignore a row (say, function call to an unmarked function), add a hinting comment after this row: `# type: ignore`. You can also add another comment after this comment (gosh).
* Finally, you can exclude a whole block of code from these checks by importing `TYPE_CHECKING` constant from `typing`, and then do `if not TYPE_CHECKING`.

# Refs

https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html