# Pydantic

A popular Python library for data flow validation.

Parents: [[python]], [[data_cleaning]]
See also: [[mypy]], [[kedro]],  [[oop]], [[sqlalchemy]]

#validation #python


Pydantic is a data flow / data model validation tool: it does not analyze the data itself, but checks the validity and consistency of transformations applied to objects that carry this data. The process is based on importing `BaseModel` from `pydantic`, and then creating a bunch of subclasses inheriting to this model, with typed (see [[mypy]]) parameter fields (maybe also describing allowed values). Once this is done, Pydantic can check whether complex hierarchical [[json]]-like structures that are passed via APIs between different parts of your code are consistent, and match the existing schema. For example:

```python
from pydantic import BaseModel
from typing import List, Optional

class Profession(BaseModel):
	name: str = 'Blacksmith'

class Person(BaseModel):
	age: int
    profession: List[Profession]
    gender: Optional[str] = None
```
>  Pydantic consistently uses the word "model" to describe the data schema (say, by having this word in the names of its methods, as in `.model_dump()`), which may sometimes be confusing. In a team with more DEs than DSs, the default meaning of the word "model" will inevitably shift from ML models to Pydantic data models.

If classes are introduced out of order (that is, we need to reference a data class before it was defined), we have to reference them as strings, by name (e.g. `'Profession'`, same as it is the case for [[mypy]]). Once they are finally loaded, we need to call `Person.model_rebuild()` on the model that references them, to turn these strings to real references.

Once you actually have classes with fields, you can validate them using this Pydantic model to will check if all the promised fields are indeed provided: `Person.model_validate(anna)`. If validation fails, Pydantic raises `ValidationError`.

Finally, Pydantic is a popular tool for serializing [[oop]] structures (see below).

Apparently it's particularly popular with web-interfaces (like FastAPI for example), as it integrates well, and can both validate and fix (somewhat) inputs from forms.

# Model-building

⚠️Importantly, there was a big non-inremental change in behavior between Pydantic v1 and Pydantic v2 in terms of how required and optional fields are treated (tbc 🔥)

* Most basic types (`int`, `float`) are validated as-is
* `str` (strings) are apparently coersed (🔥?)
* `list`, `tuple` and `set` are weird, as they allow each other, as well as `frozenset`, `deque`, and generators. It attempts to coerse to whatever type you specified though.
* `dict` - same thing: it tries to coerse to dict with `dict(v)`, so everything depends on whether the conversion is successful.
* `typing.Any` doesn't work unfortunately (doesn't work as one could expect that is would), as it includes `None` as a possible "value" for a key, which essentially makes the field optional. It's NOT how you tell the model that you just want the field to exist. Distinguishing between `None` and unset fields is possible, but not trivial (see below).
* `typing.Union` allows to combine several types. As a reminder, it is used as `a: Union[this, that]` (with square brackets).
* `age: None` means that the key `age` has to be set to None. But it's different from non-existing: it needs to exist, just be set to None! Interestingly though, if you do `Union[int, None]`, that's synonimous to `Any`, and just makes the key optional. Not sure why…
* `typing.Dict`, `typing.Set` etc - allow to define more complex objects, such as `Dict[str, float]` for example.
* `pathlib.Path` - tries to convert the value with `Path(v)`, which may be helpful for validation
* Writing something like `a: str = 'something'` is used to set a default value  🔥 _I'm not sure what it means actually… Including what it means if you do `= None` in this context_

**Built-in constraint validations** are achieved using `a: int = Field(gt=0)` (as if giving a field a default value), with `Field` imported like that: `from pydantic import Field`. 
> 🔥 Is it Field, or is it `from pydantic import constr`? Because some manuals seem to do exactly same stuff with `constr` instead of Field...

The functionality supported by this syntax is kinda all over the place, but some tricks are pretty useful:
* For numbers:
    * `gt=0` - positive (greater than; `lt` for the opposite)
    * `ge=0` - non-negative (greater or equal; `le` for the opposite)
    * `multiple_of=2` - even, for ints only
    * `allow_inf_nan=True` - subj
    * `max_digits`, `decimal_places` - for floats only
