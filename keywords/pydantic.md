# Pydantic

A popular Python library for data flow validation.

Parents: [[python]], [[data_cleaning]]
See also: [[mypy]]


Pydantic is a model for data processing / data flow validation: it does not analyze the data itself, but the validity and consistency of transformations applied to the data. The process is based on importing `BaseModel` from `pydantic`, and then creating a bunch of subclasses for this model, with parameters (representing fields) that are typed (see [[mypy]]), and may be assigned default values. It consistently uses the word "model" to describe the data schema (say, by having a method `.model_dump()` defined for the `BaseModel` class), which may be confusing in practice. In a team with more DEs than DSs, the default meaning of the word "model" will inevitably shift from ML models to Pydantic data models.

# Refs