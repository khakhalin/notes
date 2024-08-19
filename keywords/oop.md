# Object-Oriented Programming

Parent: [[01_Tools]]
Related:  [[python]], [[dictionary]]

Subtopics:
* [[solid]] - core principles
* [[design_pattern]] - best practices
* [[decorators]] - in Python specifically
* [[pydantic]] - a popular library for enforcing data structures in Python
* [[unit_test]] - testing-driven development

#oop


# General terminology

Important terms, ordered thematically, not alphabetically:
* Dog **extends** Animal - Dog class is derived from Animal class, so every dog is a Dog, but also an Animal.
* Dog **implements** Friend - Dog class is not derived from Friend, but supports all interfaces from Friend. In this case Friend is probably an interface
* **Interface** - describes what methods the class will need to support, but not how they are implemented
* **Abstract class** - a class that shouldn't (and ideally couldn't) be instantiated, only inherited. Mixins, interfaces.
* **abstract method** - a method that is "promised" in a prototype class (abstract class), but is not actually impelemented. Yet any object inheriting to this class needs to implement it to pass.
* **Mixin** - A way to mix-in a bunch of methods to your class; class inheritance hierarchy is no longer a tree.
* **Duck typing** - used in Python. Classes with matching method names can be used interchangeably, even if they don't fomally inherit to the same shared inheritance (if it walks like a duck, and talks like a duck...)
* **Instance method** - a method that requires a class to be instantiated, like `self.do()`.
* **Static method** - a method that is supported by a class, but doesn't refer to a specific instance. Good for utility functions.

# OOP in Python: Technical

Methods without special decorators are considered instance methods (that's the default).

Magics (to be placed immediately before method declaration; before the `def` line):

`@staticmethod` - To create a **static method**. Static methods don't take `self` as the first argument (they don't refer to an instance). Essentially, it's a stand-alone utility function that you simply don't want to become a "global variable", but that want to keep bound to a class.

This decorator is built-in, and it makes the code clearer + ensures we are never tempted to call it on instnaces with cryptic error messages. Without a decorator, some calls of this method from an instance may fail, but only because of a different number of arguments passed to the method (it would be `self` + other arguments, when called from an instance). The error message will be unpredictable and confusing. What's almost worse, some calls may formally succeed, with `self` just becoming the 1st argument and offsetting all other arguments by one (if some of them are optional, and the number of arguments is variable). This would lead to a nightmare situation, with the same method failing sometimes, but not always. To make it clear and reliable, let's decorate it with `@staticmethod` - then it will honestly break with `AttributeError` at any attempt to call it for an instant, which is infinitely better for debugging. 

On top of static and instance methods, there are also **class methods** (they use a magic decorator `@classmethod`). Instead of accepting `self` as the first parameter, these methods take `cls` (a pointer to a class). They cannot modify instance state (as they don't have access to an instance), but they do have access to static and class methods via `cls.blabla`, and they can modify class variables (if there are any). Why are they useful? One common pattern is for creating Factories (see [[design_pattern]]) by writing custom init methods. (`Npc.demon()` and `Npc.angel()` may both internally call `Npc.__init__()` with some good sets of parameters, to create different NPCs, and that's clearly a method to be called on a class, not on some instance of this class). Same as above, the decorator is even more useful here as it will prevent calling these methods on an instance, _despite_ the same number of arguments.

`@abstractmethod` - to promise a method in the abstract class, and require all child classes to implement it. It needs to be imported from `abc`.

