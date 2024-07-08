# Pandas

Parent: [[01_Tools]], [[python]]
See also: [[numpy]], [[py_dates]], [[sklearn]], [[matplotlib]]

#tools #python


# Creation and IO

* Creation: `DataFrame(columns=['x','y'])` (note curious capitalization)
* `[[something]]` simply means "a list of lists". It's a type thing, not a separate syntax.
* To look into it: `df.head()`
* Read: `pd.read_csv('name.txt', sep='\t')`
* Write: `df.to_csv('name.csv', index=False)`

# Addressing

## Columns
* Get column names: `df.columns`
* Rename  only some columns: `df = df.rename({'a': 'X', 'b': 'Y'}, axis=1)`
* Rename all columns, just overwrite `.columns` with a different vector
    * Or, if chaining, do this: `.set_axis(['X','Y'], axis=1, inplace=False)`
* Delete some columns: `df = df.drop(['a','b'], axis=1)` or `df.drop(columns=['a'])`.

## Indexing
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

## Quering and filtering
For conditional data retrieval we have a choice between: 

* **logical indexing** `df.loc[d.x>0]` Can take list comprehensions as an argument (instead of a series); can be written to; but slower, and harder to read.
* Another form of logical indexing: `df.x.eq(0)` (or things like `ne`, `le` etc.)
* **queries**: `df.query('x>0')`, where `x` is a name of a column. Easier for a human to read; works slightly faster, but cannot be written to. To reference a normal variable `a` in a query, use `@a` inside the query string. Also keep in mind that they create a view of the original dataframe, not a new dataframe, which saves time and space, but can lead to slice-assignment issues if you assume that the result is an independent dataframe of its own. It's not.
* To find the first row index that satisfies a criterion, follow with `.idxmax()` - it returns the location of the maximum (like `np.argmax()`), and in this case truth is the maximum.
* To find `None`-like objects (or their absence), use `	.notnull()`, and its opposite `.isnull()`. At least for numerical columns, `isna()` and `notna()` also work, but not sure if they are exact synonyms (🔥). Note also that it's `np.isnan()` in Numpy, but `isna()` in Pandas.
    * `pd.isnull(x)` also works great if you extracted the value from Pandas. Unlike `np` that only supports numbers and NAs, this one also works with strings and dates.
    * Note that while **queries** support stuff like `'x>0 | x<100'`, they don't support these na-related functions for some reason, unless you call them with a  "fancy call". To filter out nans one has two options:
        * Hacky but recommended solution: `query('x == x')`. This works, coz NAs aren't equal to themselves!!
        * Fancy call:  `query('x.notna()', engine='python')`
* To search a particualr string:
    * `df.col1.str.match('Smth')` if you want strings that start with 'Smth'. 
    * `df.col1.str.contains('sub')` if you want strings that contain a substring. Both only work for queries if you add an `engine='python'` flourish.
* A vectorized version of `x in a` expression looks like `.isin(a)`

To thin out a dataset, several options:
* `df = df[df.x>0]` - classic Python, common.
* `df = df.drop(df[df.x>0].index)` - looks bulky and weird, does anyone do it?
* drop in-place (not sure if common)
* `df = df.query('x>0')`- query, good for chaining. You could also add a `.copy()` at the end, to explicitly copy a view of a parent dataframe into a new dataframe, if you plan to keep changing it. In practice it seems to always copy the dataframe anyways, at the first irreversible change downstream from the query, but if you want to be safe, you can also do it explicitly.

An alternative way to get the first row in every category, without `groupby('GROUP').agg({'VAL':'first'})`:
* `df.sort_values(['GROUP', 'VAL']).drop_duplicates(['GROUP'])` (to kill duplicates of one column only). Not sure yet which method is faster (🔥?)

Conditional indexing supports functions, as long as they take and return Pandas series, or something compatible, like a Numpy array). For example, conditional forms (both local indexing and queries) support elementwise Boolean operators, like `&` and `|`.

