# Pandas

Parent: [[python]]
See also: [[numpy]], [[py_dates]]

#tools

# Open questions

* There's still some controversy with creating filtered dataframes using `query` (see below). I found a way to make it work correctly, and without warnings, but I still don't understand why the warnings appeared in the first place, and how adding `.copy()` happened to fix it.

# Creation and basic IO

* Creation: `DataFrame(columns=['x','y'])` (note curious capitalization)
* `[[something]]` simply means "a list of lists". It's a type thing, not a separate syntax.
* To look into it: `df.head()`
* Read: `pd.read_csv('name.txt', sep='\t')`
* Write: `df.to_csv('name.csv', index=False)`

# Addressing

#### Columns
* Get column names: `df.columns`
* Rename  only some columns: `df = df.rename({'a': 'X', 'b': 'Y'}, axis=1)`
* Rename all columns, just overwrite `.columns` with a different vector
    * Or, if chaining, do this: `.set_axis(['X','Y'], axis=1, inplace=False)`
* Delete some columns: `df = df.drop(['a','b'], axis=1)` or `df.drop(columns=['a'])`.

#### Indexing
* Select columns by label: `df.x` or `df['x']`. Returns a series. To cast into a numpy object, add `.values` at the end (just like that, without parentheses). Overall, we prefer dot-notation`df.x`, except for several special cases (⚠️): 
    * If your column name is a reserved word, like `count`, `first` or `last`. If you have a column named "first", `d['first']` would work well, but `d.first` won't
    * If column name contains a space (only `df.['my dog']` would work)
    * Column name is an integer rather than a string
    * You use dynamic addressing (`a = 'name'; df[a]`)
    * You want to assign values to a dataframe (see below)
    * Two common useful patterns  is to use `.unique()` instead of `.values`if you want a list of unique values, and `.tolist()` after either, if you need a standard  list, and not a numpy array.
* Select rows by label: `df.loc[1]`. Works for both df (returns a row-series), and for column-series (returns a single value).
* To cast a single value from a row-series into just a value, use `.values[0]` at the end.
* Out-of-range integers are forgiven (ignored) on reading, but cause an error if you try to write
* **Iterating through rows**: either `for i in range(df.shape[0]): df.loc[i]`, or `for key,val in df.iterrows()`  (in both cases we get row-series). To slice into slivery dataframes, one could use `df.loc[[i]]`, but usefulness of that is unclear.

Footnotes:
* https://www.dataschool.io/pandas-dot-notation-vs-brackets/

#### Conditionals and filtering
For **conditional data retrieval** we have a choice between: 
* **logical indexing** `df.loc[d.x>0]` Can take list comprehensions as an argument (instead of a series); can be written to; but slower, and harder to read.
* Another form of logical indexing: `df.x.eq(0)` (or things like `ne`, `le` etc.)
* **queries**: `df.query('x>0')`, where `x` is a name of a column. Easier for a human to read; works slightly faster, but cannot be written to. To reference a normal variable `a` in a query, use `@a` inside the query string. Also keep in mind that they create a view of the original dataframe, not a new dataframe, which saves time and space, but can lead to slice-assignment issues if you assume that the result is an independent dataframe of its own. It's not.
* To find the first row index that satisfies a criterion, follow with `.idxmax()` - it returns the location of the maximum (like `np.argmax()`), and in this case truth is the maximum.
* To find `None`-like objects (or their absence), use `	.notnull()`, and its opposite `.isnull()`. It seems that `isna()` and `notna()` also work, and are exact synonyms (I think?).
    * Note that while **queries** support stuff like `'x>0 | x<100'`, they don't support these na-related functions for some reason, unless you call with a certain flourish. So to filter out nans one has two options:
        * A hack: `query('x == x')`. This works, because NAs aren't equal to themselves!!
        * A proper fancy call:  `query(	'x.notna()', engine='python')`

To thin out a dataset, several options:
* One: `df = df[df.x>0]`.
* Another, `df = df.drop(df[df.x>0].index)`. This one seems bulkier, so not sure if it is ever preferred?
* Use a query (see above). `df = df.query('x>0')`

