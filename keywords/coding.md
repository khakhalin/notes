# Coding

Parents: [[01_Tools]]
See also: [[python]]

#bib #coding


Subtopics:
* [[oop]] - object-oriented programming
    * [[design_pattern]] - a famous coding framework
    * [[solid]] - one of the most core oop principles
* [[unit_test]]- on testing in general
* [[debugging]] - how to debug
* [[refactoring]] - how to refactor
* [[logging]] - (how to log, mostly in [[python]])

Languages:
* [[python]]
* [[javascript]]
* [[julia]]

# Code smell (bad practices)

Types of smells:
1. Bad variable names (Remember: a long name is always better than a long comment!)
2. Functions that do many things instead of one thing
3. Code repetition
4. Magic values (hard-coded values like 256, 0.9 etc.)
5. Dead code (commented out, inconsequential)
6. Print statements everywhere (ruins of abandoned debugging)
 
# Good practices

* Write unit tests, and start with tests, aka test-driven development: see [[unit_test]]
* Make small and frequent commits

Methods and functions should:
1. be small (~20 lines = 1 screen; better less than that)
2. do one thing only (not many different things)
3. do it at one level of abstraction (like, it shouldn't combine high-level processing of an object and detail work on its innards)
    * You want to have a hierarchy of functions, so that you could read and understand high-level code without necessarily diving down into the low-level code. At high-level you have things like, for example, "create", "run", "change", "delete". At low level you work on peculiarities of the inner workings of this object. You don't want to mix these levels together, or the code will become unreadable and untraceable.
* have few arguments (ideally, 0-2)
* not use flags as arguments (so something like `fun(also_do_this=True)` is not good) - consider writing 2 functions instead
    * It feels to me that this last one is more questionable than the others?

# References:

https://www.thoughtworks.com/insights/blog/coding-habits-data-scientists