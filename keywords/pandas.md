# Pandas

#tools

Parent: [[python]]
See also: [[numpy]]

## Basics and IO

* Creation: `DataFrame` (curious capitalization)
* `[[something]]` simply means "a list of lists". It's a type thing, not a separate synax.
* To look into it: `df.head()`
* Read: `pd.read_csv('name.txt', sep='\t')`
* Write: `df.to_csv('name.csv', index=False)`

## Addressing

**Columns**
* To get column names, `df.columns`.
* Rename all columns, just overwrite it with a different vector.
* Rename  only some columns: `df = df.rename({'a': 'X', 'b': 'Y'}, axis=1)`
* Delete some columns: `df = df.drop(['a','b'], axis=1)`

**Indexing**
* Select columns by label: `df['x']`. Returns a series. To cast into a numpy object, add `.values` at the end (right like that, without parentheses). An alternative spelling `df.x` works in most cases, except if the name of the column is special (⚠️Gotcha: for example, `first` and `last` are reserved words, so if you have a column named "first", then `d['first']` would work well, but `d.first` won't)
* Select rows by label: `df.loc[1]`. Works for both df (returns a row-series), and for column-series (returns a single value).
* To cast a single value from a row-series into just a value, use `.values[0]` at the end.
* Out-of-range integers are forgiven (ignored) on reading, but cause an error if you try to write
* For **conditional data retrieval**: 
    * either **logical indexing** `df.loc[d.x>0]` (can be combined with list comprehensions, and can be written to) 
    * or **queries**: `df.query('x>0')` (easier for a human to read; reads slightly faster, but cannot be written to).
* Conditional indexing supports functions, as long as they take and return Pandas series, or something compatible, like a Numpy array).
* **Iterating through rows**: either `for i in range(df.shape[0]): df.loc[i]`, or `for key,val in df.iterrows()`  (in both cases we get row-series). To slice into slivery dataframes, one could use `df.loc[[i]]`, but usefulness of that is unclear.

**Chained Assignment**
A problem while writing to a frame, selecting by both column and row.
* Good: reference both by label (index): `df.loc[1,'x']`
* Also Good: reference both by position:`df.iloc[1,0]`. Row goes first. 
* Read, but not write: `df.x[1]`, which is equivalent to `df['x'][1]`.
* Read but not write: Both `df.x.iloc[1]` and `df.iloc[1].x`. Documentation states that whether any given slice would work or not is "officially unpredictable", so chaining shoud never be used.

## Data transformations

**Strings**
* For basic string operations, like cut first chars: `df['x'].str[1:]`
* Other operations, all follow a paradigm of `df['x'].str.lower()`. They only work on a series, not on a dataframe. It also means that `df.x` won't work in this case, as it doesn't return a series. 
* To create a new column by combining other columns, use a normal `+` notation for strings, like `d['a'] = d['b'] + d['c']`. f-strings don't seem to work here though.
* `split` splits every string into an array of substrings, same as for normal Python.
* There's a support of regular expressions (see [[regex]]) in `str`, such as `extract` (extracting part of a string that matches the pattern), `findall` (only leaving entries that match the pattern).

**Numbers**
* Cast to type: `.astype(int)`

Refs:
* https://www.tutorialspoint.com/python_pandas/python_pandas_working_with_text_data.htm
* https://pandas.pydata.org/pandas-docs/stable/user_guide/text.html

## Joining and merging

* Concatenation: `df.concat(objs)` where objs is a list of dataframes. Parameters:
    * `axis`: default 0 (horizontal stacking), but may be something else
    * `join`: default value of `outer` (for union of matching indices), but can be changed to `inner` (for intersection). Makes sense for `axis=1` (when columns are merged): with `outer` missing values are padded with NaNs, while with `inner` incomplete rows are eliminated.
    * `keys`: generated hierarchical keys. Expects a list of names, to become the first level of these keys.
* A wrapper for adding homogeneous rows: `f = f.append(f2)`
    * If indices are meaningful, use this notation, and it will check that they don't duplicate. If indices are essentially just row numbers, add `ignore_index=True` to make it more relaxed. And maybe also `sort=False`, to keep things simple and fast.
* Full-featured in-memory join: `pd.merge`. See the description here: https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html

## Grouping and Aggregation

First group, then apply aggregation. `dfs = df.groupby('g').agg({'a': [min, np.mean]})`

Aggregation is coded as a dictionary, and more than one function can be applied to every column. Functions are passed as an array of functions (!!!). If a column in the original dataframe is summarized in more than one way (say, if you calculate Column names in summary dataframe become a **multiindex** (when column index is a tuple).

Grouping columns (those that appear in the summary dataframe not because they were summarized, but because they were grouped by) also become a multiindex. It means that they aren't normal columns, and cannot be referenced directly. To turn into a "normal" data frame, do `dfs = dfs.reset_index()`.

## Misc

To print a really long data frame, do this:
```python
with pd.option_context('display.max_rows', 1400):
    print(dfs)
```