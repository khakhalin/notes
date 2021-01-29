# Unit testing

(with emphasis on Python and Data Science)

#tools #coding #testing #oop

Parents: [[oop]], [[01_Tools]]
See also: [[python]], [[ml_lore]], [[design_patterns]]

The idea is of course simple: test every little part of your code separately. An opposite of **integration testing**, where product is tested as a whole.

Practically, in Python, you make a special class (name isn't critical) that inherits to `unittest.TestCase` (here `unittest` a package that needs to be imported). This class should contain methods with names starting with `test_`, that call the whatever you're testing with some parameters, and then do `self.assertEqual(a,b)` for the actual value, and the target value. These methods will be automatically run when this `.py` file is run.

What's the benefit of using this library? Instead of one error mistake, with the full verbose exception traceback, you will now have a neat list with all fails shown and explained (which file, which line, with what exception).

Unit testing deep into the code may be hard, as there's no context to work with. One may have to write some special code to mock, stub dependencies (aka **dependency injection**). For some reason this is also sometimes called **Inversion of control** (IoC).

Footnotes:
* https://nirajrules.wordpress.com/category/unit-testing/

## Code coverage

Essentially - the share of code (literally, how many lines) that gets executed during all unit tests, taken together. Sometimes there's a target on it (typically around 80%). Can also be defined in respect to functions, branches (in control structures), conditions (for every boolean expression) etc.

This is actually technically challenging. One approach: **offline instrumentation** - a way to modify code in an automated way, injecting trackers in every other line.

Footnotes:
* https://dzone.com/articles/code-coverage-not-only-for-unit-tests
* https://www.atlassian.com/continuous-delivery/software-testing/code-coverage

# Refs

https://www.freecodecamp.org/news/an-introduction-to-testing-in-python/