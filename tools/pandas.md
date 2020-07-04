# Pandas

#tools

* Creation: `DataFrame` (curious capitalization)
* `[[something]]` simply means "a list of lists". It's a type thing, not a separate synatx.
* Select columns by label: `d['x']`. Alternative spelling: `d.x`. Returns a series. 
* Select rows by label: `d.loc[1]`. Works for both df (returns a row-series), and for column-series (returns a single value).
* To cast a single value from a row-series into just a value, use `.values[0]` at the end.
* Iterating through rows: either `for i in range(d.shape[0]): d.loc[i]`, or `for key,val in d.iterrows()`  (in both cases we get row-series). To slice into slivery dataframes, one could use `d.loc[[i]]`, but usefulness of that is unclear.
* **Chained Assignment**: a problem while writing to a frame, selecting by both column and row.
    * Good: reference both by label (index): `d.loc[1,'x']`
    * Also Good: reference both by position:`d.iloc[1,0]`. Row goes first. 
    * Read, but not write: `d.x[1]`, which is equivalent to `d['x'][1]`.
    * Read but not write: Both `d.x.iloc[1]` and `d.iloc[1].x`. Documentation states that whether any given slice would work or not is "officially unpredictable", so chaining shoud never be used.
* Out-of-range integers are forgiven (ignored) on reading, but cause an error if you try to write
* For **conditional data retrieval**: 
    * either **logical indexing** `d.loc[d.x>0]` (can be combined with list comprehensions, and can be written to) 
    * or **queries**: `d.query('x>0')` (easier for a human to read; reads slightly faster, but cannot be written to).
* Conditional indexing supports functions, as long as they take and return Pandas series, or something compatible, like a Numpy array).