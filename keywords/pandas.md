# Pandas

#tools

Parent: [[python]]
See also: [[numpy]]

# Open questions

* What's the recommended way to filter only some rows from a dataframe, based on their values? Most SQL-like? Are we supposed to use logical indexing, or are there methods that are recommended, either coz they are faster, or more user-transparent?
* There's still some controversy with creating filtered dataframes using `query` (see below). I found a way to make it work correctly, and without warnings, but I still don't understand why the warnings appeared in the first place, and how adding `.copy()` happened to fix it.

# Creation and basic IO

* Creation: `DataFrame(columns=['x','y'])` (curious capitalization)
* `[[something]]` simply means "a list of lists". It's a type thing, not a separate synax.
* To look into it: `df.head()`
* Read: `pd.read_csv('name.txt', sep='\t')`
* Write: `df.to_csv('name.csv', index=False)`

# Addressing

**Columns**
* Get column names: `df.columns`.
* Rename all columns, just overwrite it with a different vector.
* Rename  only some columns: `df = df.rename({'a': 'X', 'b': 'Y'}, axis=1)`
* Delete some columns: `df = df.drop(['a','b'], axis=1)` or `df.drop(columns=['a'])`.

**Indexing**
* Select columns by label: `df['x']`. Returns a series. To cast into a numpy object, add `.values` at the end (right like that, without parentheses). An alternative spelling `df.x` works in most cases, except if the name of the column is special (⚠️Gotcha: for example, `first` and `last` are reserved words, so if you have a column named "first", then `d['first']` would work well, but `d.first` won't)
    * Two common useful patterns here is to slam `.unique()` at the end if you want only unique values, and `.tolist()` if you need a standard Pythonic list.
* Select rows by label: `df.loc[1]`. Works for both df (returns a row-series), and for column-series (returns a single value).
* To cast a single value from a row-series into just a value, use `.values[0]` at the end.
* Out-of-range integers are forgiven (ignored) on reading, but cause an error if you try to write
* **Iterating through rows**: either `for i in range(df.shape[0]): df.loc[i]`, or `for key,val in df.iterrows()`  (in both cases we get row-series). To slice into slivery dataframes, one could use `df.loc[[i]]`, but usefulness of that is unclear.

For **conditional data retrieval** we have a choice between: 
* **logical indexing** `df.loc[d.x>0]` Can take list comprehensions as an argument (instead of a series); can be written to; but slower, and harder to read.
* Another form of logical indexing: `df.x.eq(0)` (or things like `ne`, `le` etc.)
* **queries**: `df.query('x>0')`, where `x` is a name of a column. Easier for a human to read; works slightly faster, but cannot be written to. To reference a normal variable `a` in a query, use `@a` inside the query string.
* To find the first row index that satisfies a criterion, follow with `.idxmax()` - it returns the location of the maximum (like `np.argmax()`), and in this case truth is the maximum.

Conditional indexing supports functions, as long as they take and return Pandas series, or something compatible, like a Numpy array). Both conditional forms support elementwise Boolean operators, like `&` and `|`.

There's a strange pitfall  ⚠️ associated with conditional data retrieval that I doesn't understand for now. `query` seems to create a new dataframe (fewer rows), but apparently (?) it acts as a copy, and not as a deepcopy. As a result, if you save the results in a "new" dataframe `df2 = df.query('x>0')`, and then set or transform values in  `df2` in any way, it causes a warning "A value is trying to be set on a copy of a slice from a DataFrame". Even though df2 gets updated, and df seems still intact (unchanged). Replacing a simple `query` with `df.query(...).copy()` seems to help (still works, but now also the warning is eliminated). _Yet I still don't understand that, and it bothers me._

**Chained Assignment**
A problem while writing to a frame, selecting by both column and row.
* Good: reference both by label (index): `df.loc[1,'x']`
* Also Good: reference both by position:`df.iloc[1,0]`. Row goes first. 
* Read, but not write: `df.x[1]`, which is equivalent to `df['x'][1]`.
* Read but not write: Both `df.x.iloc[1]` and `df.iloc[1].x`. Documentation states that whether any given slice would work or not is "officially unpredictable", so chaining should never be used.

Refs:
* https://stackoverflow.com/questions/29888341/a-value-is-trying-to-be-set-on-a-copy-of-a-slice-from-a-dataframe-warning-even-a

# Data transformations

