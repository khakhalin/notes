# Spark and PySpark

Parents: [[database]], [[python]]
See also: [[parquet]], [[pandas]]

#db #python


**ApacheSpark**: an open-source analytics engine for large-scale data processing. The core idea of sparks is that there are shards of data distributed over physical locations on a cluster (aka Resilient Distributed Dataset, or RDD). These shards are read-only, so we cannot damage them by editing: we can only create a new one, and delete a new one. An individual operation on this multiset is performed as traversal (possibly parallelized) of a Directed Acyclical Graph (DAG), where every node is a piece of data, and each age is a transformation. For iterative processes (e.g. ML) you do it in a loop.

Spark requires a cluster manager and a distributed storage system (e.g. Hadoop). Has programming interfaces (libraries) for [[python]], Java, Scala, net, R, [[julia]]. Operations on RDDs are lazy (first coded, then implemented), and RDDs are immutable, so if the data changes - actually we just write a new RDD. The history of creating this RDD is stored, so if some data is lost, it can be reconstructed from the previous stage (fault-tolerance). The key API abstraction is called "Spark SQL"; then programming interfaces translate it to methods.

RDD were the first thing to be developed in Spark, and they are are good for non-structured data, but these days people mostly use DataFrames, which inherit to RDDs, but have a table-like schema imposed on top. Individual rows of a dataframe, in this case, become those pieces of data that are distributed across RDDs. (🔥 is this description true? Or do we still dive from DF to RDD every now and then?)

Typically, Spark stores data in [[parquet]] files, but there's no 1:1 match between these two concepts; both Spark can use other formats (say, json, or Avro from Hadoop project), and Parquet files are used for data transfer and storage outside of any Spark context. Still, it appears that they were at least developed somewhat together, and synergize in terms of operatoins on selected columns, and schema evolution (an ability to add data without rewriting everything).

Footnotes:
* https://blog.purestorage.com/purely-educational/rdd-vs-dataframe-whats-the-difference/
* https://spark.apache.org/docs/latest/api/python/reference/api/pyspark.RDD.html

# Pyspark syntax

To run Pyspark locally, you need to install `pyspark` [[python]] package, but also have Java Developer Kit (jdk) installed. Google it.

For examples below to work, one needs to do lots of imports. Unfortunately there is no single easy handle them, of a `np` or `pd` style. One can try to import all `sql.functions` as one alias (ofteh `sf`), but some important classes are still lying outside of it.

```python
import pyspark.sql.functions as sf
from pyspark.sql.window import Window
```

## Session

Many cheatsheets and introductinos online reference things like `spark.createDataFrame`, and here `spark` is not a handle of the package, but a session object. This session needs to be established first, and then you need to either connect to an existing Apache Spark instance running somewhere, or to create a local session.

To create a session object:
```python
import pyspark as ps
spark = ps.sql.SparkSession.builder.getOrCreate()
```
If any extra parameters are needed, add this after `builder`, but before the `getOrCreate()` method:
`.config("some_config_option", "value")`

## Dataframes

Main object: Spark DataFrame, created with `spark.createDataFrame()`, from a list of `(key, value)` tuples (NOT a dict as for [[pandas]]!). Keys correspond to columns. The schema for the data may be both implicit (derived from the data on dataframe creation), or explicit. Unlike for Pandas, creating a generic "empty dataframe" seems plain impossible; the best that we can do is this:
```python
from pyspark.sql.types import StructType
spark.createDataFrame([], StructType([]))
```

One can also create a Spark df from a Pandas df using `spark.createDataFrame(pandasDF)`, which is often the best approach in practice, especially for small dataframes, as Pandas creation interface is just so much more reasonable. In at-scale applications however one needs to be careful, as Spark checks incoming Pandas-origin data for Nulls, which consumes resources.

To load extermal data into a dataframe:
* `spark.read.parquet('filename')` (or `.csv`, or some other formats)
* `df.write.parquet('filename')` - subj

