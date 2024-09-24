# Linting

Parent: [[python]]
See also: [[mypy]], [[unit_test]]

#python #coding


Common packages: 

# Flake8

To exclude a row from tests (to make it an exception), write `# noqa: E800` after it. It will remove it from this particular rule. Use a comma-separated list for several rules. In particular, `E800` is about forbidding commented code (which from linter's pov somehow includes any formal writing).

Flake does not have "on" and "off" commands to disable linting for a segment of code, unfortunately. Either row by row, or an entire file :( To do an entire file, add `# flake8: noqa` to it.

Linters also obviously come with a config file where one can put some rules on a permanent ignore list. For example, by default Flake8 doesn't allow lambda-expressions. Who would ever want to work with a rule like that? No one!

# Isort

Isort literally just sorts imports at the beginning of the file. To exclude a file from isort, add `# isort: skip_file` at the beginning. If you want to keep your imports split by group (say, standard first, custom later) and isort keeps destroying it, use `# isort: split` between groups (then it will sort and reformat only within each group). 

⚠️Unfortunately Flake also tries to check for sorting, and it totally ignores `# isort: split` throwing warnings. For now I don't know how to make flake defer to isort's decisions about optimal import sorting. But allegedly `application-import-names` and `application-package-names` sections in `.flake8` config file may help to at least somewhat rectify this. 🔥

# Black

`# fmt: off` turns formatting linting off starting from this command. If you only need to exclude a block of the code from formatting interventions (say, a manuall formatted matrix or something like that), add a `# fmt: on` once the tricky segment is over.