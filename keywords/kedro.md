# Kedro

Parents: [[data_cleaning]], [[python]]
See also: [[pydantic]]

#validation #python


Open-source Python framework to provide "scaffolding" for DS and ML. Data versioning, incremental computations. Has some visual tool (`kedro viz`) to visualize the flows within the project.

Needs to be installed with pip. Then create a new project with `kedro new`. This would initialize a template (literally, some folder structure). List all your project requirements in `requirements.txt` (which was pre-created for you; see docs for language). Install all dependencies automatically (if necessary).

Access credentials (aka "secrets": logins and passwords) go to a special file `conf/local/credentials.yml` (the intro reads as if it's added to `gitignore` by default, but I'm not sure).

Define your data sources in `conf/base/catalog.yml`, giving names to individual sources (even if these are just `csv` data files).

Define data storage for intermediate results, also in this yaml file (typically, these are Parquet files, stored either locally or in blobs in the cloud). They are also meaningfully named, as these names will be used later in pipeline configuration. If an intermediate file is set as `versioning: true` then instead of being overwritten, the results of intermediate runs will be stored.

> There's something in the docs about how if you don't specify these files, these intermediate steps are still created in some shared space, but are then cleaned automatically, but I'm not sure if I got it right.

The meat of the project is based of "nodes", or individual steps of data processing, each with clear inputs and outputs (in the form of the data files, listed above). They are described in the `nodes.py` file (🔥but also there's some `nodes` folder apparently?)

The nodes are arraged into a global computation graph by the `pipeline.py` file. In it, the full pipeline is assembled a `Pipeline` object, made from a list of `node` objects, with sources and outputs specified as arguments (using names from the `catalog` yaml).

Doing `kedro run` for this project executes all pipelines, and creates a nice logging output (kedro can automate some fancier logging as well). With a `ParallelRunner` (not a default setting), independendent nodes can be run in parallel. You can also run individual nodes, or node lists. Or run only some sub-pipelines by naming and then "slicing" them (🔥not sure how).

Pipelines are specified in `conf/base/parameters_data_science.yml`; one can set random seeds, test size, and other parameters (🔥?)

# Refs

kedro.org
docs.kedro.org

