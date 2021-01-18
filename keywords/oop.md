# Object-Oriented Programming

#oop

Related: [[python]]
Subtopics: [[design_patterns]]

# Getters and setters

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