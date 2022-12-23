# Pandas

Parent: [[01_Tools]], [[python]]
See also: [[numpy]], [[py_dates]], [[sklearn]]

#tools #python


# Creation and IO

* Creation: `DataFrame(columns=['x','y'])` (note curious capitalization)
* `[[something]]` simply means "a list of lists". It's a type thing, not a separate syntax.
* To look into it: `df.head()`
* Read: `pd.read_csv('name.txt', sep='\t')`
* Write: `df.to_csv('name.csv', index=False)`

# Addressing

### Columns
* Get column names: `df.columns`
* Rename  only some columns: `df = df.rename({'a': 'X', 'b': 'Y'}, axis=1)`
* Rename all columns, just overwrite `.columns` with a different vector
    * Or, if chaining, do this: `.set_axis(['X','Y'], axis=1, inplace=False)`
* Delete some columns: `df = df.drop(['a','b'], axis=1)` or `df.drop(columns=['a'])`.

### Indexing
* Select columns by label: `df.x` or `df['x']`. Returns a series. To cast into a numpy object, add `.values` at the end (just like that, without parentheses). In most cases, dot-notation and parentheses-notation are equivalent, but there are some special cases when only parenthesis-notation works:
    * If your column name is a reserved word, like `count`, `first` or `last`. If you have a column named "first", `d['first']` would work well, but `d.first` won't
    * If column name contains a space (only `df.['my dog']` would work)
    * Column name is an integer rather than a string
    * You use dynamic addressing (`a = 'name'; df[a]`)
    * You want to assign values to a dataframe (see below)
* Two common useful patterns  is to use `.unique()` instead of `.values`if you want a list of unique values, and `.tolist()` after either, if you need a standard  list, and not a numpy array.

Overall, dot-notation looks neat, and saves you 3 characters, but over time I decided to phase it out and switch to parenthesis. The main reason is that with color highlighting, strings have a different color, and parenthesis notation typically shows string contansts inside, when you know the names of your columns. So it is much easier to quickly see where the same word is used repeatedly in the code. While with dot-notation, column names and methods llook identical, and this somehow feels wrong.

Other useful patterns:
* Select rows by label: `df.loc[1]`. Works for both df (returns a row-series), and for column-series (returns a single value).
* To cast a single value from a row-series into just a value, use `.values[0]` at the end.
* Out-of-range integers are forgiven (ignored) on reading, but cause an error if you try to write

To iterate through rows:
* either `for i in range(df.shape[0]): df.loc[i]`
* or `for key,val in df.iterrows()`  (in both cases we get row-series). 
* To slice into one-row slivery dataframes, one could also use `df.loc[[i]]`, but not sure if useful.

Footnotes:
* https://www.dataschool.io/pandas-dot-notation-vs-brackets/

### Conditionals and filtering
For conditional data retrieval we have a choice between: 

* **logical indexing** `df.loc[d.x>0]` Can take list comprehensions as an argument (instead of a series); can be written to; but slower, and harder to read.
* Another form of logical indexing: `df.x.eq(0)` (or things like `ne`, `le` etc.)
* **queries**: `df.query('x>0')`, where `x` is a name of a column. Easier for a human to read; works slightly faster, but cannot be written to. To reference a normal variable `a` in a query, use `@a` inside the query string. Also keep in mind that they create a view of the original dataframe, not a new dataframe, which saves time and space, but can lead to slice-assignment issues if you assume that the result is an independent dataframe of its own. It's not.
* To find the first row index that satisfies a criterion, follow with `.idxmax()` - it returns the location of the maximum (like `np.argmax()`), and in this case truth is the maximum.
* To find `None`-like objects (or their absence), use `	.notnull()`, and its opposite `.isnull()`. It seems that `isna()` and `notna()` also work, and are exact synonyms (I think?). Note also that it's `np.isnan()` in Numpy, but `isna()` in Pandas.
    * Note that while **queries** support stuff like `'x>0 | x<100'`, they don't support these na-related functions for some reason, unless you call with a  flourish. To filter out nans one has two options:
        * A hack: `query('x == x')`. This works, because NAs aren't equal to themselves!!
        * A proper fancy call:  `query(	'x.notna()', engine='python')`
