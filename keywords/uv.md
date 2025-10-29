# UV

Parents: [[tools]], [[environments]], [[python]]
See also: [[git]]

#tools #environment #python


UV seems to be a super-fast replacement for pip.

Common usage: instead of doing `pip something` do `uv pip something`. But if you are doing an uv project properly (see below), it's better to use `uv add`, as `uv add` updates the `.toml` file, while `uv pip` doesn't.

To add a particular version, `uv add numpy==1.25.0`

To list installed packages, `uv pip list`

# Starting a new project: uv with conda

This is a legacy mode that preserves pip syntax: you just use `uv pip` instead of `pip`. Not sure it's a good one.

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

# New project: pure uv

In this mode you do
* `uv init` - it creates a venv
* `uv add numpy` for all changes - it auto-updates `toml` and the venv
*  You may want to add `.venv` to `.gitignore` if it's present in the folder (if uv created it)

How to recreate an environment from toml: ????