Conditional indexing supports functions, as long as they take and return Pandas series, or something compatible, like a Numpy array). For example, conditional forms (both local indexing and queries) support elementwise Boolean operators, like `&` and `|`.

⚠️ There's a strange pitfall associated with conditional data retrieval that I doesn't understand for now. `query` seems to create a new dataframe (fewer rows), but apparently (?) it acts as a copy, and not as a deepcopy. As a result, if you save the results in a "new" dataframe `df2 = df.query('x>0')`, and then set or transform values in  `df2` in any way, it causes a warning "A value is trying to be set on a copy of a slice from a DataFrame". Even though df2 gets updated, and df seems still intact (unchanged). Replacing a simple `query` with `df.query(...).copy()` seems to help ( works, and the warning is eliminated). By extension, it should not be a problem if you do some transforms in the same dot-pipeline as querying, as it is this other transform that would create a copy. But if querying is the only thing you do, always finish with a `copy()` (_or at least that's my current understanding_).

**Chained Assignment**
A problem while writing to a frame, selecting by both column and row.
* Good: reference both by label (index): `df.loc[1,'x']`
* Also Good: reference both by position:`df.iloc[1,0]`. Row goes first. 
* Read, but not write: `df.x[1]`, which is equivalent to `df['x'][1]`.
* Read but not write: Both `df.x.iloc[1]` and `df.iloc[1].x`. Documentation states that whether assigning values to any given slice would work or not is "officially unpredictable", so chained assignment should never be used.

Footnotes:
* https://stackoverflow.com/questions/29888341/a-value-is-trying-to-be-set-on-a-copy-of-a-slice-from-a-dataframe-warning-even-a

# Data transformations

**Numbers and logic**
* Cast to type: `.astype(int)`
* Vectorized not: `~` operator. For example, `~np.array([True,False])` is "F, T" obviously.

**Strings**
* For basic string operations, like cut first chars: `df['x'].str[1:]`
* Other operations, all follow a paradigm of `df['x'].str.lower()`. They only work on a series, not on a dataframe. It also means that `df.x` won't work in this case, as it doesn't return a series. 
* To create a new column by combining other columns, use a normal `+` notation for strings, like `d['a'] = d['b'] + d['c']`. f-strings don't seem to work here though.
* `split` splits every string into an array of substrings, same as for normal Python.
* There's a support of regular expressions (see [[regex]]) in `str`, such as `extract` (extracting part of a string that matches the pattern), `findall` (only leaving entries that match the pattern).

**Dates and times**
* Documentation (not too well organized; hard to find stuff): https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html
* Most useful methods are called on a datastamp series using a prefix (similar to how it works for strings): for example `df.x.dt.dayofyear` (note the absence of brackets; it's really like that). Other useful ones (beyond obvious `hour`, `day` etc.): `date()` and `time()` (these two return a new timestamp, not a number, and so have brackets!); `weekofyear`, `dayofweek` (zero-indexed starting Monday), `is_month_end` (subj) etc.
* To create as series of datastamps from a reasonable column of strings, use `df.x = pd.to_datetime(df.x)`.
* See also: [[py_dates]] for Pythonic work with dates in general

**Applying  arbitrary functions to every element of a column**
Several options here:
* **map**: `df.x = df.x.map(@function)`. For one-liners, works well with lambda notation. 
* `df.apply(@fun, axis=1)`, and it appears that with it one can write a function that is applied to every row of a dataframe, but I'm not quite sure.

**Updating certain values in a column**
The archetypical way seems to be using a `where` command, which is cool, but weird: first, it doesn't feel like a "where" keywords (it's more an "update" or "replace" kinda of a command), and second, the condition `where` gets is actually for those values that won't be updated. If the condition is true, `where` passes the value through, otherwise it replaces it with a default. Example: `df.x.where(df.x>0, -2)` will replace all values that are below zero with −2. Use `~` if need to flip the condition (some people even prefer writing it with a tilde, to make it more clear which values are updated, even tho it's obviously a mindhack).

