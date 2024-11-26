# Environments

Parent: [[python]]

#tools

There are several ways to install packages in Python:
* **conda** installs packages written in any language. Makes sure there are no conflicts using a "Satisfiability Solver".
* **pip** installs python packages. Installs in an "overwrite" mode: recursively installs all downstream dependencies, but doesn't look for overall consistency.


When different python packages (like Tensorflow vs Pytorch) have conflicting requirements on versions of everything, we can isolate them in different environments. Conda can create its environments itself; for pip it's possible to use some weird tools (venv?), but it's better to just do pip within a decidated conda environment.

* List all environments: `conda info --envs`
* Create an environment for a package: `conda create -n NAME PACKAGE`. For example `conda create -n tf tensorflow-gpu`
* Switch: `conda activate NAME`
* Rename: `conda rename -n old_name new_name`
* To go to base: `conda deactivate`

**Jupyter** doesn't seem to be aware of conda environments by default, although apparently it is possible to hack it. Because of that, for now, I'll probably forgo environments. Even though not being able to easily make backup version of a conda state is a pain. _Maybe I can still use environments like that, tho? Like, make a copy of base, them mofidify base, then if it doesn't work, copy from the backup version back to base?_

## Conda

Conda may come with:
* `anaconda` - huge set of tools, too much typically. Beginners like it tho.
* `miniconda` - minimal conda installer from the Anaconda team, which means that some parts of it may conflict with commercial use; there was some weird ToS change at some point.
* `miniforge` - another minimal conda installer, this one community-driven and free. May have a slightly lower cross-platform support tho.

Environments:
* `conda activate env_name` - activate an environment
* `conda env list` - list environments
* `conda create -y -n env_name python=3.9.*` - one useful sequence. `-y` means that for this environment all conda-related dialogues will be set to "default yes" mode, when user doesn't have to confirm their commands.
* `conda env remove -n env_name` - removes environment

Packages:
* List all packages: `conda list`
* Find all available versions of a certain package: `conda search NAME` (same for core python)
* Figure out which one is installed: `conda list NAME`
* Force update to a certain version `conda install NAME=VERSION` (like 2.2.0 or smth)

Never do `update all`, or it can ruin your TF installation. Only upgrade point-by-point, and give up easily if any risky conflicts emerge. It's a house of cards.

## PIP

* `pip install NAME==version` - install
* `pip install NAME --upgrade` - upgrade
* `pip uninstall NAME` - remove
* `pip list` - see all versions
* `pip show package_name` - see the version of this package in particular. The name should be exact.
* `pip install -e ".[devel]"` - when run from a [[git]] repo, installs this repo as a package in developer mode (with code references linked to the repo 🔥 ??)

To investigate:
* `pip freeze` - 🔥 Not sure how it works, but seems to list all currently installed versions of all packages.
* `pip freeze | grep package_name_element` - the claim is that it is another way to check the name of all packages that a certain sequence within their name. A combo of freezing environment and searching through it with [[bash]]. In practice it seems to be finding wrong packages (with names not matching `pip list`), and doesn't seem to communicate their versions. I'm confused. 🔥

# GPU-related tuning

For Windows GPU work, do something like `pip install tensorflow-gpu==2.2.0`. (Though it defaults to the highest possible value anyways). Conda usually lags behind PIP by a few releases, and it's harder to satisfy it. 

To check if CUDA is installed, and if it sees GPU, on conda, do `numba -s`, then scroll to the top (where hardware is described).

# Refs

* [Anaconda blog, understanding conda and pip](https://www.anaconda.com/blog/understanding-conda-and-pip#:~:text=Pip%20installs%20Python%20packages%20whereas,software%20written%20in%20any%20language.&text=Another%20key%20difference%20between%20the,the%20packages%20installed%20in%20them.)