# Spark and PySpark

Parents: [[database]], [[python]]
See also: [[parquet]]

#db


**ApacheSpark**: an open-source analytics engine for large-scale data processing. If I get it right (🔥 check!) the core idea is that there are shards of data distributed over physical locations in a cluster (aka Resilient Distributed Dataset, aka RDD). They are (at least for the main purpose? 🔥 _what does it mean?_) read-only. An individual operation on this multiset is performed as traversal (possibly parallelized) of a Directed Acyclical Graph (DAG), where every node is a piece of data, and each age is a transformation. For iterative processes (e.g. ML) you do it in a loop.

Requires a cluster manager and a distributed storage system (e.g. Hadoop). Has programming interfaces (libraries) for [[python]], Java, Scala, net, R, [[julia]]. Operations on RDDs are lazy (first coded, then implemented), and RDDs are immutable, so if the data changes - actually we just write a new RDD. The history of creating this RDD is stored, so if this RDD is lost, it can be reconstructed (fault-tolerance). The key API abstraction is called "Spark SQL"; then programming interfaces translate it to methods.

Variable broadcasting: if some data needs to be available on all nodes, it is possible.

Typically, Spark stores data in [[parquet]] files, but it's not a 1:1 match; both Spark can use other formats (say, json, or Avro from Hadoop project), and Parquet files are used for pickling and data transfer outside of any Spark context. Still, it appears that they were developed at least somewhat together, and synergize in terms of operatoins on selectd columns, and schema evolution (an ability to add data without rewriting everything).

# PySpark

Main object: PySpark DataFrame, created with `spark.createDataFrame()`, from a list of tuples (not a dict as for [[pandas]]). Schema may be implicit, or explicit. Can also create a spark df from a pandas df. Also:
* `spark.read.parquet('filename')` (or `.csv`, or some other formats)
* `df.write.parquet('filename')` - subj

All transformatoins are accumulated until an action, such as `collect()` is called. In particular:
* `collect()` tries to load df (the resulting one) in memory, leading to overflow if the data is too huge.
* `toPandas()` does the same, but also transforms it to Pandas.

Simple operations:
* `columns` returns the list of columns
* `.column_name` returns an object of "Column" type (not yet the data: it's lazy!)
* `select(df.column_name)` - returns a dataframe with this column. _I'm assuming it can also take a list of columns?_
* `withColumn('name', data)` - adds a column. Here `data` is likely to be another lazy Pyspark expression.
* `filter(df.column_name == 1)` - select a subset of rows, based on some conditional (in this case, simple comparison). Note that here we use Column object as if it were data and not just a pointer.

By default `.show()` only shows 1 line (because lazy), but it can be made eager with some special magic command (probably not the best idea tho, at least outside of testing notebooks).

Maximally flexible approaches that draw from Pandas-like functions (but I would assume, with questionalble performance?):
* `@pandas_utf` is a magic that turns a function on a pandas series into a functoin on a pyaspark column.
* `mapInPandas` is a method that takes a lazy function (an iterator, that yields, and not returns dataframes, and that has an interator of dataframes as an input) and applies it to a Pyspark dataframe. The function should be on a kind that is applicable per-shard, as it never sees the full dataframe as one whole.

Grouping:
* `groupby('col_name')` - groups by column. After that one can:
    * `.avg()` - apply the same aggregation function to all columns
    * Define a function on a pandas_df that runs aggregation methods on columns (`.mean()`, `.sum()` etc.), then apply this function using `applyInPandas`. Because Spark df is grouped, once it pretends to be pandas-like, these aggregation functons will work.
 
Instead of using ORM-style methods, one can also use [[sql]] on Spark DFs!
```python
df.createOrReplaceTempView('mytable') # This creates a view / interface
spark.sql("SELECT count(*) from mytable") # You get a df!
```
One can even define Pandas-style functions (as above), and then put them in these "sql" expressions!

SQL is also helpful for creating columns on the fly:
```python
from pyspark.sql.functions import expr
df.select(expr("count(*)") > 0) # creates a df with a subset of rows
```

# Refs