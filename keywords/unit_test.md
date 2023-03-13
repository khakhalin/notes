# Testing and continuous integration for Python & Data Science

Parents: [[01_Tools]] / [[oop]]
See also: [[python]], [[design_pattern]], [[ml_lore]], [[coding]] - some examples of unit tests for ML

#tools #coding #testing #oop


A simple idea: **test every little part of your code separately**, as opposed to **integration testing**, where product is tested as a whole. In Python, there are special packages : `pytest` (the most popular one), `unittest`, `nosetests`, `doctest`.

What's the benefit of using these libraries? Instead of one error message, with the full verbose exception traceback, you will now have a neat list of all fails shown and explained (which file, which line, with what exception).

# Pytest

Create a file with a name starting with `test_` (say, `test_fun1.py`). Inside this file, import `pytest`, import whatever you're planning to test; then write a bunch of functions (free-standing, not a part of any class), all named `test_bla_bla` Make the names descriptive, but there's no need to match the names of project classes in any specific way. Each function should test one use case, or one aspect of functionality. Inside each of these functions, call your methods of interest, and do all sorts of `assert call(x)==known_result`.

Alternatively, create a separate class for each test case. To work with pytest, these classes should be named `TestSomethingSomething` (camelcase), and then individual methods within each class still should be named `test_something_something`.

Then in command line (terminal, not Python console), run `pytest test_fun1.py`, or, if all tests are collected in one folder (named `tests`), just run `pytest tests`. This would run all the tests in each of the files, and output the result (either a total sum of all tests that were run, or a list of errors that happened).

Useful practical asserts that can be used:
```python
assert isinstance(y, (np.ndarray, np.generic))
assert expected_value == pytest.approx(x, rel=1e-3)
assert np.allclose(xhat, x, atol=1e-5)
```
Here `np.allclose()` is a cool [[numpy]] function that tests of all elements in two array-like variables are close to each other, within a certain relative and/or absolute error (given by parameters with default values `rtol=1e-05, atol=1e-08,`)

To test that some function raises an exception when it should raise an exception, do this ([ref](https://stackoverflow.com/questions/23337471/how-to-properly-assert-that-an-exception-gets-raised-in-pytest)):
```python
pytest.raises(Exception):
    test_function()
```
This sequence will succeed if `test_function` failed (and raised an exception), and will fail if it succeeded. It is also possible to specify which exactly exception needs to be raised.

Pytest gives a really nice run-down of how the assert expression itself is calculated, how exactly the left side is ≠ to the right side, and where all values are coming from. To debug further, and see the content of other local variables in the pytest output on failure, you may also write some like `assert x==1, f"y={f}"`, as this string after the comma will become a part an assert error message, and so will be shown in the output.

Pytest also allows to run only tests with a name matching a pattern: `pytest -k "pattern"`. With a good naming system, this may be useful. Moreover, this "pattern" thing supports logical operations and what not (`pytest -k "ToLine and not test_string"`). 🔥

It's also possible to mark-up code with helpers, mark some tests as "expected to fail" (aka "ignore for now"), skip some tests conditionally, based on the environment variables, such as Python version, or operating system:
`@pytest.mark.skipif(sys.version_info > (2, 7), reason="requires Python 2.7")`

To pre-load some heavy data for the test, one needs to use a **fixture**.
```python
@pytest.fixture(scope="session") # Without this it will be recalculated every time
def preload_data():
    return load_heavy_data()

def test_something(preload_data): # This using fixture as an input 
    data = preload_data # Note the absence of parantheses here
    test(data)
```
This is the part that I don't quite like, for data science purposes itself, as it doesn't allow sequential tests. Also, the fixture itself isn't tested, so if it loads data, and we want to have a test for data loading, we'll have to load data twice (which is ok, but not ideal). It is allegedly possible to test fixtures itself using a `pytester` plugin (a tester for tests) ([ref](https://stackoverflow.com/questions/56631622/how-to-test-the-pytest-fixture-itself)), but honestly it looks super cumbersome.

It seems to also be popular go use fixtures to setup databases, and then finish them with `yield` instead of `return`, to create samplers from these databases (? 🔥 ) See for example: [1](https://smirnov-am.github.io/pytest-advanced-fixtures/)

# Unittest

Another popular alternative to `pytest`. The key difference is that you need to make a special class (name isn't critical) that inherits to `unittest.TestCase` (`unittest` is a package that needs to be imported). This class should contain methods with names starting with `test_`, which call whatever you're testing with some parameters.

Then instead of using build-tin pythonic `assert`, we use some custom asserts. 
* Equality: `self.assertEqual(a,b)` These methods will be automatically run when this `.py` file is run.



# Fancy techniques

Unit testing deep into the code may be hard, as there's no context to work with. One may have to write some special code to mock, stub dependencies (aka **dependency injection**). For some reason this is also sometimes called **Inversion of control** (IoC).
Footnotes:
* https://nirajrules.wordpress.com/category/unit-testing/

**Code coverage**: essentially - the share of code (literally, how many lines) that gets executed during all unit tests, taken together. Sometimes there's a target on it (typically around 80%). Can also be defined in respect to functions, branches (in control structures), conditions (for every boolean expression) etc. Achieving high code coverage may be technically challenging. One approach: **offline instrumentation** - a way to modify code in an automated way, injecting trackers in every other line.
Footnotes:
* https://dzone.com/articles/code-coverage-not-only-for-unit-tests
* https://www.atlassian.com/continuous-delivery/software-testing/code-coverage

# Misc advice

In large projects, it may be prudent to organize tests in the same way in which the main project is organized (just within the `tests` folder). This way, there's some traceable correspondence, structure to it.

# Continuous Integration

**Travis CI** app on GitHub; free for open source projects. With it, every commit leads to a build. Testing can now be automated as apart of CI process, via editing `.travis.yml` settings file. Another attractive option here: `codeconv` app that generates code coverage reports (and if integrated with Travis like that, it makes code coverage reports auto-generated).

# Refs

https://www.freecodecamp.org/news/an-introduction-to-testing-in-python/