Generally, all transformatoins on a dataframe are **lazy**: they are accumulated as an execution plan (DAG), and are NOT executed eagerly until an input-output **action** is called (a point when the calculation can no longer be postponed). Examples of actions:
* `.collect()` - tries to load the dataframe (the result of all transformations, if any) into memory, as a list. If it doesn't fit to memory, you get an overflow.
* `.toPandas()` - does the same, but also transforms it to Pandas.
* `limit(7)` - a DF of (first? random?) 7 rows. Goes well with `toPandas()` to peek inside in notebooks.
* `.show()` - collects at least some data, and shows the dataframe. By default it only shows 1 line (because of lazy), but it can be made eager with some special magic command (probably not the best idea tho, at least outside of testing notebooks). If the dataframe is already fully in memory (for example, if it's a small dataframe that you just created), the entire df will be shown.
    * `.display()` - a fancier kind of `show()` that only exists in Databricks notebooks, apparently, so it's probably not a part of standard pyspark
* `.save.parquet()` - obviously
* `.dropDuplicates(optional_list_of_columns)` - ⚠️ Note that it triggers a full calculation (or at least something very similar to it).
* `session.createDataFrame(df.rdd, df.schema)` - re-create a new Spark df with the same data as in the old df, but from scratch, with lineage broken.

This is both a curse and a blessing. A blessing because supposely an execution plan can be optimized by external optimizers (although personally I am yet to see it happen 🔥). A curse, because writing in this style means that you cannot refer to things like the number of rows in a dataframe, or unique values in a column, without triggering a calculation, and potentially runining everything. It turns a coding into a much more deliberatelly planned activity, which may feel weird.

There are also command that trigger a partial recalculation, or either a partial or full one, depending on the context. For example:
* `.count()` - that's a tricky one, as it's heavily optimized, and tries not to trigger a full calculation where possible.
    * It seems that on a one-node instance (without parallelization) it triggers it, and always returns a correct result.
    * If run on a multi-core instance, and combined with a command that explicitly collects information from all cores, for example `.cache().count()` (see "Caching" below) it also returns a correct result
    * ☢️ If run on a multi-core instance without an explicit collection, it may be off  however. This is impostant to remember when checking for duplicates, as the paradygmatic `if df.count() != df.dropDuplicates().count()` test may lie to you in some edge cases!

Footnotes:
* https://stackoverflow.com/questions/69038671/why-method-count-does-not-get-true-num-of-rows

**Structure info**

To interact the schema (that doesn't cause a recalculation):
* `df.printSchema()` - in a human-readable almost-graphical way (in a notebook)
* `.columns` returns the list of column names (strings)

To interact with values (these cause a partial recalculation, and so are NOT harmless commands, from the DAG optimization POV): 
* `df.count()` - returns the number of rows.
* `distinct()` - returns distinct rows. `select(row_name).distinct().collect()` returns a list of distinct values.

**Restructuring**

* `df1.union(df2)` - concatenates dataframes vertically, but would only work if columns match exactly
* `df1.unionByName(df2, allowMissingColumns=True)` - also concatenates dataframes vertically, but if any columns are not matching, forgives it and pads missing values with `null`s.

Quite tellingly, horizontal (column-wise) concatenation is impossible. Not only there is no simple function to perform it, but if you try to write it yourself, you'll find it impossible to do (short of casting to Pandas and back). For example, one could try to add a column of pseudo-indices using `monotonically_increasing_id()`, in a hope to join by them later, but then these ids won't match across 2 dataframes as they are paralellized and thus somewhat random. Or one could try to enumerate rows using
 `.withColumn('id', row_number().over(Window.orderBy('col_name')))` 
But the `col_name` would be different in both data frames, and then, what if there are repetitions in these values? Then the result of sorting is unpredictable, as it's parallelized. Can we calculate `row_number()` without sorting? No, it's impossible, as the result would have been non-deterministic, because of parallel calculations! This approach would have only worked if there are enough columns in both dataframes to sort the rows in exactly the same way… But then we could as well just created a real index (over several columns). In other words, two statements are true: 1) the order of rows in a dataframe is not defined, because of parallel processing, 2) indices are critical, as data can only can be combined and processed via joins (see below).

Footnotes:
* https://www.geeksforgeeks.org/how-to-iterate-over-rows-and-columns-in-pyspark-dataframe/

**Sampling**

* `.first()` - returns the 1st row, as a pyspark Row type.
    * If you iterate through a Row as an iterable, you get all values, but no key names.
    * If you cast it to a dictionary with `.asDict()`, you can do `.items()` on this dict and iterate through `key, val` pairs.
* `.take(2)` - returns two first rows as a list of rows
* `.sample(with_replacement=False, fraction, random_seed)` - weird random sampling. The fraction should be treated as something very approximate, and the length of the result isn't guaranteed. Officially the faction should be `<1`, actually can be `<2`.