**Creating a new column from old columns:**
There are least 6 different ways to do it, listed here from (arguably) best to worst:
* `df['z']=df.x+df.y` - that's what most people use, and it's one of the fastest approaches, but it has 2 draw-backs. One, it cannot be chained (using `.` notation). And two, when used on a view, it throws a "slice assignment" warning. So after things like `.agg` it will work just fine, but after a `.query` it will cause a problem (as `.query` creates a view!).
* `df=df.assign(z=df.x+df.y)` - this one is a bit slower, but can be chained! Many people prefer it, and consider it archetypical.
* `df.insert(2, 'z', df.x+df.y)` - the fastest of all, but cannot be chained (it's in-place), which also means that it doesn't feel Pandas-y. Also, you cannot run it 2 times in a row (returns an error, saying that this column already exists), and you need to know where to add this new column.
* `df.loc[:,'z']=...` - prevents slicing assignment warning, but is the slowest of all by far. May be good for updates though. Still not chainable though.
* `df.eval('z=x+y', inplace=True)` - I read somewhere that it is supposed faster than other methods, but in my experiments it is slower than almost any other method. So not sure why anyone would use it. Also, non-chainable.
* `df=df.eval('z=x+y', inplace=False)` - chainable, but also slower.

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

**Refs**
* Documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html
* https://stackoverflow.com/questions/53645882/pandas-merging-101

# Grouping and Aggregation

First group, then apply aggregation. `dfs = df.groupby('g').agg({'a': [min, np.mean]})`

**Aggregation** is coded as a dictionary, and more than one function can be applied to every column. Functions are passed as an array of functions (!!!), or function names (like `'count'`). If a column in the original dataframe is summarized in more than one way (say, if you calculate Column names in summary dataframe become a **multiindex** (when column index is a tuple).

**Grouping** columns (those that appear in the summary dataframe not because they were summarized, but because they were grouped by) also become a multiindex. It means that they aren't normal columns, and cannot be referenced directly. To turn into a "normal" data frame, do `dfs = dfs.reset_index()`.

**Sorting** rows is done with `.sort_values(by='str')`, or a list of strings (column names). `ascending=True` is a default, so change if necessary.

# Pivoting

* To go from wide to long format: `pd.melt()`. Gets dataframe, a list of columns to preserve (as `id_vars`), and optionally, new names for var and value columns (as `var_name` and `value_name`). Also some flexibility around indices.
* The opposite to melting (pivoting):
    * `df.pivot(index='factor1', columns='factor2', values='values')`. Each of them may also be a list of columns, in which case it creates nested indices. If some value is not available, it is filled with NaNs. Requires that every combo of factor1-factor2 were unique in the original table, or returns an error (so it doesn't aggregate, only reshapes the data. Aggregate manually first).
    * `df.pivot_table()` - same as `pivot` (so takes same parameters `index, columns, values`), but also can do simple aggregation (a parameter `aggfunc`) that can take values like `mean` (default), `count, first, max, min` etc. Which means that it handles duplicates, by default trying to silently average them.
    * There's also `wide_to_long` that is useful if the input table has columns with values that fit a certain prefix-suffix pattern(like `day1, day2, day3` or something like that). You provide it with these suffixes, it automates the transformation.

Footnotes:
* https://stackoverflow.com/questions/30960338/pandas-difference-between-pivot-and-pivot-table-why-is-only-pivot-table-workin

# Chaining

How to change columns in a chaining way? It doesn't seem to be easy. `assign` for example doesn't always work, as you cannot reference newly created columns in it (those made by `agg` or `pivot`). So we have the following options:
* Official version: `pipe(fun, arg1)` - allows user-written functions in chains, except that this `fun` shold be defined on a dataframe, and return dataframe. So it's not for quick apply-lambdas on a single column.
* Hack: `.apply(lambda x: fun(x) if x.name=='col_name' else x)` - not very pretty, but it works.

# Misc

To print a really long data frame, do this:
```python
with pd.option_context('display.max_rows', 1400):
    print(dfs)
```

# Refs

Pandas advice from Chip Huyen
https://github.com/chiphuyen/just-pandas-things/blob/master/just-pandas-things.ipynb

Chaining:
* https://tomaugspurger.github.io/method-chaining.html