* Useful ways to search by string:
    * `df.col1.str.match('Smth')` if you want strings that start with 'Smth'. 
    * `df.col1.str.contains('sub')` if you want strings that contain a substring. Both only work for queries if you add an `engine='python'` flourish.

To thin out a dataset, several options:
* One: `df = df[df.x>0]`.
* Another, `df = df.drop(df[df.x>0].index)`. This one seems bulkier, so not sure if it is ever preferred?
* Use a query (see above). `df = df.query('x>0')`, potentially with a `.copy()` at the end, to immediately cast (copy) a view of a parent dataframe into a separate dataframe.

Conditional indexing supports functions, as long as they take and return Pandas series, or something compatible, like a Numpy array). For example, conditional forms (both local indexing and queries) support elementwise Boolean operators, like `&` and `|`.

⚠️ There's a strange pitfall associated with conditional data retrieval that I doesn't understand for now. `query` seems to always create a view of a dataframe, which may look like a new dataframe with fewer rows, but that actualy acts as a copy, and not as a deepcopy. As a result, if you save the results in a "new" dataframe `df2 = df.query('x>0')`, and then change values in  `df2` in any way, it may case a warning "A value is trying to be set on a copy of a slice from a DataFrame". In this case df2 gets updated, and df seems still intact (unchanged), but there's this warning, and I'm not sure why. (Is it because they had to replace a view with a copy when you ran a command? So they want us to be more deliberate here?). Replacing a simple `query` with `df.query(...).copy()` turns a view into a legit new dataframe, and thus eliminates a warning.

### Chained Assignment problem

"Chained assignment" is a name for a problem that you get when writing to a section of a dataframe, when selecting by both column and row. In essence, if you manage to selecte both of them at once, you're good. But if you do first one, then another, then you get a view of a view, and you can read, but not write. The situationis complicated by the fact that in some cases even chained assignment may actually work (with a warning), but for a different dataframe the same code may crash.

* `df.loc[1,'x']` - Read and write. Reference both by column and row by index.
* `df.iloc[1,0]` - Read and write. Reference both by position. Row number goes first. 
* `df['x'][1]` - Only read.
* `df.x[1]`- Only read. (completely equivalent to the one above, just different syntax)
* `df.x.iloc[1]` - Officially Unpredictable (according to the manuals), so should be assumed to be "Only read"
* `df.iloc[1].x` - Officially Unpredictable, exactly like the one above.

Footnotes:
* https://stackoverflow.com/questions/29888341/a-value-is-trying-to-be-set-on-a-copy-of-a-slice-from-a-dataframe-warning-even-a

# Quick data exploration

Useful methods:
* `df.col.hist()` - draws a nice histogram
* `df.col.value_counts()` - outputs value-counts for a categorical variable.
* `df.describe(include='all')` - outputs a derivative dataframe with min-max-mean-std-count etc. summary. Without this `include='all'` clause doesn't include categorical variables, but only numerical ones.

# Data types

### Numbers and Boolean
* Cast to type: `.astype(int)`
* Vectorized not: `~` operator. For example, `~np.array([True,False])` is "F, T".
* Replace NaNs: `.fillna(0)` replaces all NaNs with zeroes. Can be run on a dataframe, or a series.
* Replace something else: use a numpy function: `.replace(old, new)`.
* Basic math with a series returns a series, so one can do `(df.a + 10).values` etc.