## Columnar calculations

Basic adding and deleting:
* `df.select('some_column_name')` - as in [[sql]], returns a sub-table with these columns (also a dataframe). You can provide either a single name, or  alist of names, and unlike for square brackets in Pandas, `select('A')` and `select(['A'])` would give the same result.
* `df.drop("col_name")` or `df.drop(df.col_name)` - the opposite of select :) To drop several columns, either list them separated by columns, or unroll a list with `*[col1, col2]`.
* `withColumnRenamed('old_name', 'new_name')` - renames a column
* `withColumn('name', lit(None))` - adds an empty column. Using `lit(0)` will add you a column of zeros etc. You cannot specify different values here; if you manage to pass an iterable inside `lit()`, you'll just get this iterable as the same value in every row.

**Referring to columns:**
* `.some_column_name` returns an object of "Column" type, similar to how in [[pandas]] it returned a series. But it's not yet the data itself, as the execution is lazy! It's not even an iterable, and there seems to be no easy way to iterate thrown values of a column (as values within a dataframe are not ordered!). If you need to iterate, you have to either collect (eagerly calculate) the entire dataframe, and then iterate by rows, picking the proper column from each row, or cast the entire thing to Pandas.
* if the name is a variable, you use `sf.col(col_name)` where `sf` comes from `import pyspark.sql.functions as sf`

**Using formulas**: If the formula is simple enough, it can be added in native pyspark using `withColumn` and column references:
`.withColumn('NewColumn', df.X + df.Y**2)`. Spark functions has all the main math, string etc. functions implemented, so in most cases that's enough.
* **Dates:**
    * `date_add(x, -1)` - shift by one day in the past
    * `current_date()` and `current_timestamp()`
    * `datediff`
    * `date_trunc('str', d)` where `str` may be `year`, `quarter`, `month`, `week`, `day`, `hour`, `minute`, `second` (and a few smaller ones)
    * `date_part('str', d)` - to extract a part of a date as a number
    * `dayofyear()`
    * `date_format(date, format)` - to string

Conditional formulas:
* `.withColumn('b', sf.when(sf.col('a')>0, 1).otherwise(0))`

For more complex formulas, we need to shift from a dataframe to a Resilient Distirbuted Dataset (RDD), calculate a new column there, then shift back to a dataframe. And because horizontal concatenation is impossible, it's not enough to only calculate a new column, but we need to carry over the elements of the index, or the calculation will be in vain. Because of that, the syntax is really shaped for vectorized output. Here's an example that copies the entire dataframe, then adds one extra column on the right:
```python
df = (df.rdd.map(
	lambda row: [row[i] for i in range(len(row))] +  # This part copies all old columns
    	[row.A + row.B ...]  # This one adds a new column (that's where you put fancy stuff)
    ).toDF(df.columns + ['NEW_COLUMN_NAME'])
 )
```

