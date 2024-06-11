# Spark and PySpark

Parents: [[database]], [[python]]
See also: [[parquet]], [[pandas]]

#db #python


**ApacheSpark**: an open-source analytics engine for large-scale data processing. If I get it right, the core idea is that there are shards of data distributed over physical locations on a cluster (aka Resilient Distributed Dataset, or RDD). They are read-only. An individual operation on this multiset is performed as traversal (possibly parallelized) of a Directed Acyclical Graph (DAG), where every node is a piece of data, and each age is a transformation. For iterative processes (e.g. ML) you do it in a loop.

Requires a cluster manager and a distributed storage system (e.g. Hadoop). Has programming interfaces (libraries) for [[python]], Java, Scala, net, R, [[julia]]. Operations on RDDs are lazy (first coded, then implemented), and RDDs are immutable, so if the data changes - actually we just write a new RDD. The history of creating this RDD is stored, so if this RDD is lost, it can be reconstructed (fault-tolerance). The key API abstraction is called "Spark SQL"; then programming interfaces translate it to methods.

Variable broadcasting: if some data needs to be available on all nodes, it is possible.

Typically, Spark stores data in [[parquet]] files, but it's not a 1:1 match; both Spark can use other formats (say, json, or Avro from Hadoop project), and Parquet files are used for pickling and data transfer outside of any Spark context. Still, it appears that they were developed at least somewhat together, and synergize in terms of operatoins on selectd columns, and schema evolution (an ability to add data without rewriting everything).

# Python Spark

To run Pyspark locally, you need to install `pyspark` [[python]] package, but also have Java Developer Kit (jdk) installed. Google it.

For examples below to work, one needs to do lots of imports. Unfortunately there is no single easy handle, of a `np` or `pd` style. One can try to import all `sql.functions` as one, but some important classes are still lying outside of it.
```python
from pyspark.sql.functions import lit, monotonically_increasing_id,
	row_number
from pyspark.sql.window import Window
```

## Getting connected

Many cheatsheets and introductinos online reference things like `spark.createDataFrame`, and here `spark` is not a handle of the package (`pyspark` is most often imported as `ps`), but a session object. This session needs to be established first, and you need to either connect to an existing Apache Spark instance running somewhere, or to create a local session.

To create a session object:
`spark = ps.sql.SparkSession.builder.getOrCreate()`
If any extra parameters are needed, add this after `builder`:
`.config("some_config_option", "value")`

## Dataframes

Main object: Spark DataFrame, created with `spark.createDataFrame()`, from a list of `(key, value)` tuples (NOT a dict as for [[pandas]]). Keys here correspond to columns. The schema for the data may be both implicit (derived from the data on dataframe creation), or explicit. 

One can also create a Spark df from a Pandas df; that may be a good approach if you absolutely need to create a dataframe it from a dictionary.

To open some data into a dataframe:
* `spark.read.parquet('filename')` (or `.csv`, or some other formats)
* `df.write.parquet('filename')` - subj

Generally, all transformatoins on a dataframe are accumulated as commands (not executed eagerly) until an action, such as `collect()` is called. In particular:
* `.collect()` - tries to load the dataframe (the result of all transformations, if any) into memory, as a list. If it doesn't fit to memory, you get an overflow.
* `.toPandas()` - does the same, but also transforms it to Pandas.
* `.show()` - collects at least some data, and shows the dataframe. By default it only shows 1 line (because of lazy), but it can be made eager with some special magic command (probably not the best idea tho, at least outside of testing notebooks). If the dataframe is already fully in memory (for example, if it's a small dataframe that you just created), the entire df will be shown.

**Sampling:**

* `.first()` - returns the 1st row, as a pyspark Row type.
    * If you iterate through a Row as an iterable, you get all values, but no key names.
    * If you cast it to a dictionary with `.asDict()`, you can do `.items()` on this dict and iterate through `key, val` pairs.
* `.take(2)` - returns two first rows as a list of rows
* `.sample(with_replacement=False, fraction, random_seed)` - weird random sampling. The fraction should be treated as something very approximate, and the length of the result isn't guaranteed. Officially the faction should be `<1`, actually can be `<2`.

**Structure info**

To see the schema:
* `.` - for programming purposes (as a hierarchy of objects) 🔥
* `df.printSchema()` - in a human-readable almost-graphical way (in a notebook)
* `.columns` returns the list of column names (strings)
* `df.count()` - returns the number of rows. Note that it's not a harmless command, as it obviously triggers a calculation.
* `distinct()` - returns distinct rows

Footnotes:
* https://www.geeksforgeeks.org/how-to-iterate-over-rows-and-columns-in-pyspark-dataframe/

## Column transformations

* `df.select('some_column_name')` - as in [[sql]], returns a sub-table with these columns (also a dataframe). You can provide either a single name, or  alist of names, and unlike for square brackets in Pandas, `select('A')` and `select(['A'])` would give the same result.
* `df.drop("col_name")` or `df.drop(df.col_name)` - the opposite of select :)
* `withColumnRenamed('old_name', 'new_name')` - renames a column
* `withColumn('name', lit(None))` - adds an empty column. Using `lit(0)` will add you a column of zeros etc. You cannot specify different values here; if you manage to pass an iterable inside `lit()`, you'll just get this iterable as the same value in every row.

