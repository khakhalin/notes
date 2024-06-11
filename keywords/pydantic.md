# Pydantic

A popular Python library for data flow validation.

Parents: [[python]], [[data_cleaning]]
See also: [[mypy]], [[kedro]],  [[oop]]

#validation #python


Pydantic is a model for data processing / data flow validation: it does not analyze the data itself, but the validity and consistency of transformations applied to the data. The process is based on importing `BaseModel` from `pydantic`, and then creating a bunch of subclasses for this model, with typed (see [[mypy]]) parameters representing fields (and maybe also describing default values). For example:

```python
from pydantic import BaseModel
from typing import List, Optional

class Profession(BaseModel):
	name: str = 'Blacksmith'

class Person(BaseModel):
	age: int
    professoin: List[Profession]
    gender: Optional[str] = None
```
>  Pydantic consistently uses the word "model" to describe the data schema (say, by having this word in the names of its methods, as in `.model_dump()`), which may be confusing in practice. In a team with more DEs than DSs, the default meaning of the word "model" will inevitably shift from ML models to Pydantic data models.

If classes are introduced out of order (we need to reference a data class before it was defined), then same as in [[mypy]] we have to reference them by name, as strings (e.g. `'Profession'`). And then once they are finally loaded, we call `Person.model_rebuild()` on the model that reference them, to turn these strings to real references.

Now, once you actually have classes with fields, you can validate them using this Pydantic model: it will check if all the promised fields are indeed provided: `Person.model_validate(anna)`. If validation fails, Pydantic raises `ValidationError`.

🔥As in [[mypy]], there's this special situation with `TypeVar('T')` and `Generic[T]` models that for now I don't understand.

🔥One can also create models on-the-fly, in runtime, from a dict, and so still be able to use it for downstream validation later. Use the `create_model` method.

A model can be turned to [[json]] object using `.model_json_schema()` method (and then outputted as a json string using `json.dumps(model_as_json)` method on this json object. An opposite process apparently starts with `my_dict = json.loads(json_text)`, and then by calling `pydantic.tools.parse_obj_as(my_dict)`. But that's a rare use case, apparently. it seems that people dump models to json more often than creating models from them? 🧠

# Refs