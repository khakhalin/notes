# Object-Oriented Programming

Parent: [[01_Tools]]
Related:  [[python]], [[dictionary]]

Subtopics:
* [[design_pattern]] - best practices
* [[solid]] - core principles
* [[unit_test]] - testing-driven development

#oop


# General terminology of OOP

Important terms, ordered thematically, not alphabetically:
* Dog **extends** Animal - Dog is a derived class from Animal, so any dog is a Dog, but also an Animal.
* Dog **implements** Friend - Dog class is not derived from Friend, but supports all interfaces from Friend.	 Good for interfaces.
* **Abstract class** - a class that shouldn't (and ideally couldn't) be instantiated. Only inherited. Mixins, interfaces.
* **Interface** - describes what methods will be, but not how they will be used.
* **Mixin** - an interface with behavior. To mix-in a bunch of methods from a 2nd class.
* **Duck typing** - used in Python. Classes with matching method names can be used interchangeably, even if they don't fomally have shared inheritance.
* **Instance method** - a method that requires a class to be instantiated, like `self.do()`.
* **Static method** - a method that is just included in the class hierarchy, but doesn't need an instance. Good for utility functions.

# Specifics of OOP in Python

**Non-local variables**: when defining function within a function, we can make local variables of the outer function become sorta "global" for the inner function, which may be handy. If you only plan to read from this variable, just refer to it as if it's global. If you plan to update it, write `nonlocal var_name` inside the inner function, as if declaring it. After that it won't be masked.
🔥  Is it actually just a naive way to describe some of the "smarter" concepts listed below?

Apparently functions can have properties (as if they were in class), but that's weird, and almost always  not recommended: https://sethdandridge.com/blog/assigning-attributes-to-python-functionss
🔥  Is it actually just a naive way to describe some of the "smarter" concepts listed below?

To create a **static method**, use `@staticmethod` magic immediately before declaration (before the `def` line). They obviously don't take `self` as the first argument. As "staticness" of methods is implicit in Python (via either using or not using `self` as the first argument), the `@` decorator is entirely about communicating your intent, not about changing the way the method behaves. _Is it true?_

On top of static and instance methods, there also exist **class methods** that use a magic decorator `@classmethod`. Instead of accepting `self` as a first parameter, these methods take `cls` (a pointer to a class). They cannot modify instance state (there's no instance), but have access to all class methods via `cls.blabla`, and  can modify the class (_I've no idea what it means_). One pattern of use: for factories (`Npc.demon()` may call `Npc.__init__` with some good set of parameters that creates a demon).

All other methods (that don't have decorators) are considered instance methods (that's a default).
Refs: https://realpython.com/instance-class-and-static-methods-demystified/

**Inheritances** and **mixins** are easy: just make the class inherit to several classes, like `class Child(Parent1, Mixin1)` etc. Note that it makes Python different from, say, original Java, that didn't have mixins (but it kinda does have now apparently, called "default methods on interfaces") .
Footnotes: https://stackoverflow.com/questions/918380/abstract-classes-vs-interfaces-vs-mixins

If both parent classes have the same method (aka **Deadly Diamond of Death**), the order in which parent classes are listed becomes the order in which they are investigated. Footenote: https://en.wikipedia.org/wiki/Multiple_inheritance#The_diamond_problem

To **invoke methods of a parent class**, use `super().method` or `super(className, self).method`. These two notations above are technically synonymous, but `super()` is of course preferred, as it's simpler. Note that it's not `self.super()`, but just `super()`, as it's not a method of a class. With this, we have two type of **overloading**: we can replace prototypical method with a new one, or we can execute something on top of it (first call `super()`, then do something else as well).
Refs: https://realpython.com/python-super/

**Abstract classes** can be created using magic `@abstractmethod` from `abc` package.

There's an argument that **composition** (having a larger object contain instances of smaller objects) is easier to maintain in Python than class inheritance. That's because Java-style inheritance may conceptually clash with Pythonic methods that are implicitly inherited by every Pythonic object (inherited from `Object`), and that become even more prominent if you try to use Pythonic patterns of programming, like iterators.
Footnote:  https://realpython.com/inheritance-composition-python/

**Closures**: Writing a function that returns another function. One way to use it: to bind data to the function, as if this data was hard-coded inside the function, as an alternative to either global variables, or passing data as an argument every time. Have a function that receives the data, whips out a function that uses this data, and then returns the function itself. Refs: [1](http://www.trytoprogram.com/python-programming/python-closures/), [2](https://www.programiz.com/python-programming/closure)

## Decorators

Decorators are a type of a function that acts as a meta-function: takes another function as an argument, writes a helper function around it, and returns this outer helper function. Can, for example, be used to wrap a closure around a function, to sorta "automate memoization". According to Google style code, while they may be helpful, they aren't exactly recommended, as they tend to complicate things.

Footnote:
* https://www.python-course.eu/python3_memoization.php

## Getters and setters

The idea is to protect object properties, making them private, and channeling all outer-world interactions with them through special methods: a getter (to get the property), and a setter. The benefit here is that you can add all sorts of safety checks when the property is both assigned and returned.

In some languages, like Java, these functions are typically called `get_x`. In Python, it's recommended to either keep properties public (if no checks are needed), or use special magic decorators to make them look public, while actually being accessed through a getter/setter. Note, that with the code below,  it's preferable to use `self.x` and not `self._x` in the class itself: even though the private property exists, it's safer to always keep the check in place instead of bypassing them randomly.

```python
@property
def x(self):
    return self._x

@x.setter
def x(self,x):
    if x<0: self._x = 0
    else    self._x = x
```

A nice thing is that the simple (public attribute) and the complex (pythonic setter/getter) behaviors have the same external interface (`.x`), and so we can always start simple, then go fancy, with zero external refactoring.

Note also that it introduces a distinction between terms "property" and "attribute". An **attribute** is object-bound data, while a **property** is a method to access this data. Also, a property doesn't have to match the name of the attribute, and may be synthetic (combine values of several attribtes into one output).

Footnotes:
* https://www.python-course.eu/python3_properties.php

## List of special methods for pythonic objects

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

**Collection-like behaviors:** (all these should still have `__` around them; I just got lazy and decided not to write them. But actually they are there!)
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