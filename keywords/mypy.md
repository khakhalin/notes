# Mypy and Typing

Parent: [[python]], linting
See also:

#python


Starting Python 3.5 one can add type hints in a form of
```python
age: int = 1 # or simply age: int, if declared by not assigned

def func(x: str) -> int:
	return 0
```
Built-in types: int, float, bool, str
(also some `x: bytes = b"test"`, but not sure what it means)


None of these declarations do anything though; they don't affect run-time, and they are not checked in any way by Python itself. Where they become useful is if you do some checks, linting-style, before running the script.

**Typing** package 

# Refs

https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html