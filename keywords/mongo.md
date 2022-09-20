# Mongo

Path: [[database]], [[nosql]]

#db #tools


Most high-level conceptual topics about nosql database design are covered in [[nosql]]

The hierarchy of Mongo data (it's a tree, with several elements at each level):
* **Server**
* **Database**
* **Collection** - analogious to a table in relational databases.
* **Document** - this is roughly equivalent to a row in a large [[sql]] table. It probably corresponds to one instance / object, of whatever this database is supposed to track / represent
* **Key-value pairs** - these serve as columns in an [[sql]] database, except that every document may have different keys, so it's really not an orthogonal table, but a collection of somewhat similar objects. There's also no expectation that a shared key implies the same data type.

The only required key is `_id` - you either provide it, or Mongo invents it for you, if you don't. Unique for each document.

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
* AND: `{'$and': [EXPR1, EXPR2]}` (where `EXPR` shoud both have their own curly brackets, e.g. `[{'location': 'osaka'}, {'weather': 'sunny'}]`). Similarly, `$or`, `$not`, `$nor`

Another type of query, to find **distinct levels** for a field (kinda like unique in [[pandas]]):
```python
db.Collection_name.distinct(
    field : field_name,
    query : {'date': {'$gt': pd.to_datetime('2002-02-02')}}
)
```

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

# Getting info and troubleshooting
collection.estimated_document_count()
collection.find_one()
collection.index_information()
db.command('dbstats') # General info about the database (across all collections)
db.command('collstats', 'collection_name') # A shitload of stats about the structure and recent performance
collection.find(query).explain()['executionStats'] # To troubleshoot query performance

# Cleaning up
collection.drop() # Careful with it - just drops everything without warning, hesitation, tears...
```

As with pandas, instead of brackets-notation `db['collection_name']` one can use point-notation `db.collection_name`, but then collection name needs to be written explicitly, while with bracket-notation it can be stored in a variable (as a string).

While `find_one()` directly returns a document, a simple `find()` returns a **lazy iterator**, so we have to either iterate through it, or force full data retrieval using `list(result)`. If scared by the size of the possible output, chain it into `collection.find(query).limit(10)`. Note however that this operation is only fast if your query is permissive; if you are looking for something that is never true, mongo will have to look through _all_ records before realizing there's nothing to return.

To cast the output to [[pandas]], simply do `df = pd.DataFrame(list(result))`, as pandas like dictionaries. Given that the output is reasonably normalized (and maybe not too deeply nested? 🔥 ), it should work well. If the data is not normalized, and has lots of "stray fields" that are mentioned only once, Pandas will create a sparse column almost entirely filled witn nulls for each of these "stray fields", which is of course not ideal, as it will make the output very heavy.

To go in the opposite direction, and send pandas dataframe content to Mongo, use `df.to_dict('records')` as an argument for `insert_many()`. The `records` argument just tells [[pandas]] how to transform a df into a dict: by default it makes a dict of lists, where every column is an element of a dict, equal to a list. We howwever want a list of dictionaries, with each record (row) becoming a tiny dictionary.

To create a new collection, just pretend to "grab" a non-existing collection `db['new_name']` and try to write into it - it should totally work. The only way to figure out if a collection actually exists (or rather, whether it is non-empty) is to request a list of non-empty collections and  explicitly look for your name in this list : `if 'new_name' in db.list_collection_names()`

# Indexing

To create an **index** on a collection: `collection.create_index('key')`

Unlike in [[sql]], where indices are typically based on hashtables, in Mongo it mostly seems to be about physically ordering the records. Because of that, it's important if an index is created in an ascending (default) or descending order. To create a descending index, use `create_index(('key', -1))`.

> Note that [[javascript]]-like commands (often shown online in examples) use dict-like syntax here (something like `collection.createIndex({"key":-1})`), but for some weird reason Pymongo uses a list of tuples here, instead of a dictionary. So examples may need to be translated.

Mongo also supports fancy nested multi-property index, where records are first sorted by `key1`, then by `key2` etc. Do this to create a multiindex where the first key is ascending, and the second one is descending:
`collection.create_index([('key1', 1), ('key2', -1)])`

It is also possible to have one hashed index per collection. Hashed indices allow sharding, so it's better to be an index that breaks the collection well in sub-parts. It does not even has to be the first index in a multiindex; for example:
`collection.create_index([('key', 1), ('key2', 'hashed'), ('key3', -1)])`

When using `pymongo`, it's also possible to use sorting method of `pymongo.TEXT` instead of simple 1 and -1. It seems that with this option, one gets access to fancy fine-tuning of text searches, such as specifying how special characters are sorted, for example, in different languages, and for different alphabets (like, whether ä goes between a and b, or in the very end, etc.) In my very limited practice however it did not work _at all_, triggering a complete lookup across the entire database instead, for very normal, short text-based fields. Either I did something very wrong, or `pymongo.TEXT` works weirdly in compound indices. A simple `1` worked perfectly though. 

Not all fields can be used as hashed indices though; array-like fields cannot be used. If, upon creating an index, you try to use an array as a value for this field, you'll get an error. Nested fields can be used, as the hashfunction just collapses the nested structure into a dict-like string. In terms of sharding, generally, it is a bad idea to hash a key that has very few levels, or where the size of levels is very unevenly distributed (where there exist super-common default levels), as one level can only belong to one shard, which would create a bottleneck. Monotonically changing keys however are great for sharding, as they tend to be evenly distributable across shards. But either way, sharding doesn't happen automatically; there are special methods that need to be called to trigger sharding.

To see what indexes exist on a collection, do: `collection.index_information()`

The best way to troubleshoot an index is to feed some queries to the database, but instead of looking at the results, look at the statistics of execution using `collection.find(query).explain()['executionStats'] `. The key parameter here is how many rows were considered. For a correctly created index, the number of rows viewed should be almost the same as the number of rows ultimately found. 

🧿 A really weird thing about indexes that I see "experimentally" (in my practice) is that if an index contains 2 keys, each with more than one level, and you do `query={'key1': val1}`, Mongo will look through all records, and it will be super-slow. Essentially, in this case it does not use the index at all. But  if you do `query={'key1': val1, 'key2' : {'$ne' : 'something_weird'}}`, so basically "key2 is not equal to some totally made-up and impossible value", this second query ends up looking only all proper rows (those that have a correct value for the first key, but all possible values for the second key). So it starts using the index. Why wouldn't it use the idex for the first query, I cannot fathom.

Footnotes:
* https://www.mongodb.com/docs/manual/core/index-hashed/
* https://www.mongodb.com/docs/manual/core/sharding-shard-key/
* https://www.mongodb.com/docs/manual/core/hashed-sharding/
* https://www.mongodb.com/docs/manual/core/sharding-choose-a-shard-key/
* https://pymongo.readthedocs.io/en/stable/api/pymongo/collection.html#pymongo.collection.Collection.create_index

# Misc

Mongo has an internal date format, so storing dates as strings is not recommended. However unlike sql, it doesn't seem to convert string queries to "internal dates" on the fly. So the best strategy is to convert all date-times pythonic **datetime** (using either `datetime`, or its [[pandas]] interface: see [[py_dates]]), then pass them to queries as variables. 

Judging from the fact that during index creation one can set parameters `expireAfterSeconds` and `weights` (for weiging a bunch of fields while ordering), Mongo may support some sort of fuzzy queuing with timeouts. (??? 👁 )

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