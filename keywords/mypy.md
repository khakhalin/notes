# Type-hinting and -checking in Python

Parent: [[python]]
See also: [[pydantic]], [[linting]]

#python


As of Python 3.5, one can add type hints when setting variables and defining functions, and then potentially check the consistency of these hints with a special static checker (`mypy`, see below) before script execution. Some of this type hinting is now built-in basic Python, others needs to be imported via `from typing import`. Let's start with some basic functionality. You can do:
```python
age: int = 1 # typing at first assignment 
height: int  # if declared by not assigned (e.g. class properties)

def func(x: str) -> int:
	return 0
```
Built-in types: `int`, `float`, `bool`, `str`, `bytes`

To provide typing to iterables and maps (starting Python 3.9) we do
* `x: list[int] = [1]` for lists
* `x: set[int] = {1}` for sets
*  `x: dict[str, int] = {'word': 0}` (key, val) for dictionaries
*  `x: tuple[int, float, str] = ...` for tuples (explicitly listing types of components).
*  There are also tuples of variable size (that I don't understand for now)
(Note that from python 3.5 to 3.8, the syntax was different)

Fancier stuff is typed via special objects (that need to be imported from `Typing`):
* `f: Iterator[int]` - an iterator that yields integers
* `f: Iterable[int]` - a list-like object (duck-typed) that supports `len()` and `__getitem__`, but not necessarily a list.
* `Mapping[int, str]` (or any other types in the brackets) - a duck-typed dict-like object that supports `getitem` and `setitem`, but not necessarily a pure dict.
* `f: Callable[[int]]` - a function that takes a single `int` argument. (🔥 Not sure if the syntax is right: the manual says it is, but my interpreter seems to expect `[[int]], NoReturn` or something like that?)
* `IO[str]` - for functions that work with objects created via `open()` call (not sure how it works, but ok)

## Typing and OOP

For custom (user-created) classes, just use the name of the class there, so in theory it should be really simple: `f(x: MyClass) -> MyOtherClass`.

Subclasses should obviously be accepted in place of a base classn, reflecting the basic principle of [[oop]] that every derivative class is supposed to satisfy all properties of the base class). So if your class is inheriting to some base class (say, `int`), and you use it with some call that expects an `int`, it will be alright. When declaring classes that are based on your custom classes however, you have to explicity declare them as generic. All built-in classes are generic, but by default your custom classes seem to be non-generic (🔥 is is true?). So instead of a normal Pythonic way of doing
`class NewClass(): ...` which I think defaults to `NewClass(object)`
you need to do this? (🔥 is it true? I'm still confused about this)
`class NewClass(Generic[MyClass]): ...`

Alternative explanation: `Generic[int]` should be thought of as an extension of `list[int]` or `Iterator[int]`, but to a more general behavior, manipulating values of `int` one way or another. It may be a heap, a stack, anything; what matters is that it takes some number of values of the same type (`int` in this case) and does something with them. It is also possible to define a Generic above an arbitrary, and not yet defined class using `TypeVar` (see below)

The problem is both classes should be defined before `f()` is defined, which is fine if they are imported from custom packages, but may be weird within a package.
* To fight this, one can put class name in quotation marks (provide them as strings), and then apparently Mypy is happy (see below).
* Another approach is to add this line to the header: `from __future__ import annotations`. Then apparently everything works, but allegedly with potential bad side-effects for other packages. When this line is included, all (?) references to classes are turned into strings (so it's like a massive automated version of the previously described solution, to provides class names as strings). And this can break other libraries ([[pydantic]], if I'm not mistaken?))
* When working with packages / files that describe interacting objects you may also run into a problem of **circular importing**, when you need to import `a` to `b` to define a type of output in `b`, but also the other way around. But circular importing is disallowed in Python. One way to solve it (short of rearranging the code, as one has to do in case of real circular referencing) is to use `if TYPE_CHECKING`:
```python
from typing import TYPE_CHECKING
if TYPE_CHECKING:
  from a import atype
```
And then proceed defining `btype`. In this case `a.py` can import `btype` normally.

There's some complex story about class variables (they have their own declaration, e.g. `ClassVar[int]`), and about a hack around that is neccessary to access and change class variables dynamically after all (to do it, you have to override the setter and the getter). But I'll assume it's a rather rare case (I dislike class variables), so let's skip it for now.

> Would it be true to say that from the theory POV class variables should be static, but Python makes them dynamic, so Typing makes them static, while still leaving a hacky workaround to have some of them dynamic after all, to cover for special cases?

Handing [[decorators]] is weird; see below.

# Fancier types

It seems that originally most of these developments were a part of `typing` package, but then some of them migrated to main Python. You still need to import `typing` (or rather, individual methods and objects from `typing`) for fancier things. For example:

All of the above types disqualify `None` as a value, as it doesn't belong to corresponding types! Which would disrupt coding habits that are based on `None` as a default value. To allow `None`, instead of `x: str` (or similar) do `x: Optional[str]` (possible after doing `from typing import Optional`)

To make a variable accept either of several classes, do
```python
from typing import Union
x: int | str # In Python 10+, or:
x: Union[int, str] # Before Python 10
```

