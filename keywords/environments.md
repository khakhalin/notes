# Environments

#tools

Parent: [[python]]

Two key ways to install stuff
* **conda** installs packages written in any language. Makes sure there are no conflicts using a "Satisfiability Solver".
* **pip** installs python packages. Installs in an "overwrite" mode: recursively installs all downstream dependencies, but doesn't look for overall consistency.


When different python packages (like Tensorflow vs Pytorch) have conflicting requirements on versions of everything, we can isolate them in different environments. Conda can create its environments itself; for pip it's possible to use some weird tools (venv?), but it's better to just do pip within a decidated conda environment.

* Create an environment for a package: `conda create -n NAME PACKAGE`. For example `conda create -n tf tensorflow-gpu`
* Switch: `conda activate NAME`.
* To go to base: `conda deactivate`

**Jupyter** doesn't seem to be aware of conda environments by default, although apparently it is possible to hack it. Because of that, for now, I'll probably forgo environments. Even though not being able to easily make backup version of a conda state is a pain. _Maybe I can still use environments like that, tho? Like, make a copy of base, them mofidify base, then if it doesn't work, copy from the backup version back to base?_

## Conda
`
* List all packages: `conda list`
* Find all available versions of a certain package: `conda search NAME` (same for core python)
* Figure out which one is installed: `conda list NAME`
* Force update to a certain version `conda install NAME=VERSION` (like 2.2.0 or smth)

Never do `update all`, or it can ruin your TF installation. Only upgrade point-by-point, and give up easily if any risky conflicts emerge. It's a house of cards.

## PIP

* Install: `pip install NAME==version`
* Remoe: `pip uninstall NAME`

For Windows GPU work, do something like `pip install tensorflow-gpu==2.2.0`. (Though it defaults to the highest possible value anyways). Conda usually lags behind PIP by a few releases, and it's harder to satisfy its stricter 

# CUDA and technical stuff

To check if CUDA is installed, and if it sees GPU, on conda, do `numba -s`, then scroll to the top (where hardware is described).

# Refs

* [Anaconda blog, understanding conda and pip](https://www.anaconda.com/blog/understanding-conda-and-pip#:~:text=Pip%20installs%20Python%20packages%20whereas,software%20written%20in%20any%20language.&text=Another%20key%20difference%20between%20the,the%20packages%20installed%20in%20them.)