☢️ There's a strange pitfall associated with conditional data retrieval that I don't understand for now. `query` seems to always create a view of a dataframe, which may look like a new dataframe with fewer rows, but that actualy acts as a copy, and not as a deepcopy. As a result, if you save the results in a "new" dataframe `df2 = df.query('x>0')`, and then change values in  `df2` in any way, it may cause a warning "A value is trying to be set on a copy of a slice from a DataFrame". In this case df2 gets updated, and df seems to remain intact (unchanged), but there's this warning, and I'm not sure why. (Is it because they had to replace a view with a copy when you ran a command? So they want us to be more deliberate here?). Replacing a simple `query` with `df.query(...).copy()` turns a view into a legit new dataframe, and thus eliminates a warning.

## Chained Assignment problem

"Chained assignment" is a name for a problem that you get when writing to a section of a dataframe, when selecting by both column and row. In essence, if you manage to selecte both of them at once, you're good. But if you do first one, then another, then you get a view of a view, and you can read, but not write. The situationis complicated by the fact that in some cases even chained assignment may actually work (with a warning), but for a different dataframe the same code may crash.

* `df.loc[1,'x']` - Read and write. Reference both by column and row by index.
* `df.iloc[1,0]` - Read and write. Reference both by position. Row number goes first. 
* `df['x'][1]` - Only read.
* `df.x[1]`- Only read. (completely equivalent to the one above, just different syntax)
* `df.x.iloc[1]` - Officially Unpredictable (according to the manuals), so should be assumed to be "Only read"
* `df.iloc[1].x` - Officially Unpredictable, exactly like the one above.

Footnotes:
* https://stackoverflow.com/questions/29888341/a-value-is-trying-to-be-set-on-a-copy-of-a-slice-from-a-dataframe-warning-even-a

# Exploratory analysis

Useful methods:
* `df.col.hist()` - draws a nice histogram
* `df.col.value_counts()` - outputs value-counts for a categorical variable.
* `df.describe(include='all')` - outputs a derivative dataframe with min-max-mean-std-count etc. summary. Without this `include='all'` clause doesn't include categorical variables, but only numerical ones.

# Specific Data types