### Strings
* For basic string operations, like cut first chars: `df['x'].str[1:]`
* Other operations, all follow a paradigm of `df['x'].str.lower()`. They only work on a series, not on a dataframe. It also means that `df.x` won't work in this case, as it doesn't return a series. 
* To create a new column by combining other columns, use a normal `+` notation for strings, like `d['a'] = d['b'] + d['c']`. f-strings don't seem to work here though.
* To cast to a string: `df.x.astype(str)`
* `split` splits every string into an array of substrings, same as for normal Python.
* To remove leading spaces: `strip()`

### Regex

There's support of regular expressions (see [[regex]]) in `str`, such as:
* `.str.extract(r'(expr))` - extract part of a string that matches the pattern
* `.str.findall(r'...')` - only leave entries that match the pattern ??? 🔥
* `extractall` ?? 🔥

Footnotes:
* https://stackoverflow.com/questions/61822799/using-pandas-extract-regex-with-multiple-groups
* https://stackoverflow.com/questions/766372/python-non-greedy-regexes
* https://pandas.pydata.org/docs/reference/api/pandas.Series.str.extract.html

### Dates and times

Documentation (a bit wordy): https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html

The main data type for dates is a **Timestamp**, and it directly inherits to the `datetime.datetime` types (described in [[py_dates]]). Pandas however provides a much neater interface for manipulating these methods. It means that if you create a pandas-timestamp and a datatime-datetime for the same moment, they will be equal to each other, and `isinstance(dt, datetime.datetime)` will be true for both of them, even though `type()` will return different names, and formally types won't be equal to each other. A standard case of class inheritance.