**Inheritances** and **mixins** are easy: just make the class inherit to several classes, like `class Child(Parent1, Mixin1)`. Apparently having easily available mix-ins is not a given for an OOP language (original Java didn't have them.

If both parent classes have the same method (a situation known as the **Deadly Diamond of Death**), the order in which parent classes are listed becomes the order in which they are investigated. This sequence can be seen by looking into a built-in `__mro__` property of the class (not the instance of it, but the class itself).
Footnote: https://en.wikipedia.org/wiki/Multiple_inheritance#The_diamond_problem

To **explicitly invoke methods of a parent class**, use `super().method` or `super(className, self).method`. These two notations above are technically synonymous, but `super()` is preferred, as it's simpler. Note that it's not `self.super()`, but just `super()`, as `super` is not an instance method. With this, Python has two types of **overloading**: we can either replace the prototypical method with a new one by completely rewriting it, or we can execute something on top of it (first call `super().method`, then do something else as well).
Refs: https://realpython.com/python-super/

**Abstract classes** may be created using magic `@abstractmethod` from `abc` package. But you need to import this package, it's not a default functionality.

If you create a class, even the most empty class possible, it still inherits to some 26 or more default Pythonic methods (see "List of pythonic methods" below), as every class in Python 

**Non-local variables**: when defining function within a function, we can make local variables of the outer function become sorta "global" for the inner function, which may be handy. If you only plan to read from this variable, just refer to it as if it were global. If you plan to update it, write `nonlocal var_name` inside the inner method, as if "declaring
 it. After that it won't be masked (if you change it inside the method, these changes will be visible outside).

Refs:
*  https://realpython.com/instance-class-and-static-methods-demystified/
*  https://stackoverflow.com/questions/918380/abstract-classes-vs-interfaces-vs-mixins

# OOP in Python: Ideas

An archetypical situation in Python is a function (method) with lots of parameters (think how [[pandas]] methods are called). An archetypical advice is that once you have more than 3 parameters, you need a data class to be configured, as otherwise it becomes a mess; it's impossible to keep track of parameters, and their interactions with each other etc. A data class may be a NamedTuple, or a dict. But it's even better to have a class with properties, and control them with [[pydantic]] (both structure and static typing, see [[mypy]]).

Basically what we are trying to avoid is any sort of chaining (passing) parameters down the hierarchy, as it's a nightmare:
```python
def fun1(a=1, b=2):
  x = fun2(a=a, b=b)
```
What we want to do in cases like that is 🔥🔥🔥 ?????

> So what do we do? Do we create a config, and propagate it down as a whole? Or is there an "officially good" way to make properties of related (composed? interacting?) objects somehow "talk to each other"?

## Inheritance vs composition

There's an argument that **composition** (having a larger objects contain instances of smaller-leve objects) is easier to maintain in Python than class inheritance. One reason is that Java-style inheritance may conceptually clash with Pythonic methods that are implicitly inherited by every Pythonic object (inherited from the base clas of `Object`), and that becomes even more prominent if you try to use Pythonic patterns of programming, like iterators.

_If I undstood it correctly_: In composition paradigm, instead of creating a bunch of devired classes with custom methods, you give your classes a property that is itself an object, from a certain set of objects. Say, if you have humans and elves, and they can also work as either medics or scouts, in the inheritence paradigm you'll have a base "player" class, two derived "human" and "elf" classes, and a "medic" mix-in. In a compositional paradigm, you'll have a "player" class, with properties like "magic" and "profession", and then depending on the character, you'll populate it with different forms of `Magic` and `Profession` classes. In the inheritance paradigm, you'd write `player.do_job()` or `player.cast_magic()` and get different results depending on which abstract classes this particular derived class inherits to. In the compositional paradigm, you'd do `player.job.do()`, and the effect will depend on the type of object that is "loaded" in the "`.job`" slot for this `player`.

Refs:  https://realpython.com/inheritance-composition-python/

## Getters and setters

The idea is to protect object properties, making them private, and channeling all outer-world interactions with them through special methods: a getter (to get the property), and a setter. The benefit here is that you can add all sorts of safety checks when the property is both assigned and returned.

In some languages, like Java, these functions are typically called `get_x`. In Python, it's recommended to either keep properties public (if no checks are needed), or use special magic decorators to make them seem public, while actually having being accessed through a getter/setter. Note, that with the code below,  it's preferable to use `self.x` and not `self._x` in the class itself: even though the private property exists, it's safer to always keep the checks in place instead of bypassing them randomly.

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

# Trivia

As in Python "everything is an object", functions can have properties, as if they were classes, but that's weird, and not recommended: https://sethdandridge.com/blog/assigning-attributes-to-python-functionss

# Refs

.