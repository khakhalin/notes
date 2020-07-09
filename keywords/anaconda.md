# Anaconda and installing packages

#tools

Parent [[python]]

# Installing packages

Two key ways:
* **conda** installs packages written in any language. Makes sure there are no conflicts using a "Satisfiability Solver".
* **pip** installs python packages. Installs in an "overwrite" mode: recursively installs all downstream dependencies, but doesn't look for overall consistency.

**Environments**
If, say, Tensorflow and Pytorch have conflicting requirements on versions of everything, we can isolate them in different environments. Conda can create its environments itself; for pip it's possible via some weird tools (venv?).

## Conda

* List all packages: `conda list`
* Find all available versions of a certain package: `conda search NAME`
* Figure out which one is installed: `conda list NAME`
* Force update to a certain version `conda install NAME=VERSION` (like 2.2.0 or smth)

## PIP

* Install: `pip install NAME==version`
* Remoe: `pip uninstall NAME`

Currently removed
tensorboard
tensorflow-gpu-estimator

grpcio 1.24.3

# Refs

* [Anaconda blog, understanding conda and pip](https://www.anaconda.com/blog/understanding-conda-and-pip#:~:text=Pip%20installs%20Python%20packages%20whereas,software%20written%20in%20any%20language.&text=Another%20key%20difference%20between%20the,the%20packages%20installed%20in%20them.)