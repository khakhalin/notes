# Linting

Parent: [[python]]
See also: [[mypy]], [[unit_test]]

#python #coding


Common packages: **Flake8**

To exclude a row from tests (to make it an exception), write `# noqa: E800` after it. It will remove it from this particular rule. Use a comma-separated list for several rules. In particular, `E800` is about forbidding commented code, which seems to also cover any sort of code within a comment, which is of course not helpful at all.

Linters also obviously come with a config file where one can put some rules on a permanent ignore list. For example, by default Flake8 doesn't allow lambda-expressions. Who would ever want to work with a rule like that? No one!