## Numerical
* Cast to type: `.astype(int)`
* Vectorized not: `~` operator. For example, `~np.array([True,False])` is "F, T".
* Replace NaNs: `.fillna(0)` replaces all NaNs with zeroes. Can be run on a dataframe, or a series.
* Replace something else: use a numpy function: `.replace(old, new)`.
* Basic math with a series returns a series, so one can do `(df.a + 10).values` etc.
* Rank: `df.groupby('col1')['col2'].rank(method='dense', ascending=False)` produces a rank-transformed series. Lots of cool methods of ranking: `average` (default), but also `min`, `max`, `dense`, `first`. Se [manual](https://pandas.pydata.org/docs/reference/api/pandas.Series.rank.html) for more.

## Strings
* Operations on existing strings:
    * cut first chars: `df['x'].str[1:]`
    * Methods on strings are collable: `df['x'].str.lower()`, but they only work on a series, not on a dataframe.
        * For example: to remove leading spaces: `strip()`
        * `.split()` splits every string into an array of substrings, same as for normal Python.
* Creating new string columns:
    * The simplest (but inflexible) way to cast to a string: `df.x.astype(str)`
    * f-strings written in  `f"bla"` way won't work in this type of expression, but they can be used via a `str.format()` method, mapped to a series: `df.x.map('{:02d}'.format)`
    * To combine strings, use a normal `+` notation, e.g.: `d['a'] = d['b'] + d['c']`. 

There's support of **regular expressions** (see [[regex]]) in `str`, such as:
* `.str.extract(r'(expr))` - reformats the series into a new dataframe.
    * The string parameter should contain at least one pair of brackets, or it won't work (in this case the brackets are called "capture groups"). 
    * Returns a dataframe with a column for each capture group (NaN if there's no match).
    * To find substrings : `.str.extract('(substring)').notna()`
    * A simpler alternative to the previous one: `.str.find("substring")>-1`
* `.str.findall(r'expr')` - returns a series of lists; empty if no match, a list of matches if any.
* `extractall` ? 🔥

To get rid of umlauts and other special letters in a simple (but potentially dirty) way:
`.str.normalize('NFKD').str.encode('ascii', errors='ignore').str.decode('utf-8')`
To make it a bit more flexible, run `.replace(character_dict, regex=True)` with a custom dictionary first; then normalize the rest.

Footnotes:
* https://stackoverflow.com/questions/61822799/using-pandas-extract-regex-with-multiple-groups
* https://stackoverflow.com/questions/766372/python-non-greedy-regexes
* https://pandas.pydata.org/docs/reference/api/pandas.Series.str.extract.html
* https://stackoverflow.com/questions/49891778/conversion-utf-to-ascii-in-python-with-pandas-dataframe

## Dates and times

Documentation (a bit wordy): https://pandas.pydata.org/pandas-docs/stable/user_guide/timeseries.html

The main data type for dates is a **Timestamp**, and it directly inherits to the `datetime.datetime` types (described in [[py_dates]]). Pandas however provides a set of much neater wrappers these methods. It means that if you create a pandas-timestamp and a datatime-datetime for the same moment, they will be equal to each other, and `isinstance(dt, datetime.datetime)` will be true for both of them, even though `type()` will return different names, and formally types won't be equal to each other. A standard case of class inheritance.

The most useful methods are called on series of Timestamps using a prefix (similar to how it works for strings): for example `df.x.dt.dayofyear`. For example:
* `year`, `hour`, `day`, and other obvious words, that turn a a datastamp into an integer. Also: `dayofyear`, `weekofyear`, `dayofweek` (zero-indexed starting Monday). Note the absence of parentheses; it's really like that; it's a property, but not a method, apparently! (🔥 why?)
* `date()` and `time()` return a new series of either **Dates** or **Times** that are not quite Timestamps, but kinda related. ☣️ Note that a Date is not just a rounded Datetime, as it does not inherit to a Timestamp, and is not equal to a datastamp encoding 00:00 time on the same date. This may cause problems if one isn't careful.
* `dt.round('D')` - rounding to a day; similarly `floor` is for rounding down, and `ceil` for rounding up.
    * Aliases for frequencies can be found here: https://pandas.pydata.org/docs/user_guide/timeseries.html#timeseries-offset-aliases
    * This approach won't work for months, and will return an error, as months have different lengths. See below (offsets) for a proper solution.
* To produce a nice string: `strftime('%w-%a')`; the format codes are described here: https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior
    * Most common string: `%Y-%m-$d` - ISO date. Alghouth `str(timestamp.date())` gives the same result.
* To parse strings into stamps (an opposite to formatted output): `df.x = pd.to_datetime(df.x)` - it's surprisingly smart, and in most cases just magically works. But if it cannot get the format, just provide a `format` parameter, designed of same `%Y` etc elements (for example, %d/%m/%Y`)
* To test if a date is special:`is_month_end`

**Timedelta** is a separate class. It can be produced by subtraction of two stamps, and it can be added to a stamp. It also supports its own set of methods, such as `dt.days` (to express this difference as a number of days)	. Note the plural, and the absence of parentheses.
* `(df.Date - df.Date.min()).dt.days` - Number of days from the smallest date
* `pd.Timedelta('15d')` - Manually generate a timedelta

**DateOffset** is another curious class that I don't quite understand, but it is necessary in these cases:
* To round to the first day of every month:
    * `df.DATE.dt.floor('d') + pd.offsets.MonthEnd(n=0) - pd.offsets.MonthBegin(n=1)`. This is more complicated than just rounding to a month, as months are not a real unit (they have variable size), which somehow makes this fancy context-sensitive construction necessary.
    * For one date one can do `my_date.replace(day=1)`, but it is not serialized (can only be serialized with `apply` or list comprehension)
    * There's also a fast hacky solution : `df.DATE.dt.to_period('M').dt.to_timestamp()`

To generate a **range of date-times**: `pd.date_range(start, end, period, freq)` (for more parameters, see [manual](https://pandas.pydata.org/docs/reference/api/pandas.date_range.html)). 
* Despite the name, generates proper full-blooded timestamps, and not just scrawny dates. Note also that `freq` is not the 3d default argument, so better to provide it as a named argument.
    * Default freq is 'd'
* For the first day of each month, use offset: `pd.date_range(start, end, freq=pd.offsets.MonthBegin(1))`
* If the goal is to upsample data, instead of creating a date_range, and then manually doing `.merge_asof`, try using:
    * `df.asfreq(freq, method='ffill')` (for frontfill, or `bfill` for backfill)
    * `df.resample()` - ? 🔥

A note on deprecation for datetime functions (first announced some time in 2022): apparently, the only part that will be deprecated is the `pd.datetime` class - one needs to really import `datetime.datetime` (see [[py_dates]]). All other wrappers, like `pd.to_datetime()` for example are supposed to remain intact. (Refs: [1](https://gitlab.tudelft.nl/rhenning/ANTS-model/-/issues/28), [2](https://stackoverflow.com/questions/60856866/why-was-datetime-removed-from-pandas-1-0#:~:text=%22FutureWarning%3A%20The%20pandas.,Import%20from%20datetime%20module%20instead.%22))

## Categorical

In some 🔥
* `Series.cat.codes` - 🔥 

For some practical examples, see [[LightGBM]], where defining categorical input vectors is a critical step of data preparation (the only way to explain to the model, which dummy variables are catecoricall, and which are not).

Footnotes:
* https://stackoverflow.com/questions/51102205/how-to-know-the-labels-assigned-by-astypecategory-cat-codes

## Json-like

* `pd.json_normalize(df['col_name'])` - takes a single column of [[json]]-like dicts; produces a dataframe with all possible columns (with `None` for missing values). The names of these new columns are point-joined property names (like in `user.name.suffix`)

# Data transformations

**Applying  arbitrary functions to every element of a column**
Several options here:
* **map**: `df.x = df.x.map(@function)`. For one-liners, works well with lambda notation. 
* `df.apply(@fun, axis=1)`- then write a `fun` function to be applied to every row of a dataframe, taken as a series (see chaining advice below).

**Creating a new column from old columns:**
There are least 6 different ways to do it, listed here from (arguably) best to worst:
* `df['z']=df.x+df.y` - that's what most people use, and it's one of the fastest approaches, but it has 2 draw-backs. One, it cannot be chained (using `.` notation). And two, when used on a view, it throws a "slice assignment" warning. So after things like `.agg` it will work just fine, but after a `.query` it will cause a problem (as `.query` creates a view!).
* `df = df.assign(z = df.x + df.y)` - this one is a bit slower, but can be chained! Many people prefer it, and it seems to be considered archetypical. In its simplest form (the one shown here) it can however only reference columns that existed in the original dataframe
* `df = df.assign(z = lambda d: d['x'] + d['y'])` - similar to the previous one, but as now it references an abstract dataframe, and not the original dataframe, this one can be fully chained, as it can reference columns created at the previous step.
* `df.insert(2, 'z', df.x+df.y)` - the fastest of all, but cannot be chained (it's in-place!), which also means that it doesn't feel Pandas-y. Also, you cannot run it 2 times in a row (returns an error, saying that this column already exists), and you need to know where to add this new column.
* `df.loc[:,'z']=...` - prevents slicing assignment warning, but is the slowest of all by far. May be good for updates though. Still not chainable though.
* `df = df.eval('z=x+y')` - chainable, but also slower. In principle, `eval` can also be run in-place, but then of course it becomse non-chainable, while still being slow.

In practice, in terms of both speed and readability (and maybe a bit of a balance between the two), I decided to always use chaining, and thus relying on `eval` as much as possible (because of serialization it is actually reasonably fast). But as eval doesn't support some operations (it can't work with strings or with `.shift`, for example), I'd switch to `assign + lambda` for tougher cases. Assign _may_ also bring some performance boost, but this still needs to be investigated.

```python
df = (df
      .eval("z = x + y")
      .assign(w = lambda d: d['z'].astype(str) + ' hehe')
      .assign(v = lambda d: d['w'].shift(-1))
     )
```

Note that even `assign-lambda` does not support f-strings, so the best way to cast a non-string to a string seems to be use `.astype(str)`. (Using an f-string inside `eval` causes an error; inside `lambda` it doesn't cause an error, but is not serialized, and transform the entire series into one giant string.) Also, running `str(d.z)` does not cause an error, but doesn't do what we want, as it retains the type explanation at the end (debugging-style). Short of using a list comprehension, at the moment, I'm not sure how to plug f-string functionalify to pandas directly.

**Updating certain values in a column**
The most popular way seems to be using a `where` command, which works like a `query` with a default value. Because of that, when it's used to update values, it actually updates values for which the condition is _false_, not true. For example: `df['x'] = df['x'].where(df['x'] > 0, -2)` will replace all values that are below zero with −2. One can use `~` to flip the condition, and it seems that when updating stuff some people even prefer writing it with a tilde, to make it more clear which values are updated, but it is such a mindhack that I reject it with indignation.

The same `where` method can be used for a vectorized **ternary operation** on columns. If you want to get a column that takes values from the original `COL_ORIGINAL` column when a condition `CONDITION` is true, and values from `COL_ALTERNATIVE` column otherwise, do this:
`df.COL_ORIGINAL.where(df.CONDITION, df.COL_ALTERNATIVE)`. 
And as this expression produces a series, it can be used inside a chained `assign` clause:
`.assign(COL = lambda d: d.COL.where(<condition>, <alternative_values>)`.

**Applying a function to every row**
This is possible with running `apply` over rows; for example, this line normalizes all values for every row:
`.apply(lambda d: d/d.sum(), axis=1)`
The operation is however extremely slow (an explicit loop, not vectorized), so for every practical purpose it's better to stop chaining and perform a numpy operation on values, then chain further. In this example, doing this:
`d.loc[:,:] = d.values / d.sum(axis=1).values[:, np.newaxis]`
Alternatively, one can put this function into a function, and `pipe()` this function.

**Refs:**
* https://www.tutorialspoint.com/python_pandas/python_pandas_working_with_text_data.htm
* https://pandas.pydata.org/pandas-docs/stable/user_guide/text.html
* https://jakevdp.github.io/PythonDataScienceHandbook/03.12-performance-eval-and-query.html
* https://datatofish.com/integers-to-strings-dataframe/

# Merging

**Concatenation**: `df.concat(objs)` where objs is a list of dataframes. Parameters:
* `axis`: default 0 (horizontal stacking), but may be something else
* `join`: default value of `outer` (for union of matching indices), but can be changed to `inner` (for intersection). Makes sense for `axis=1` (when columns are merged): with `outer` missing values are padded with NaNs, while with `inner` incomplete rows are eliminated.
* `keys`: generated hierarchical keys. Expects a list of names, to become the first level of these keys.

Appending one row: `df.loc[len(df), :] = [a, b, c]`

**Merging** (full-featured, in-memory join). Archetypeical use:
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
* If `on` is left empty (None), merges on all columns that are found in both dataframes (unless joining on indexes is invoked)
* If keys are named differently in left and right dataframes, use `left_on='keyleft', right_on='keyright'`. If merging on more than one column, use lists for both (same as for `on`).
* If some columns are duplicated, the result inherits both, but modifies names. This can be changed by providing `suffixes` argument (a list). By default, adds `_x` and `_y` for left and right respectively. If you want one set of columns to retain old names, pass `None` as one of the elements of this list.
* If some rows are not unique, behavior can be modified with `validate` parameter.
* A diagnostic column may be created with `indicator=True` parameter. This creates a new column `_merge` with values of `left_only`, `right_only`, or `both`.  This feature may be quite helpful when used in conjunction with `.query` and `.drop` to create exclusive joins; e.g. do a join, then only leave records from the left that weren't found in right).
* Instead of merging on an arbitrary column, we can also merge on index, which is controlled with `left_index=True` argument (and similar for the right). _Is it faster?_
* To join only some columns, join with an incomplete dataframe (make df_left not a full dataframe, but select the columns you need using standard `df[['x','y']]` notation).
* To remove duplicates, do `.drop_duplicates()`

An interesting trick to create a "Kronecker-multiplied" backbone of several factors, to get a row for every combination of "indices" (which is helpful for transforming a sparse representation into a dense one): merge on a dummy column.
```python
a = [1,2,3]
b = ['a','b']
pd.DataFrame({'A':a, '_':1}).merge(pd.DataFrame({'B':b, '_':1})).drop('_', axis=1)
```

For **merging multiple dataframes** at once, set indices properly, and then do `pd.concat()` with a list of dataframes, and `join=inner` (or something else) argument. See documentation for details. This looks neater, and may be faster than consecutive merges.

**merge_asof** - sort of a sequential merge with short-circuiting, similar to vlookups in Excel that find the latest so-so match, instead of searching for a perfect match. For example, if `df_checkpoints` contains only some measures at some time points, and `df_full` contains a full list of time-staps, do this to "infill" (protract step-wise) every point measure across all time-stamps between point-measures:
`pd.merge_asof(df_full, df_checkpoints, on='TIMESTAMP')`
In this case to me it feels that we're going foward, filling data with stuff, but actually the default settings for the direction is named `direction=backward`, so I'm guessing it's about "looking backwards" at the last reasonable value. If you switch `direction` to `forward`, you'll be filling missing points with the next value. It's also possible to use `direction=nearest`.
* Note that without piping, this method is not stackable (it's not a method of a dataframe, but of Pandas). But one can always do `df1.pipe(pd.merge_asof, df2, on='TIME')`.
- [ ] * Both dataframes need to be sorted (on the key that we're using for merging).
* https://pandas.pydata.org/pandas-docs/version/0.25.0/reference/api/pandas.merge_asof.html

🚫 **Recently deprecated: Append**
* `df = df.append(df2)` - used to be a way to add homogeneous rows. Takes either a one-row dataframe, or a dictionary. 
* If indices are meaningful, use this notation, and it will check that they don't duplicate. If indices are essentially just row numbers, add `ignore_index=True` to make it more relaxed. And maybe also `sort=False`, to keep things simple and fast. Note that `ignore_index=True` seems to be necessary if we're supplying new rows as a dictionary (because by definition we don't have an index in this case?)
* Remember that while `append()` sounds in-place, it's actually NOT an in-place method, so we need to do `df = df.append(blabla)`.

Footnotes:
* Documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html
* https://stackoverflow.com/questions/53645882/pandas-merging-101

# Grouping and Aggregation

First group by a column, then apply aggregation. `dfs = df.groupby('g').agg({'a': ['min', np.mean]})`

**Grouping** columns (those that appear in the summary dataframe not because they were summarized, but because they were grouped by) also become a multiindex. It means that they aren't normal columns, and cannot be referenced directly. To turn into a "normal" data frame, do `dfs = dfs.reset_index()`.

**Aggregation** is coded as a dictionary, and more than one function can be applied to every column. Functions are passed as either function objects, or function names (strings, like `'count'`). 
* You can pass either one function, or a list of them. If a column from the original dataframe is summarized in several different ways, the output summary dataframe gets a _multiindex_. Accessing the index of this summary dataframe gives you tuples.
* Some useful aggregation functions: sum, mean, min, max, count, var, std, sem, first, last, nth (?).
* To calculate a **mode** of some variable, use this trick: `mode = lambda x: x.value_counts().index[0]`, then pass `mode` (the function, not a string) as an argument to `agg`.
* To do a `cumsum` or `rank` that re-sets for every new group of rows, `groupby` by this grouping column, then use `cumsum` (or `rank`) as an aggregating function. 
    * This usage feels a bit weird, compared to other usages, as it doesn't change the size of the dataframe, so it cannot be easily combined with "normal" summary functions, such as `sum` or `count` (it doesn't produce an error, and those "normal" functions are still applied, but the results are placed somewhere seemingly arbitrary in the middle of the dataset, and it doesn't seem like a stable or transparent behavior)
    * Also it doesn't preserve indices, so it is not supposed to be merged back; it is supposed bo be put inside an `assign` clause, to be concatenated from the right based on dimensions, not indices.

**Sorting** rows is done with `.sort_values(by='str')`, or a list of strings (column names). `ascending=True` by default. If using a list of keys, `ascending` can also be a list.

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