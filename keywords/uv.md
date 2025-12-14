# UV

Parents: [[tools]], [[environments]], [[python]]
See also: [[git]]

#tools #environment #python


UV seems to be a super-fast replacement for pip.

Common usage: instead of doing `pip something` do `uv pip something`. But if you are doing an uv project properly (see below), it's better to use `uv add`, as `uv add` updates the `.toml` file, while `uv pip` doesn't.

To add a particular version, `uv add numpy==1.25.0`

# New project: pure uv

In this mode you do:

* `uv init` - it creates a venv. If running within an existing repo, use `uv init --bare` instead, then placeholder code will not be created. It creates the `pyproject.toml` file.
* `uv sync` - just a good thing to do, in this case?
*  You may want to add `.venv` to `.gitignore` if it's not added there automatically. `.toml` and `.lock` files, on the other hand, should be committed to vent.
* `uv add numpy` etc. to install packages  - it updates `toml` and the venv. If something is only need in development, use `--dev` key, as for example here: `uv add --dev pytest`

To run anything using venv (virtual environments) you just prefase it with `uv run ...`, like for example `uv run python -m` or `uv run pytest`.

To install a project of this kind after cloning it from git, just do `uv sync`.

# Antipattern: combining conda with uv

This is a legacy mode that preserves pip syntax: you just use `uv pip` instead of `pip`. For all purposes, including `uv pip list` to list installed packages. It's much faster than pure `pip`, but of course doesn't use most of `uv` features. It is _not recommended_ in practice, but I retain some notes here, as i've seen it done like that as a "transitional phase" from long-term conda use to uv.

1. `conda create --name env_name python=3.12` - before installing anything, create a conda environment. We will also use conda to install Python and uv itself. `conda activate env_name`
2. `pip install uv` - this installs uv
3. `mkdir new_project`, `cd new_project`
4. Init git. One option is to run `git init` locally before initializing `uv`. But then github-style license won't be created. Alternatively, we can create a project on github and clone it.
5. `uv init --no-workspace` - this will create a `pyproject.toml`. The `--no-workspace` key is important to let uv know that we're using conda to maintain environments, otherwise it will pull the blanket on itsef.
6. Add all dependencies you need using `uv pip install numpy`. The `toml` file isn't updated automatically in this case.
7. You may need to create a conda-targeting `environment.yaml`
8. Commit and push go Github.

If repo was created locally, and not cloned, here's how you push. Got on gibhub, create a new repo. Then do this.
```
git add . 
git commit -m "Initial commit"
git remote add origin https://github.com/your-username/your-repository.git
git pull origin main --allow-unrelated-histories
git push -u origin main
```

As in this mode we're using pip-style management, to require a requirements file, do `uv pip freeze > requirements.txt` after each install.