# Matplotlib

Parents: [[01_Tools]] / [[python]]
See also: [[numpy]], [[pandas]]

#tools #python


# Tips and tricks

**Hiding stuff**
* Remove top and right border, ggplot-style: `plt.gca().spines[['right', 'top']].set_visible(False)`
* Remove tick labels: `ax.set_yticklabels([])`
* Remove ticks: `plt.xticks([])`
* Remove entire axis instead: `plt.gca().get_xaxis().set_visible(False)`

**Area plot** 
  * Manual: `plt.fill_between(x, ylo, yhi, alpha=0.2)` for each band
  * Entire [[pandas]] dataframe in one go: `plt.stackplot(df.DATE, df.iloc[:, 1:].T)`. Note this `.T`; it doesn't work without it.

**Heatmaps**
* To quickly visualize NaNs in a numpy array as an image: `plt.imshow(np.isnan(x).T, aspect='auto')`
  * For bw colors, add: `cmap='gray'`. to distinguish "all 1s" from "all 0s", add `vmin=0, vmax=1`
  * To change x limits, add this (as another param): `extent=[xmin, xmax, 0, x.shape[1]]`
  * To change ticks: `plt.xticks(tick_positions, tick_labels)`

**Legend** - how to add it on the side (sadly, no single standard keyword for that):
`plt.legend(labels=labels, loc='upper left', bbox_to_anchor=(1.04, 1.015), frameon=False)`
The last one is to kill the border, which makes exact placement a bit easier. Unfortunately the two magic numbers have to more or less be used like that. Not sure also, how stable this solution is to main image rescaling.
* Even after using a Pandas `df.plot()` one can edit some parts of the legend by running `plt.gca().legend(title='Title', framealpha=1)` etc.

Placing text on a plot: unfortunately there seems to be no serialized solution (in the core library), so we need to do this in a loop: `plt.annotate(text_string, (x, y))`, where x and y are single values.

**Misc**
* log scale axis: `plt.xscale('log')`
* To make subplots look pretty, always run `plt.tight_layout()` in the very end
* A cheap fast way to make a `.-` plot style look nice is to make `linewidth` ridiculously small (say, 0.3)
* `plt.grid(alpha=0.1)` tends to look nice (barely visible)
* Letters as markers: use some [[latex]] `markers = ["$\mathbf{"+s[0]+"}$" for s in view.columns[1:]]`, then plot in a loop, doing `marker=markers[i]` for every series.
* To save figure without cutting the legend off: `plt.savefig("filename.png", bbox_inches="tight")`

Footnotes:
* https://stackoverflow.com/questions/4700614/how-to-put-the-legend-outside-the-plot

# Colors and colormaps

Some nice Tableau-like colors can be reached as named colors (strings) of form `tab:blue` etc. Not as aggressive as defaults. The other way to get some nice names is to use xkcd prefix, e.g. `'xkcd:sky blue'`.

Getting color values from a listed colormap (each color value is a 4-element vector of RGB+alpha. RGB values are within 0-1; Alpha is always 1.0)
* For a continious colormap: `cmap=plt.colormaps.viridis`, then call them like a function `cmap(i)`, or prepopulate a list: `[cmap(i) for i in range(n)]`. If `cmap()` arguments are integer, the limits (to cover the full colormap) seem to be from 0 to 256, but if you make them float, the limits are 0 to 1, which is probably way more confrotable.
* For a categorical colormap:  `cmap=plt.colormaps.tab10`, then `[cmap(i/n) for i in range(n)]`
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

**Colorbar** bells and whistles: `plt.colorbar(shrink=0.6, extend='max')`. Here `shrink` allows to scale it; `extend` (very optional) adds a color marker to show the color for those points that fall off the scale. I don't find it too intuitive though, unless you know exactly what this extra triangle means.  It's also possible to `extend='min'` and `both`.

Footnotes:
* https://matplotlib.org/stable/tutorials/colors/colorbar_only.html
* https://matplotlib.org/stable/tutorials/colors/colormap-manipulation.html
* https://matplotlib.org/stable/api/cm_api.html
* https://matplotlib.org/stable/tutorials/colors/colormaps.html

# Dates

A pretty, minimalistic formatter for dates that only shows years when they change (basically, the smallest amount of labeling that still unambiguously labels the dates):

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

A free book on Visualizing stuff in Matplotlib: https://github.com/rougier/scientific-visualization-book