Weird options (🔥 I'm not sure why would these would be needed in practice?)
* `.flatmap(func)` - exactly same thing, with `func` applied to every row, but after the application the result is flattened into a single long list (all rows kinda concatenated)
* `flatMapValues(func)` - again, first apply `func` to every row, then transform the dataframe into a list of `(key, value)` tuples, where "keys" are column names (🔥 ?? Is it true?)

Finally, it is possible to borrow functionality from Pandas using a so-called **Pandas UTF**. This is the most flexible approach, but it comes with an overhead cost. `@pandas_utf` is a magic that turns a function on an imagined pandas series into a function on pyspark columns. For example: 🔥🔥🔥
```python

```

`mapInPandas` is another (???? 🔥🔥🔥) method that takes a lazy function (an iterator, that yields, and not returns dataframes, and that has an interator of dataframes as an input) and applies it to a Pyspark dataframe. The function should be on a kind that is applicable per-shard, as it never sees the full dataframe as one whole.

## Joins

`df.join(other, on=..., how=...)` - basic join
* `on` can be a string (single column name), a list of column names, a column-like expression (a vectorized expression on existing columns that returns a boolean spark series), or even a list of them.
* `how` includes:
    * `left`, `right`, `inner` - these are clear. `inner` is also the default one.
    * `outer`, `full`, `fullouter` - these all mean the same, and it's just about trying to match, but then including all columns in the output, both those that matched and those that didn't, from both sources
    * `cross` - cartesian product of left and right df, without a filter
    * `left_outer`, `right_outer` - aliases for left and right respectively
    *  `semi`, `left_semi` - don't bring any columns from the right df, but only include those rows from the left df that had a match on the right. Essentially, use joining for filtering.
    *  `anti`, `left_anti` - the opposite of `semi`: as if you did a left merge, but then dropped all rows that found a match. And also deleted the columns from the right df.
* Examples:
    * `df.join(df2, [df.name == df2.customer, df.age == df2.value])`

Other interesting points:
* Spark supports anti-joins, like `how="left_anti"`, which includes all rows from the 1st df that did NOT have a match in the 2nd df.

Footnotes
* https://iomete.com/resources/reference/pyspark/pyspark-join

## Filtering

* `df.filter(df.my_column == 1)` - select a subset of rows, based on some condition (in this case, simple comparison). Note that here we use Column object as if it were data and not just a pointer.
    * Alternatively, the clause may be a string, referencing column names: `df.filter("age > 5")`
    * Or we can use an sql-function: `df.filter(sf.col("age") > 5)`. Note that at least Databricks somehow differentiates between single and double-quotes here, and requires the use of double-quotes.
    * Note that `filter` is casting stuff before comparison, and in some cases it may cause weird effects. For example:
        * `1 == 1` is true (obviously, duh)
        * `1 == '1'` is also true, as string is cast to integer
        * `1 != '2'` is true (duh)
        * `1 == 'X'` is false, as when a letter is cast into an integer it's becoming some sort of a NaN
        * `1 != 'X' ` is however also false! Because you can't negotiate with NaNs! Which is different to how Pandas is treating it, for eample.
* `df.where()` - just a complete alias for `filter`; no new functionality
* `df.col_name.isNull()` - thats how to find nulls / NAs

An operation that is somewhat akin to filtering: **drop_duplicates**
* Simple `drop_duplicates` is usually safe, but triggers (almost?) a full calculation of thd df, and so shouldn't be thrown around casually. In most cases, for large dataframes, some kind of merging and/or summarizing provides a better performance.
* `drop_duplicate(['col1', 'col2'])` allows to drop duplicates only on a selected column, and it is a dangerous operation (☢️) as if the df is broken across partitions, these will be processed separately, and then summarized, which means that it is not guaranteed which rows will end up in the final df. On one partition it would have been the 1st one for every occurance of `col1, col2`, but with many partitions it's unpredictable, and also depends on the number of cores on which the code runs.
    * One option that seems to work (and is ok for small dataframes): do `.coalesce(1)` and sorting `.orderBy('col1')` before dropping duplicates.
    * Another otion, to use window functions instead (see below); for example, instead of `.drop_duplicates(['col'])` do:
    ```python
   df = (
       df.withColumn('rank', rank().over(
           Window.partitionBy('col').orderBy('date')
           ))
       .filter(col('rank')==1)
       .drop('rank')
    )
     ```   

## Grouping and summarizing

**Grouping**:
* `groupby(*list_of_columns)` - groups by column. Then one can apply summarizing functions from above. 

**Summarizing**
* `.mean()` (without `agg`) - apply this summary function to every column of a dataframe
* `.agg(sf.mean("col_name").alias("mean_col_name"))` - apply this summary function to a column. (also `.avg()`), `min`, `max`, `count`, `sum`, `stdev`, `variance`
* To calculate several different things, separate them with `,` inside the `agg`.
* `.agg({'col_name': "max'})` - another way to run aggregation on a single column. Regardless of how you run it (with a list of operations, or a dictionary), you get a column with one value, and oddly named. If you run `head()` on this column, you get a `Row` of values. You can access elements of a row as if it were a list, but to turn it to a real list, unfortunately, you have to use a list comprehension.
* `.collect_set` - another type of an aggregation function that collects values within a group into one vector. ⚠️ The sequence of rows in a dataframe is famously non-deterministic, so if you care about the exact values of these vectors at all (for example, if you ever plan to group by them, or perform a `drop_duplicates` on them), you _have to_ use `.sf.array_sort(sf.collect_set(col))` instead of a naked `collect_set(col)` to sort the elements in the set post-factum.

Operations transforming a data column into a summary column:
* `.stats()` - most of the classic "min-max-mean" statistics, all calculated at once, and returned as a summary table
* `.select('col_name').distinct()` - returns a column of unique values
* `.histogram(n_bins)` - apparently builds a historgram (🔥 should it be run on a column?)

Deferring to Pandas: (🔥 does this disrupt the DAG?)
* Define a function on a `pandas_df` that runs aggregation methods on columns (`.mean()`, `.sum()` etc.), then apply this function using `applyInPandas`. Because Spark df is grouped, once it pretends to be pandas-like, these aggregation functons will work.

## Window functions

Once you 🔥🔥🔥🔥🔥🔥

Footnotes:
* https://medium.com/@suffyan.asad1/an-introduction-to-window-functions-in-apache-spark-with-examples-66cb06b06e1f

## SQL query interface
 
Instead of relying on ORM-style methods, one can also use [[sql]] queries on Spark DFs:
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

# Optimization

## Under the hood

In practice, a calculation in Spark consists of **Jobs**. The definition of a Job is that is always starts and finishes with an **action**: 🔥

Each job consists of **Stages**. Stage is defined as the minimal set of tasks that can be executed in parallel to each other, so they are naturally bound by wide transformations, such as grouping or joining. The stages are arranged into a computational graph (aka **DAG**) that can be visualized by UIs (see below). Each vertix of this graph is an RDD or DF (because we cannot change data, we're always creating new data), while edges are transformations (but usually in UIs the transformation is written near the vertex, even though it's kinda referring to the incoming edge). Spark tries to optimize the DAG, by moving things around (e.g. moving filtering upstream to reduce data size as early as possible), and then uses this DAG to optimize partitioning (where possible).

Stages consist of **tasks**, or individual operations. The tasks are distributed across **Executors** to allow parallelization. If everything is right, all executors should be involved in calculations to about the same degree (if not, you're having bottlenecks). Parallelization is based on **partitions** on the data, and it can in simple cases happen automatically.

Every task can succeed or fail, but also it may be **skipped** 🔥

Each stage can also be characterized by amounts of **Shuffle Read** and **Shuffle Write** (in Bytes), or moving data across partitions.

Footnotes:
* https://best-practice-and-impact.github.io/ons-spark/spark-concepts/spark-application-and-ui.html

## SparkUI and debugging

When working with UI, for stages, we can see a colored stacked bar showing stage execution. Here green (computing time) is typically good, while all other colors (orange shuffling, blue waiting for your turn to compute etc.) are bad.

There are some useful context methods that allow sending information to SparkUI from the code. All of them are called on Spark Context `sc = spark.SparkContext` where `spark` is the current spark session handle.
* `sc.setJobDescription()` - the main way of setting a context job description for operations started (Spark jobs triggered) after this context variable was set. To go back to default values (e.g. after leaving a block of code) do `.setJobDescription(None)`.
* setName
* `sc.setLocalProperty("callSite.short", "My words here")` - potentially may be doing the same thing as `setJobDescription()`, but only changing the short version, that is shown on the "many jobs" page.
* `sc.setLocalProperty("callSite.long", "More words")` - a similar thing, but for a longer description that you get if you click on a job.
* `sc.setJobGroup()` - allegedly, should set a group of jobs (until reset or changed), and all jobs created after that will grouped with this one. 🔥 In practice, for now, it didn't work for me.

Footnotes:
* https://spark.apache.org/docs/latest/web-ui.html
* https://books.japila.pl/apache-spark-internals/SparkContext/#local-properties
* https://best-practice-and-impact.github.io/ons-spark/spark-functions/job-description-notebook.html
* https://api-docs.databricks.com/python/pyspark/latest/api/pyspark.SparkContext.setJobGroup.html
* https://stackoverflow.com/questions/41903342/how-to-change-job-stage-description-in-web-ui

## Partitioning

Unlike some fancier tools, by default Spark won't perform any optimization of merges and joins (🔥 ?) . For example, if you are merging on a broader category and a date, a smart thing would be to perform a partition on the category, sort on the date, and then merge on the date within each of the partitions independently. But Spark won't do it, unless you tell it to, using these tools:
* `partitionBy(*cols)` - uses the list of columns to partition the df in memory, based on unique values. As the execution is lazy, it basically just defines the expected partition for the next method that can actually use a partitition, such as a join, or saving on disk (in which case it writes data in a partitioned, split way). For a join to work, both left and right dataframes need to be partitioned, on same columns.
* `repartition(n_partitions, *cols)` - looks like the same thing (?🔥) but with an ability to manually set the number of shards to use (`n`).
* `repartitionByRange(n_partitions, col)` - for a non-categorical but sortable column, breaks the df into `n` partitions based on the values of these column, so that each partition has a unique range of values.
* `.coalesce(n_partitions)` - the opposite of `repartition` that glues smaller partitions into large ones, without reshuffling.

## Other Join optimizations

* **Broadcasting**: instead of doing `.join(other_df ...)` one can do `.join(broadcast(other_df) ...)` in which case a full copy of `other_df` (presumably, a small dataframe) will be provided to every shard of the left data frame (big one, the one we're joining on). When broadcasting is possible, the join becomes very fast.
* **Salting**: it's not a function per se, but an approach. When partitioning by range, we may run into data skew situations with some values being heavily overrepresented in the data. Because of that, a shard containing these values will be way larger than the others, and will become a bottleneck. One option to fight with it is to _salt_ (somewhat randomize) one of the values. Another option is to manually isolate the most common value and process it separately (e.g. if a column contains lots of `NULL`s, join this part of the data separately, then concatenate)
* `DataFrame.sort(*cols)` (there's also an alias `.orderBy()` with same inputs and same functionality). Also `RDD.sortBy` exists, apparently… - 🔥 It seems that pre-sorting actually does't help with joining, as joining always starts with shuffling (sorting). But this needs to be confirmed. 🔥 

The sequence in which joins are performed is (apparently? 🔥) important even without partitions, and it is not optimized by default. Unles you turn optimizers on:
```python
spark.conf.set("spark.sql.cbo.enabled", True)
spark.conf.set("spark.sql.cbo.joinReorder.enabled", True)
```

Another cool tool for performance optimization: instead of collecting the output, calling `.explain()`, to force Spark to reveal its caculation strategy.

Automatic repartitioning before a join is called "hashpartitioning()" on the execution plan (in UI)

Footnotes:
* https://stackoverflow.com/questions/62489559/order-of-table-joining-in-sparksql-for-better-performance

## Persisting

The main idea of persisting is that in complicated computational schemes 2 (or more) calculation branches within a DAG can be executed in parallel, but still refer to the same upstream dataframe. In this case both branches will order its calculation, leading to a duplication. Unless we pre-calculate and cache it. Afaik it's a rather rare scenario though (🔥 is it?)

Or, at a different scale, two different sql queries can requst the same df. The optimizer cannot optimize across queries (?🔥 right?), so of course it will recalculate the dataframe. Unless we save (persist) it after it was calculated the first time around.

* `df.persist()` - the main command that persists a dataframe, with a bunch of bunch of parameters that describe where and how to persist it (in memory, as a [[parquet]] file, or in some other way). 🔥 Saving as a file is of course way more efficient in terms of memory management, but takes time on input-output, and also requires some claeanup at some point.
* `df.cache()` - allegedly, just a shortcut for `persist` in memory
* `df.unpersist()` - resets persisting for this dataframe in particular.

**Checkpointing** is a fancier method:
* `df = df.checkpoint()` - checkpoints a dataframe, in a sequence of "transformations" (that are not really transformations, as we remember, but planning of a computation DG). If a dataframe is contstructed iteratively, with later transformations repeatedly referencing new columns introduced earlier, it's a good idea to checkpoint a dataframe-in-construction, introducing a cachable boundary.
    * Note that this `.checkpoint()` syntax seems to be not the minimal code: there needs to be some setting of options early on.
    * Also, in practice, it creates new columns that are not automatically deleted.

**Staging** is yet fancier, and involves manual (?) saving of the table-in-construction to storage as a [[parquet]] file, at some key points during the calculation. For now it's not described here, as it's a bit too involved, but see the references below. 🔥

Footnotes:
* https://best-practice-and-impact.github.io/ons-spark/spark-concepts/persistence.html
* https://best-practice-and-impact.github.io/ons-spark/raw-notebooks/checkpoint-staging/checkpoint-staging.html

# Refs

https://books.japila.pl/apache-spark-internals/
A strange personal site with Spark documentation and advice that looks both promising and weirdly unfinished.

High-Performance Spark by Karau, Warren
https://www.oreilly.com/library/view/high-performance-spark/9781491943199/