**Strings**
* For basic string operations, like cut first chars: `df['x'].str[1:]`
* Other operations, all follow a paradigm of `df['x'].str.lower()`. They only work on a series, not on a dataframe. It also means that `df.x` won't work in this case, as it doesn't return a series. 
* To create a new column by combining other columns, use a normal `+` notation for strings, like `d['a'] = d['b'] + d['c']`. f-strings don't seem to work here though.
* `split` splits every string into an array of substrings, same as for normal Python.
* There's a support of regular expressions (see [[regex]]) in `str`, such as `extract` (extracting part of a string that matches the pattern), `findall` (only leaving entries that match the pattern).

**Numbers**
* Cast to type: `.astype(int)`

**Dates and times**
* Documentation (not too well organized; hard to find stuff): https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html
* Most useful methods are called on a datastamp series using a prefix (similar to how it works for strings): for example `df.x.dt.dayofyear` (note the absence of brackets; it's really like that). Other useful ones (beyond obvious `hour`, `day` etc.): `date()` and `time()` (these two return a new timestamp, not a number, and so have brackets!); `weekofyear`, `dayofweek` (zero-indexed starting Monday), `is_month_end` (subj) etc.
* To create as series of datastamps from a reasonable column of strings, use `df.x = pd.to_datetime(df.x)`.

**Applying functions to every element**
If transforming only one column: **map**: `df.x = df.x.map(@function)`. For one-liners, works well with lambda notation. There's also`df.apply(@fun, axis=1)`, and it appears that with it one can write a function that is applied to every row of a dataframe, but I'm not quite sure.

There's also `df.eval('C=A/B', inplace=True)` that is apparently much faster than explicit formulas on series, but this still needs to be tested / researched. #halfthere

**Refs:**
* https://www.tutorialspoint.com/python_pandas/python_pandas_working_with_text_data.htm
* https://pandas.pydata.org/pandas-docs/stable/user_guide/text.html
* https://jakevdp.github.io/PythonDataScienceHandbook/03.12-performance-eval-and-query.html

# Joining and merging

* **Concatenation**: `df.concat(objs)` where objs is a list of dataframes. Parameters:
    * `axis`: default 0 (horizontal stacking), but may be something else
    * `join`: default value of `outer` (for union of matching indices), but can be changed to `inner` (for intersection). Makes sense for `axis=1` (when columns are merged): with `outer` missing values are padded with NaNs, while with `inner` incomplete rows are eliminated.
    * `keys`: generated hierarchical keys. Expects a list of names, to become the first level of these keys.
* A simplified wrapper for **adding homogeneous rows**: `f = f.append(f2)`. Takes either a one-row dataframe, or a dictionary. 
    * If indices are meaningful, use this notation, and it will check that they don't duplicate. If indices are essentially just row numbers, add `ignore_index=True` to make it more relaxed. And maybe also `sort=False`, to keep things simple and fast.

Full-featured in-memory join: `pd.merge`. Archetypeical use:
        ```python
        res = df_left.merge(df_right[['key1', 'key2', 'col3']], 
                             how='right', 
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

**Refs**
* Documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html
* https://stackoverflow.com/questions/53645882/pandas-merging-101

# Grouping and Aggregation

First group, then apply aggregation. `dfs = df.groupby('g').agg({'a': [min, np.mean]})`

Aggregation is coded as a dictionary, and more than one function can be applied to every column. Functions are passed as an array of functions (!!!). If a column in the original dataframe is summarized in more than one way (say, if you calculate Column names in summary dataframe become a **multiindex** (when column index is a tuple).

Grouping columns (those that appear in the summary dataframe not because they were summarized, but because they were grouped by) also become a multiindex. It means that they aren't normal columns, and cannot be referenced directly. To turn into a "normal" data frame, do `dfs = dfs.reset_index()`.

# Pivoting

* To go from wide to long format: `pd.melt()`. Gets dataframe, a list of columns to preserve (as `id_vars`), and optionally, new names for var and value columns (as `var_name` and `value_name`). Also some flexibility around indices.
* The opposite to melting seems to be `pd.pivot_table()`. There's also `pd.pivot()` _and as of now I'm now sure what the difference would be. Check at some point? #todo_
* There's also `wide_to_long` that is useful if the input table has columns with suffices (like "day1, day2, day3" or something like that). You provide it with these suffices, it automates the transformation.

# Misc

To print a really long data frame, do this:
```python
with pd.option_context('display.max_rows', 1400):
    print(dfs)
```

# Refs

Pandas advice from Chip Huyen
https://github.com/chiphuyen/just-pandas-things/blob/master/just-pandas-things.ipynb