* For strings:
    * `min_length=3`, `max_length=10` - subj
    * `pattern=r'^\d*s'` - [[regex]]
* For objects:
    * `init`, `init_var`, `kw_only` - not sure 🔥
* Other
    * `Field(default='smth', validate_default=True)` - apparently, validates whether the field default value is set in the class!
    * `repr=True` - whether this field is included in the `repr` behavior for tihs object ( 🔥 as far as I understand, in this case it's not something that can be validated, or that validates something, but just a way to control `repr` behavior for a class without defining repr explicitly)
    * `exclude` - a way to exclude fields when outputing models as json…
    * `deprecated` - ?…

Distinguishing a non-set (non-existent) field, and a field that was created but was set to `None` is notoriously hard. There seems to be no quick syntax for that. One has to create a manual validator, reach into properties like `model_fields_set` etc. 
Footnotes:
* https://stackoverflow.com/questions/66229384/pydantic-detect-if-a-field-value-is-missing-or-given-as-null

Interestingly, requiring a `pydantic BaseModel` type from a field also causes problems: it doesn't break at first, if the field is indeed a model, but then the sub-keys of this sub-model aren't loaded, as BaseModel has `extra` config field set to `ignore` and not to `allow`. Creating a "PermissiveModel" inheriting from `BaseModel`, but with `config` re-setting `extra` parameter to `allow` fixes this problem. This seems to be the best way require that a sub-model exists, without specifying its structure in detail.

# Configs

While defining a class one can add configs:
```python
class Something(BaseModel):
	a: int

    class Config:
    	extra = "forbid" # If extra fields are allowed. 
        orm_mode = True # Not sure what it means actually 🔥
```

What can go into a config?
* `extra` - can be `ignore` (default), `allow` or `forbid`. If you forbid it, then having an extra field fails validation.
* `orm_mode` - not sure how it works, but it's about interacting with [[sqlalchemy]] and validating these hierarchies (???)🔥 See docs!
* `min_anystr_length` (and the same for `max_`) imposes a limitation on strings
* `arbitrary_types_allowed` - unsure but looks interesting? 🔥
* ...and a bunch of other options related to setting how Pydantic itself operates.

# Validators

In `pydantic.v1` it was possible to validate fancier propertis of an object hierarchy by using validator magic:
```python
from pydantic import validator

class MyClass(BaseModel):
	a: int

    @validator('a')
    def custom_check(cls, v):
    	assert v > 0
        return v # 🔥 Why is this needed?
```
( The example above is not realistic, as there's a better way to check `x>0` using `Field(gt=0)`; see above).

Common uses for validators:
* Check the value of the field (as shown above)
* Check if it's not None (technically, also value-checking)
* Check if at least one key is present

In `V2` the syntax has changed! 🔥🔥🔥 - see the migration guide on Pydantic site

# Saving and loading models

A model can be turned to [[json]] object using `.model_json_schema()` method (and then outputted as a json string, by using `json.dumps(model_as_json)`  on this json object. An opposite process  starts with `my_dict = json.loads(json_text)`, and then by calling `pydantic.tools.parse_obj_as(my_dict)`. 

For custom types (created with `Annotated`) one can create custom serializers, including serializers that are dependent on the target format for serialization. Check Pydantic documentatoin.

# Custom Errors

There seems to be a whole set of tools for producing meaningful, helpful errors. tbc 🔥🔥🔥

# Open questions

🔥As in [[mypy]], there's this special situation with `TypeVar('T')` and `Generic[T]` models that for now I don't understand.

🔥One can also create models on-the-fly, in runtime, from a dict, and so still be able to use it for downstream validation later. Use the `create_model` method.

# Refs

[https://docs.pydantic.dev/latest/](https://docs.pydantic.dev/latest/) - main documentation

Robust Python by Patrick Viafore. Chapter 14. Runtime Checking With pydantic