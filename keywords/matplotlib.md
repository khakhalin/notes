# Matplotlib

Parents: [[01_Tools]] / [[python]]
See also: [[numpy]], [[pandas]]

#tools #python


# Misc tricks

* log scale axis: `plt.xscale('log')`
* To make subplots look pretty, always run `plt.tight_layout()` in the end

Area plot: 
  * Manual: `plt.fill_between(x, ylo, yhi, alpha=0.2)` for each band
  * Entire [[pandas]] dataframe in one go: `plt.stackplot(df.DATE, df.iloc[:, 1:].T)`. Note this `.T`; it doesn't work without it.

To quickly visualize NaNs in a numpy array: `plt.imshow(np.isnan(x).T, aspect='auto')`
  * For bw colors, add: `cmap='gray'`. to distinguish "all 1s" from "all 0s", add `vmin=0, vmax=1`
  * To change x ticks, add this (as another param): `extent=[xmin, xmax, 0, x.shape[1]]`

# Colors and colormaps

Some nice Tableau-like colors can be reached as named colors (strings) of form `tab:blue` etc. Not as aggressive as defaults. The other way to get some nice names is to use xkcd prefix, e.g. `'xkcd:sky blue'`.

Getting color values from a listed colormap (each color value is a 4-element vector of RGB+alpha. RGB values are within 0-1; Alpha is always 1.0)
* For a qualitative colormap: `cmap=plt.colormaps.viridis`, then call them like a function `cmap(i)`, or prepopulate a list: `[cmap(i) for i in range(n)]`
* For a continuous colormap:  `cmap=plt.colormaps.tab10`, then `[cmap(i/n) for i in range(n)]`
* To get the map , one can also do `cmap = matplotlib.colormaps['plasma_r']`

Creating discrete colormaps:
* From listed colors:  `cmap = mpl.colors.ListedColormap(['red', 'green', 'blue', 'cyan']`
* From color descriptions: `matplotlib.colors.ListedColormap(["#ffffcc", "#a1dab4", "#41b6c4"], name="custom_map")`
* After that, a colormap needs to be registered using `colormaps.register` (see below):

Creating continuous colormaps (the example below creates something like a hot lava, from white to black). The way each array works is that the first value is the x-position (has to be between 0 and 1), and the other two values are RGB values at this point. If everything is continuous, then they should match; if you need a cliff-like drastic change, make them different. It seems that value smay go outside of the `0..1` range - probably normalized?

```python
cdict = {'red':   [[0.0,  1.0, 1.0],
                   [0.5,  1.0, 1.0],
                   [0.7,  0.8, 0.8],
                   [1.0,  0.0, 0.0]],
         'green': [[0.0,  1.0, 1.0],                   
                   [0.1,  1.0, 1.0],
                   [0.7,  0.0, 0.0],
                   [1.0,  0.0, 0.0]],
         'blue':  [[0.0,  1.0, 1.0],         
                   [0.3,  0.0, 0.0],                   
                   [1.0,  0.0, 0.0]]}
cmap = matplotlib.colors.LinearSegmentedColormap('custom_map', segmentdata=cdict, N=256)
matplotlib.colormaps.register(cmap=cmap, force=True)
cmap
```

To set extreme colors: 
* `cmap.set_over("black")`
* and similarly `.set_under()`
* To set a color for NaN: `cmap.set_bad('red')`

Colorbar bells and whistles: `plt.colorbar(shrink=0.6, extend='max')`. Here `shrink` allows to scale it; `extend` (very optional) adds a color marker to show the color for those points that fall off the scale. I don't find it too intuitive though, unless you know exactly what this extra triangle means.  It's also possible to `extend='min'` and `both`.

Footnotes:
* https://matplotlib.org/stable/tutorials/colors/colorbar_only.html
* https://matplotlib.org/stable/tutorials/colors/colormap-manipulation.html
* https://matplotlib.org/stable/api/cm_api.html
* https://matplotlib.org/stable/tutorials/colors/colormaps.html

# Dates

A pretty minimalistic formatter for dates that only shows years when they change (basically, the smallest amount of labeling that still unambiguously labels the dates):

```python
import matplotlib.dates
locator = matplotlib.dates.AutoDateLocator(minticks=2, maxticks=100)
formatter = matplotlib.dates.ConciseDateFormatter(locator)
ax = plt.gca()
ax.xaxis.set_major_locator(locator)
ax.xaxis.set_major_formatter(formatter)
```

# General Refs

* Brief intro from Brad Solomon: https://realpython.com/python-matplotlib-guide/
* Cheatsheet: [https://github.com/rougier/matplotlib-cheatsheet](<https://github.com/rougier/matplotlib-cheatsheet>)
* Notes from Brandon Rohrer: https://e2eml.school/blog.html#133