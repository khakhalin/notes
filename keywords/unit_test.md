# Unit testing

(with emphasis on Python and Data Science)

#tools #coding #testing #oop

Parents: [[oop]], [[01_Tools]]
See also: [[python]], [[ml_lore]], [[design_patterns]]

The idea is of course simple: test every little part of your code separately.

Practically, in Python, you make a special class (name isn't critical) that inherits to `unittest.TestCase` (here `unittest` a package that needs to be imported). This class should contain methods with names starting with `test_`, that call the whatever you're testing with some parameters, and then do `self.assertEqual(a,b)` for the actual value, and the target value. These methods will be automatically run when this `.py` file is run.

What's the benefit of using this library? Instead of one error mistake, with the full verbose exception traceback, you will now have a neat list with all fails shown and explained (which file, which line, with what exception).

# Refs

https://www.freecodecamp.org/news/an-introduction-to-testing-in-python/