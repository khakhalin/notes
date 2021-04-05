# Unit testing

(with emphasis on [[python]] and Data Science)

#tools #coding #testing #oop

Parents: [[oop]], [[01_Tools]]
See also: [[python]], [[design_pattern]], [[ml_lore]] - some examples of unit tests for ML


A simple idea: **test every little part of your code separately**, as opposed to **integration testing**, where product is tested as a whole.

In Python, there are special packages : `pytest` (the most popular one), `unittest`, `nosetests`, `doctest`.

What's the benefit of using these library? Instead of one error message, with the full verbose exception traceback, you will now have a neat list of all fails shown and explained (which file, which line, with what exception).

## Practical implementation

### Pytest package

Create a file with whatever name (say, `test_fun1.py`). Import `pytest` and whateveer you're planning to test; then write a bunch of functions (free-standing, not a part of any class), named `test_blabla` (make it descriptive, not need to match anything). Inside, call your function, and do `assert call(x)==known_result`.

Then in command line (terminal, not Python console), run `pytest test_fun1.py`, or, if all tests are collected in one folder (say, `tests`), run `pytest tests`. This would run all the tests in each of the files, and output the result (either a total sum of all tests that were run, or a list of errors that happened).

Interesting practical asserts to call:
```python
assert isinstance(y, (np.ndarray, np.generic))
assert expected == pytest.approx(x, rel=1e-3)
assert np.allclose(xhat, x)
```
Here `np.allclose()` is a cool [[numpy]] function that tests of all elements in two array-like variables are close to each other, within a certain relative and/or absolute error (given by parameters with default values `rtol=1e-05, atol=1e-08,`)

Pytest gives a really nice run-down of how the assert expression itself is calculated, how exactly the left side is ≠ to the right side, and where all values are coming from. To debug further, and see the content of other local variables in the pytest output on failure, you may also write some like `assert x==1, f"y={f}"`, as this string after the comma becomes a part an assert error message, and so is shown in the output.

### Unittest package

Make a special class (name isn't critical) that inherits to `unittest.TestCase` (where `unittest` is a package that needs to be imported). This class should contain methods with names starting with `test_`, that call the whatever you're testing with some parameters, and then do `self.assertEqual(a,b)` for the actual value, and the target value. These methods will be automatically run when this `.py` file is run.

## Fancy techniques

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