`.some_column_name` returns an object of "Column" type, similar to how in [[pandas]] it returned a series. But it's not yet the data itself, as the execution is lazy! It's not even an iterable somehow, and there seems to be no easy way to iterate thrown values of a column; most recommended solutions either collect the entire dataframe first, and iterate by rows, picking the proper column from each row, or even worse, cast the entire thing to Pandas.

## Concatenations

* `df1.union(df2)` - concatenates dataframes vertically, but would only work if columns match exactly
* `df1.unionByName(df2, allowMissingColumns=True)` - also concatenates dataframes vertically, but if any columns are not matching, forgives it and pads missing values with `null`s.

Quite tellingly, horizontal (column-wise) concatenation is impossible. Not only there is no simple function to perform it, but if you try to write it yourself, you'll find it impossible to do (short of casting to Pandas and back). For example, one could try to add a column of pseudo-indices using `monotonically_increasing_id()`, in a hope to join by them later, but then these ids won't match across 2 dataframes as they are paralellized and thus somewhat random. Or one could try to enumerate rows using
 `.withColumn('id', row_number().over(Window.orderBy('col_name')))` 
But the `col_name` would be different in both data frames, and then, what if there are repetitions in these values? Then the result of sorting is unpredictable, as it's parallelized. Can we calculate `row_number()` without sorting? No, it's impossible, as the result would have been non-deterministic, because of parallel calculations! This approach would have only worked if there are enough columns in both dataframes to sort the rows in exactly the same way… But then we could as well just created a real index (over several columns). In other words, two statements are true: 1) the order of rows in a dataframe is not defined, because of parallel processing, 2) indices are critical, as data can only can be combined and processed via joins (see below).

## Joins

* .

## Filtering

* `df.filter(df.my_column == 1)` - select a subset of rows, based on some condition (in this case, simple comparison). Note that here we use Column object as if it were data and not just a pointer.
    * Alternatively, the clause may be a string, referencing column names: `df.filter("age > 5")`
    * Or we can use an sql-function: `df.filter(sf.col("age") > 5)`
    * Note that `filter` is casting stuff before comparison, and in some cases it may cause weird effects. For example:
        * `1 == 1` is true (obviously, duh)
        * `1 == '1'` is also true, as string is cast to integer
        * `1 != '2'` is true (duh)
        * `1 == 'X'` is false, as when a letter is cast into an integer it's becoming some sort of a NaN
        * `1 != 'X' ` is however also false! Because you can't negotiate with NaNs! Which is different to how Pandas is treating it, for eample.
* `df.where()` - just a complete alias for `filter`; no new functionality
* `df.col_name.isNull()` - thats how to find nulls / NAs

## Columnar Calculations

Native Spark:
If the formula is simple, it can be added using `withColumn` and column references:
`.withColumn('NewColumn', df.X + df.Y**2)` - if there's a simple formula

For more complex formulas, we need to shift from a dataframe to a Resilient Distirbuted Dataset (RDD), calculate a new column there, then shift back to a dataframe. And as horizontal concatenation is impossible, it's not enough to only calculate a new column, we also need to carry over the elements of the index, or the calculation will be in vain. Because of that, the syntax is really shaped for vectorized output. Here's an example that copies the entire dataframe, and adds an extra column on the right:
```python
df = (df.rdd.map(
	lambda row: [row[i] for i in range(len(row))] +
    	[row.A + row.B ...] # Some fancy function
    ).toDF(df.columns + ['NEW_COLUMN'])
 )
```

Weird options (🔥 I'm not sure why would these would be needed in practice?)
* `.flatmap(func)` - exactly same thing, with `func` applied to every row, but after the application the result is flattened into a single long list (all rows kinda concatenated)
* `flatMapValues(func)` - again, first apply `func` to every row, then transform the dataframe into a list of `(key, value)` tuples, where "keys" are column names (🔥 ?? Is it true?)

Borrowed from Pandas: apparently, the most flexible approach. `@pandas_utf` is a magic that turns a function on a pandas series into a functoin on a pyaspark column. For example: 🔥🔥🔥
```python

```

`mapInPandas` is a method that takes a lazy function (an iterator, that yields, and not returns dataframes, and that has an interator of dataframes as an input) and applies it to a Pyspark dataframe. The function should be on a kind that is applicable per-shard, as it never sees the full dataframe as one whole.

## Grouping and summarizing

**Grouping**:
* `groupby('col_name')` - groups by column. Then one can apply summarizing functions from above. 

**Summarizing**
* `.mean()` (also `.avg()`), `min`, `max`, `sum`, `stdev`, `variance` - apply this summary function to every column of a datafrmae
* `.stats()` - most of these statistics above, all calculated at once
* `distinct()` - when run on a column, returns a column of unique values
* `.histogram(n_bins)` - apparently builds a historgram (🔥 should it be run on a column?)
* Define a function on a pandas_df that runs aggregation methods on columns (`.mean()`, `.sum()` etc.), then apply this function using `applyInPandas`. Because Spark df is grouped, once it pretends to be pandas-like, these aggregation functons will work.

# SQL interface
 
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