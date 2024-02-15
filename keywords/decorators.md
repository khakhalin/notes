# Decorators

Parents: [[python]], [[oop]]
See also:


A Decorator is an object (a function or a class) acting as a meta-function: it takes another object as an argument, writes some helper (wrapper) code that refers to this object, but also does something else on top its basic functionality (checking, validation, memoization, control access: ultimately, the reason for using a decorator to begin with!), and then returns this outer wrapper. By doing so, it substitutes a function with another function, or a class - with another class, carefully passing along and preserving the behaviors of the object, but also adding something on top. 

According to Google style code, while decorators may be helpful, they aren't normally recommended in Python specifically, as they tend to complicate things. 

# Decorating a function

.

## Closures
 
 Closures are minimalistic decorator that bind data to a function, as if this data was hard-coded inside the function. It is an alternative to both global variables, and to passing this context data as additional arguments every time. If within this run you decide to repeatedly use a certain generic function in some narrower way, you can manufacture this narrower function from a global function (almost like in a factory pattern?). You just need to write a decorator that receives a prototype function together with some parameters, defines a new "frozen" function that calls the outer function with these parameters, making them constant-like; then returns this new "frozen" function.
 
Refs:
* http://www.trytoprogram.com/python-programming/python-closures/
* https://www.programiz.com/python-programming/closure

# Decorating a class

This is the most cumbersome use of decorators, and it is quite possible that in most cases one should use inheritence (with override of methods) instead.

.

# Refs

* https://www.python-course.eu/python3_memoization.php
* https://google.github.io/styleguide/pyguide.html