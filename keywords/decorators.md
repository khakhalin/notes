# Decorators

Parents: [[python]], [[oop]]
See also:


A Decorator is an object (a function or a class) acting as a meta-function: it takes another object as an argument, writes some helper (wrapper) code that refers to this object but also does something else on top (checking, validation, memoization, control access: ultimately, the reason for using a decorator to begin with!), and finally returns this outer wrapper. By doing so, it substitutes a function with another function, or a class - with another class, carefully passing along and preserving the required behaviors of the object, but also adding something on top. 

According to Google style code, while decorators may be helpful, they aren't normally recommended in Python specifically, as they tend to complicate things. 

# Decorating a func with a func

In Python, as functions are objects, you just get a (meta-) function that get function as an argument, and returns a modified function. Or you can do the same with methods of objects:
```python
def outer(fun):
	def inner(*args, **kwargs): # Same arguments as for fun()
    	return new_functionality(*arg, **kwargs,
                                 fun(*args, **kwargs))
     return inner

def f(...): ...
f = outer(f)
```

Moreover, to avoid writing `f = outer(f)` already after `f()` is defined, Python has a magic shortcut (`@`) which can be written before the defition, but that under-the-hood translates to this very substitution `f = outer(f)`. It just looks way neater:
```python
@outer
def f(...): ...
```

## Closures
 
 Closures are minimalistic decorator that bind data to a function, as if this data was hard-coded inside the function. It is an alternative to both global variables, and to passing this context data as additional arguments every time. If within this run you decide to repeatedly use a certain generic function in some narrower way, you can manufacture this narrower function from a global function (almost like in a factory pattern?). You just need to write a decorator that receives a prototype function together with some parameters, defines a new "frozen" function that calls the outer function with these parameters, making them constant-like; then returns this new "frozen" function.
 
Refs:
* http://www.trytoprogram.com/python-programming/python-closures/
* https://www.programiz.com/python-programming/closure

# Decorating a func with a class

🔥 _is this thing below correct? What if we want to have a magic with parameters? Definitely re-read and rewrite this_

```python
class outer():
	def __init__(self, fun):
    	self.fun = fun

    def __called__(*args, **kwargs):
    	# Do something else
    	return self.fun(*args, **kwargs)

@outer
def fun(...):
	...
```

# Decorating a class with a class

This is the most cumbersome use of decorators, and it is quite possible that in most cases one should use inheritence (with override of methods) instead. But in theory it's possible. The trick is that:
* the outer class will be `init` at declaration, so it will receive a class of the child as an argument, not an instance. It will save it as a variable. 
* Then the outer class will be `call`ed (!!) when actually the coder was trying to init the inner class!! So upon `call` it should init the inner class, remember it, and return the instance of itself (?? 🔥🔥🔥 ???)
* Finally, if the code is trying to call the inner class, actually the outer class will be called, so it should so something, and then also call the inner class, probably… (?? 🔥🔥🔥 ???)

> I think the description above is incorrect, as if the outer class is instantiated at definition, then we'll be returning the SAME outer class instance every time we're thinking that we're creating a new inner class instance. It's not gonna work. Unless we have 2 types of instantiators for the outer class, one to be called at definitoin, and another to be called when inner classes are initialized. Or maybe a 3d class - a wrapper - defined withiin the outer class, and instantiate wrappers?



# Refs

* https://www.python-course.eu/python3_memoization.php
* https://google.github.io/styleguide/pyguide.html