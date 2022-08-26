# Mongo

Path: [[database]] / [[nosql]]

#db #tools


Most conceptual topics about nosql database design are covered in [[nosql]]

The hierarchy of Mongo data (it's a tree, with several elements at each level):
* **Server**
* **Database**
* **Collection** - analogious to a table in relational databases.
* **Document** - this is roughly equivalent to a row in a large [[sql]] table. It probably corresponds to one instance / object, of whatever this database is supposed to track / represent
* **Key-value pairs** - these serve as columns in an [[sql]] database, except that every document may have different keys, so it's really not an orthogonal table, but a collection of somewhat similar objects. There's also no expectation that a shared key implies the same data type.

The only required key (?) is `_id` - you either provide it, or Mongo invents it for you, if you don't. Unique for each document.

The most typical way to repsent documents is using [[json]].

**Collections** cannot even be explicitly created; the only way to create it is to try to write (insert documents) into it.

If a complete free-for-all is too chaotic, it is possible to **normalize** a collection, by enforcing a certain structure to it (some required keys, some required hierarchy). This allows nested referencing (?🔥 show example here?)

To enable fast sorting, lookup, and range-like retrieval within a collection, one needs to create an **index**. An index can include several fields, in which case the sequence of them, as given at index creation, defines the hierarchy. As in case of mongo, index is more like physical linear pre-sorting of records, each key in the index is also specified to be either in ascending, or descending order.

# Queries

Queries in Mongo don't look like normal expressions we all like, but rather as ugly tiny pieces of JSON (also reminiscent of python dicts), in a form of `find(expression)`. For example:

* Equality: `{'location':'osaka'}`
* Less then: `{'date':{'$lt':'2002-02-02'}}`. Similarly, `gt` (greater), `lte` (less or eq), `gte`
* Not equals: `{'location':{'$ne':'berlin'}}`
* In set: `{'location':{'$in':['osaka', 'kyoto']}}`. Similarly, `$nin` for "not in set"
* AND: `{'$and': [EXPR1, EXPR2]}` (where `EXPR` shoud both have their own `{}`). Similarly, `$or`, `$not`, `$nor`

# Pymongo

The most popular [[python]] package for mongo is **pymongo** . Typical use:
```python
from pymongo import MongoClient
client = MongoClient('mongo.my.domain', port=8080) # Some arbitrary values here for example
db = client['database_name']
collection = db['collection_name'] # This can be done even if 'collection_name' doesn't exist yet! (for writing)
result = collection.find({'location': 'osaka'})

# Writing and editing
id = collection.insert_one(document_as_a_dict) # Returns the _id field of a newly created document
ids = collection.insert_many(list_of_dictionaries) # Returns a list of _ids
collection.delete_many({$and: [{'location':'osaka'}, 
                               {'date': {'$gte': START_DATE}},
                               {'date': {'$lt':  END_DATE}}
                              ]})

# Cleaning up
collection.drop()
```

As with pandas, instead of brackets-notation `db['collection']` one can use point-notation `db.collection`, but then collection name needs to be written explicitly, while with bracket-notation it can be stored in a variable (as a string).

Note that while `find_one()` directly returns a document, a simple `find()` returns a lazy iterator, so we have to either iterate through it, or force data retrieval using `list(result)`

[[pandas]] can create dataframes from these dict-like structure, given that they are reasonably normalized (and maybe not too deeply nested? 🔥 ), so to get these results to pandas it's enough to do `df = pd.DataFrame(list(result))`. To go in the opposite direction, use `df.to_dict('records')` as an argument for `insert_many()`. This `records` argument just tells pandas how to exactly transform a df into a dict: this way it returns a list of dictionaries, one dictionary for each row, which is exactly what we need here.

To create a new collection, just pretend to "grab" a non-existing collection `db['new_name']` and try to write into it - it should totally work. The only way to figure out if a collection actually exists (or rather, if it is non-empty) is to request a list of non-empty collections and explicitly compare your name to this list : `if 'new_name' in db.list_collection_names()`

To create an **index** on a collection:
`db['collection_name'].create_index('key')`
Or, in a more fancy example:
`db['collection_name'].create_index([('key1': 1), ('key2': -1)])`

On storing **dates**: apparently Mongo has an internal date format, so storing dates as strings is not recommended. However unlike sql, it doesn't convert string queries to dates on the fly. So the best strategy is to convert all date-times pythonic **datetime** (see [[py_dates]]), then pass them to queries as variables. 

# Other

Judging from the fact that during index creation one can set parameters `expireAfterSeconds` and `weights` (for relatively weiging a bunch of fields while ordering), Mongo may be used for some sort of fuzzy queuing. (???)

# Refs

Reasonably structured documentation:
https://www.tutorialspoint.com/mongodb/mongodb_quick_guide.htm

On referencing: 
https://www.tutorialspoint.com/mongodb/mongodb_query_document.htm
https://www.w3schools.com/python/python_mongodb_query.asp

Indexing:
https://www.tutorialspoint.com/mongodb/mongodb_indexing.htm

pymongo
https://www.mongodb.com/blog/post/getting-started-with-python-and-mongodb