The most useful methods are called on series of Timestamps using a prefix (similar to how it works for strings): for example `df.x.dt.dayofyear`. For example:
* `year`, `hour`, `day`, and other obvious words, that turn a a datastamp into an integer. Also: `dayofyear`, `weekofyear`, `dayofweek` (zero-indexed starting Monday). Note the absence of parentheses; it's really like that; it's a property, but not a method, apparently! (🔥 why?)
* `date()` and `time()` return a new series of either **Dates** or **Times** that are not quite Timestamps, but kinda related, and only partially functional datatypes. Most importantly, note that a Date is not just a rounded Datetime, and it does not inherit to a Timestamp, which may cause problems if one if substituted for the other.
* `dt.round('D')` - rounding to a day; similarly `floor` is for rounding down, and `ceil` for rounding up. Aliases for frequencies can be found here: https://pandas.pydata.org/docs/user_guide/timeseries.html#timeseries-offset-aliases
* To produce a nice string: `strftime('%w-%a')`; the format codes are described here: https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior
* To parse strings into stamps (an opposite to formatted output): `df.x = pd.to_datetime(df.x)` - it's surprisingly smart, and in most cases just magically works.
* To test if a date is special:`is_month_end`
* To generate a range of dates: `pd.date_range(start, end, frequency)` (for more parameters, see [manual](https://pandas.pydata.org/docs/reference/api/pandas.date_range.html)). Frequency may be either a simple code (like the default of `d`), or a fancy complicated offset (see below), which is needed for example to generate the 5th day of each month. Despite the name, generates proper full-blooded timestamps, and not just scrawny dates.

**Timedelta** is a separate class. It can be produced by subtraction of two stamps, and it can be added to a stamp. It also supports its own set of methods, such as `dt.days` (to express this difference as a number of days)	. Note the plural, and the absence of parentheses.
* Number of days from the smallest date: `(df.Date - df.Date.min()).dt.days`

**DateOffset** is another curious class that I don't quite understand, but it is necessary in these cases:
* To create a series of stamps: `pd.date_range(start, end, freq=pd.offsets.MonthBegin(1))`
* To round to the first day of every month:`df.date.dt.floor('d') + pd.offsets.MonthEnd(n=0)-pd.offsets.MonthBegin(n=1)`. This is more complicated than just rounding to a month, as months are not a real unit (they have variable size), which somehow makes this fancy context-sensitive construction necessary.

_A note on future deprecation_: apparently, the only part that will be deprecated is the `pd.datetime` class - one needs to really import `datetime.datetime` (see [[py_dates]]). All other interfaces, like `pd.to_datetime` for example will remain intact. (Refs: [1](https://gitlab.tudelft.nl/rhenning/ANTS-model/-/issues/28), [2](https://stackoverflow.com/questions/60856866/why-was-datetime-removed-from-pandas-1-0#:~:text=%22FutureWarning%3A%20The%20pandas.,Import%20from%20datetime%20module%20instead.%22))

# Data transformations

**Applying  arbitrary functions to every element of a column**
Several options here:
* **map**: `df.x = df.x.map(@function)`. For one-liners, works well with lambda notation. 
* `df.apply(@fun, axis=1)`, and it appears that with it one can write a function that is applied to every row of a dataframe (see chaining advice below).

**Updating certain values in a column**
The archetypical way seems to be using a `where` command, which is cool, but weird: first, it doesn't feel like a "where" keywords (it's more an "update" or "replace" kinda of a command), and second, the condition `where` gets is actually for those values that won't be updated. If the condition is true, `where` passes the value through, otherwise it replaces it with a default. Example: `df.x.where(df.x>0, -2)` will replace all values that are below zero with −2. Use `~` if need to flip the condition (some people even prefer writing it with a tilde, to make it more clear which values are updated, even tho it's obviously a mindhack).

**Creating a new column from old columns:**
There are least 6 different ways to do it, listed here from (arguably) best to worst:
* `df['z']=df.x+df.y` - that's what most people use, and it's one of the fastest approaches, but it has 2 draw-backs. One, it cannot be chained (using `.` notation). And two, when used on a view, it throws a "slice assignment" warning. So after things like `.agg` it will work just fine, but after a `.query` it will cause a problem (as `.query` creates a view!).
* `df = df.assign(z = df.x + df.y)` - this one is a bit slower, but can be chained! Many people prefer it, and it seems to be considered archetypical. In its simplest form (the one shown here) it can however only reference columns that existed in the original dataframe
* `df = df.assign(z = lambda d: d['x'] + d['y'])` - similar to the previous one, but as now it references an abstract dataframe, and not the original dataframe, this one can be fully chained, as it can reference columns created at the previous step.
* `df.insert(2, 'z', df.x+df.y)` - the fastest of all, but cannot be chained (it's in-place!), which also means that it doesn't feel Pandas-y. Also, you cannot run it 2 times in a row (returns an error, saying that this column already exists), and you need to know where to add this new column.
* `df.loc[:,'z']=...` - prevents slicing assignment warning, but is the slowest of all by far. May be good for updates though. Still not chainable though.
* `df = df.eval('z=x+y')` - chainable, but also slower. In principle, `eval` can also be run in-place, but then of course it becomse non-chainable, while still being slow.

In practice, in terms of both speed and readability (and maybe a bit of a balance between the two), I would recommend always using chaining, and thus relying on `eval` as much as possible (because of serialization it is actually quite fast). But as eval doesn't support some operations (for example, `eval`  cannot work with strings, or with `.shift`), we can always switch to `assign + lambda` for tougher cases. Assign _may_ also bring some performance boost, but this still needs to be investigated.

```python
df = (df
      .eval("z = x + y")
      .assign(w = lambda d: d['z'].astype(str) + ' hehe')
      .assign(v = lambda d: d['w'].shift(-1))
     )
```

Note that even `assign-lambda` does not support f-strings, so the best way to cast a non-string to a string seems to be use `.astype(str)`. (Using an f-string inside `eval` causes an error; inside `lambda` it doesn't cause an error, but is not serialized, and transform the entire series into one giant string.) Also, running `str(d.z)` does not cause an error, but doesn't do what we want, as it retains the type explanation at the end (debugging-style). Short of using a list comprehension, at the moment, I'm not sure how to plug f-string functionalify to pandas directly.

**Refs:**
* https://www.tutorialspoint.com/python_pandas/python_pandas_working_with_text_data.htm
* https://pandas.pydata.org/pandas-docs/stable/user_guide/text.html
* https://jakevdp.github.io/PythonDataScienceHandbook/03.12-performance-eval-and-query.html
* https://datatofish.com/integers-to-strings-dataframe/

# Joining and merging

* **Concatenation**: `df.concat(objs)` where objs is a list of dataframes. Parameters:
    * `axis`: default 0 (horizontal stacking), but may be something else
    * `join`: default value of `outer` (for union of matching indices), but can be changed to `inner` (for intersection). Makes sense for `axis=1` (when columns are merged): with `outer` missing values are padded with NaNs, while with `inner` incomplete rows are eliminated.
    * `keys`: generated hierarchical keys. Expects a list of names, to become the first level of these keys.
* A simplified wrapper for **adding homogeneous rows**: `f = f.append(f2)`. Takes either a one-row dataframe, or a dictionary. 
    * If indices are meaningful, use this notation, and it will check that they don't duplicate. If indices are essentially just row numbers, add `ignore_index=True` to make it more relaxed. And maybe also `sort=False`, to keep things simple and fast. Note that `ignore_index=True` seems to be necessary if we're supplying new rows as a dictionary (because by definition we don't have an index in this case?)
    * Remember that while `append()` sounds pythonic, it's actually not an in-place method, so we need to do `df = df.append(blabla)`.

An example of full-featured in-memory join: `pd.merge`. Archetypeical use:
        ```python
        res = df_left.merge(df_right[['key1', 'key2', 'col3']], 
                            how='left', 
                            on=['key1', 'key2'],
                            suffixes=[None, "_y"]
                           )
        ```

Some comments on joining and merging:

* We can also do `df_left2 = pd.merge(df_left, df_right, ...)`.
* `how` is one of the following: `inner` (default), `outer`, `left`, `right`.
* If keys are named differently in left and right dataframes, use `left_on='keyleft', right_on='keyright'`. If merging on more than one column, use a list (same as for `on`).
* If some columns are duplicated, inherits both, but modifies names. This can be changed by providing `suffixes` argument (a list). By default, adds `_x` and `_y` for left and right respectively. If you want one set of columns to retain old names, pass `None` as one of the elements of this list.
* If some rows are not unique, behavior can be modified with `validate` parameter.
* A diagnostic column may be created with `indicator=True` parameter. This creates a new column `_merge` with values of `left_only`, `right_only`, or `both`. 
* This can also be used in conjunction with `.query` and `.drop` to create exclusive joins; e.g. do a join, then only leave records from the left that weren't found in right).
* Instead of merging on an arbitrary column, we can also merge on index, which is controlled with `left_index=True` argument (and similar for the right). _Is it faster? Is it better?_
* To join only some columns, join with an incomplete dataframe (make df_left not a full dataframe, but select the columns you need using standard `df[['x','y']]` notation).
* To remove duplicates, do `.drop_duplicates()`

For **merging multiple dataframes** at once, set indices properly, and then do `pd.concat()` with a list of dataframes, and `join=inner` (or something else) argument. See documentation for details. This looks neater, and may be faster, than consecutive merges.

Footnotes:
* Documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html
* https://stackoverflow.com/questions/53645882/pandas-merging-101

# Grouping and Aggregation

First group, then apply aggregation. `dfs = df.groupby('g').agg({'a': ['min', np.mean]})`

**Grouping** columns (those that appear in the summary dataframe not because they were summarized, but because they were grouped by) also become a multiindex. It means that they aren't normal columns, and cannot be referenced directly. To turn into a "normal" data frame, do `dfs = dfs.reset_index()`.

**Aggregation** is coded as a dictionary, and more than one function can be applied to every column. Functions are passed as either an array of functions, or function names (strings, like `'count'`). If a column in the original dataframe is summarized in more than one way (say, if you calculate Column names in summary dataframe become a **multiindex** (column index becomes a tuple).
* To calculate a **mode**, use this trick: `mode = lambda x: x.value_counts().index[0]`, then pass `mode` (the function, not a string) as an argument to `agg`.

Some useful functions: sum, mean, min, max, count, var, std, sem, first, last, nth (?).

**Sorting** rows is done with `.sort_values(by='str')`, or a list of strings (column names). `ascending=True` by default.

`transform` is a really weird method that's helpful, but also feels a bit like a cheat. Normally it is used to call a function on a dataframe, producing a dataframe of a similar shape (like, apply something to every element). But with group objects it behaves like an agg followed by a reverse-merge by grouping factors. So instead of doing something like this (summarizing by column a, then reverse-merging to a, to get a total written in every row):

```python
df = pd.DataFrame({'a': [1,2,2], 'x': [1,2,3]})
df = df.merge(
        df.groupby('a').sum().reset_index().set_axis(['a', 'total'], axis=1),
        on='a', how='left'
        )
```  
one can just do this, and get the same result:
```python
df.loc[:,'total'] = df.groupby('a')['x'].transform('sum')
```
Combining this with `assign`, one can for example calculate subtotals in a middle of a complex chained calculation:
```python
df.assign(SUBTOTAL=lambda df : df.groupby('a')['x'].transform('sum'))
```

# Pivoting

* To go from wide to long format: `pd.melt()`. Gets dataframe, a list of columns to preserve (as `id_vars`), and optionally, new names for var and value columns (as `var_name` and `value_name`). Also some flexibility around indices.
* The opposite to melting (pivoting):
    * `df.pivot(index='factor1', columns='factor2', values='values')`. Each of them may also be a list of columns, in which case it creates nested indices. If some value is not available, it is filled with NaNs. Requires that every combo of factor1-factor2 were unique in the original table, or returns an error (so it doesn't aggregate, only reshapes the data. Aggregate manually first).
    * `df.pivot_table()` - same as `pivot` (so takes same parameters `index, columns, values`), but also can do simple aggregation (a parameter `aggfunc`) that can take values like `mean` (default), `count, first, max, min` etc. Which means that it handles duplicates, by default trying to silently average them.
    * There's also `wide_to_long` that is useful if the input table has columns with values that fit a certain prefix-suffix pattern(like `day1, day2, day3` or something like that). You provide it with these suffixes, it automates the transformation.

Footnotes:
* https://stackoverflow.com/questions/30960338/pandas-difference-between-pivot-and-pivot-table-why-is-only-pivot-table-workin

# Modeling

Interactions with [[sklearn]].

Create dummy variables from a categorical column:
```python
df = (pd.concat((df, pd.get_dummies(df[col_name], prefix=col_name)), axis=1)
      .drop(col_name, axis=1)
     )
```

# Chaining tips and tricks

How to change columns while chaining? It doesn't seem to be easy.

* `.eval("COL = OLD + OTHER + @variable")` or `.eval(f"COL = OLD + {variable}")` - seems to be the best way, but has some serious limitations:
    * doesn't support string methods on data
    * cannot work with variables that are dictionaries (I think)
* `.pipe(fun, arg1, arg2)` - allows user-written functions in chains, as long as this `fun` is happy to accept a dataframe, and return a dataframe.
* `.apply(lambda x: fun(x) if x.name=='col_name' else x)` - An ugly, but working way to apply a function to one column only. The function `fun(x)` needs to be ready to take the entire column as an interable, numpy-style, as x here is not a single value, but an entire column.
* `assign` seems to be bad for chaining, as you cannot reference newly created columns with it.

# Misc

To print a really long data frame without abbreviating the middle of it, do this:
```python
with pd.option_context('display.max_rows', 1400):
    print(dfs)
```

# Refs

Pandas advice from Chip Huyen
https://github.com/chiphuyen/just-pandas-things/blob/master/just-pandas-things.ipynb

Chaining:
* https://tomaugspurger.github.io/method-chaining.html