If you plan to produce differently typed outputs for differently typed inputs (and not just a permissive set of inputs to one output, or a permissive "everything to everything"), use polymorfism:
```python
@overload
def func(x: int) -> int:
	...

@overload
def func(x: list[int]) -> list[int]:
	...

def func(x: int | list[int]) -> int | list[int]:
	if ininstance(x, int):
    	bla bla
```

Here the first two declarations (literally with these three dots `...` instead of a real declaration!!) are there only for typing purposes. Their existence prohibits `int -> list[int]` transformation.

For a very specific case of a "Function that never returns anything" (not in the sence of returning `None`, but really and trully never returning, like a function that raises an exception) there's a very special thing `typing.NoReturn` that can be used in function definition (`… -> NoReturn`).

If you don't provide a type to a function agrument, it can take a value of any type, and it will remain unchecked. But you can also use a fake class `Any` that accepts any type.

`NewType('NameOfMyNewClass', NameOfMyBaseClass)` creates (or rather, registers with `mypy`) a subclass (subtype) for a certain base class. This is an alternative (for type-checking purposes) to actually defining a new class in the code. But it can't be used at runtime. I have no idea why this can be useful in any form tbh 🤔

## TypeVar

`T = TypeVar('T')` defines a type for type-checking purposes (not at runtime), but in a different sense: it is an alias that can be be referenced in typing expressons further in the code. After defiing a type like that, one can do `-> T` for function outputs, or `x: list[T]` for inputs, lists. 

One possible use for that is just aliasing a longer sequence with a shorter sequence. Say, instead of writing `Union(int, float)` every time you can do `T = TypeVar('T', int|float)`, and then just write this one letter `T` everywhere.

> 🔥 This does not seem to be quite true, as for example one can't just write a function that receives no inputs, but outputs a value of T type; mypy complaints that a function returning T should get at least one argument of T type. Which seems to be some real actual design choice for this use case that I don't understand. For some reason you can write `def f() -> dict[T]`, but you can't write `def f() -> T` as mypy claims it's illegal. At the same time, functions that both receive and produce T are legal; and so are functions that receive T and produce something else (or nothing).

Another possible use, to indicate that while two thingies can work with any types of variables, they should coordinate this choice with each other. For example, you may be trying to define a class `Heap` that will operate a bunch of objects, so you want to inherit to `Generic`. But `Generic` of what? Just create a placeholder undefined type: and repeat it where needed, idicating that these methods should coordinate, even though the exact value of this typevar isn't yet known:
```
T = TypeVar('T')
class Heap(Generic[T]):
  items: list[T]

def swap(self, item: T) -> T:  # Not the most realistic example
  return ...
```
The eact value of `T` in this case will be "set" at initialization, either directly: 
`heap = Heap[int]()`
or indirectly (by inference from value) if the constructor supports arguments (probably also of class `T`)

It is possible to restrict values for a new typevariable:
* `T = TypeVar('T', bound=str)` - a subtype of string
* `T = TypeVar('T', str, int)` - has to be _either_ a subtype of string or of int.
* `T = TypeVar('T', str|int)` - a shortcut for `Union[str, int]`. It is very similar to "either str or int", but a bit more permissive, and in this case probably better. (A variable that is "either a string or int" is technically not a string, and not an int. Just use this syntax, ok?)

To define classes on a generic type variable

## Decorators

Handling [[decorators]] is a bit tricky. Here's what you do for a function decorator (with actual function definitions omitted):
```python
from typing import Any, Callable, TypeVar
F = TypeVar('F', bound=Callable[..., Any])
def bare_decorator(func: F) -> F:
def decorator_args(url: str) -> Callable[[F], F]:
```
🔥 _What does it mean?.._

Finally, `typing` contains a functionality `Annotated` that allows to extend variable metadata beyond just its having a named type. For example, `x: Annotated[str, metadata]` where `metadata` may be a string comment, a particular object that allows to set multiple parameters for this variable, etc. It is not used directly by normies, for the purposes of type-checking, but it is used by other libraries, for example, by FastAPI (to annotate connection properties?), strawberry (for some graph stuff), and most critically in practice, by [[pydantic]], (for additional data validation)

# Mypy package

#testing

None of these declarations do anything on their own; they don't affect run-time, and they are not checked in any way by Python itself. Where they become useful is if you do some checks, linting-style, before running the script. `mypy` is the package that _actually_ checks the consistency of our variable use, by doing some inference about what they _could be_ (that is, not in run-time)

To run it, you do `mypy project_name` in the terminal (where `project_name` is the name of a folder with your project). When standing inside the folder, do `mypy .`, or, more likely, `mypy src` as typically tests are not covered by mypy.

If something isn't perfect, there are several ways to make mypy ignore stuff:
* To inform mypy about some implicit type transformation, use a helper function `b = typing.cast(type1, a)`. It does not actualy do anything!! No actual transformation, just hinting to mypy that the type should have been changed.
* To make it completely ignore a row (say, function call to an unmarked function), add a hinting comment after this row: `# type: ignore`. You can also add another comment after this comment (gosh).
* Finally, you can exclude a whole block of code from these checks by importing `TYPE_CHECKING` constant from `typing`, and then do `if not TYPE_CHECKING`.

# Refs

https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html - basics

https://mypy.readthedocs.io/en/stable/generics.html - about generic types and decorators

https://filipgeppert.com/mypy-advanced-examples.html - some examples on generics (although it avoids mentinioning the extra limitations)