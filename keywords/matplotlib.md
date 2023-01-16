# Matplotlib

Parents: [[01_Tools]] / [[python]]
See also: [[numpy]], [[pandas]]

#tools #python


# Misc tricks

* log scale axis: `plt.xscale('log')`
* Area plot: 
    * Manual: `plt.fill_between(x, ylo, yhi, alpha=0.2)` for each band
    * Entire [[pandas]] dataframe in one go: `plt.stackplot(df.DATE, df.iloc[:, 1:].T)`. Note this `.T`; it doesn't work without it.
* To quickly visualize NaNs in a numpy array: `plt.imshow(np.isnan(x).T, aspect='auto')`
    * For bw colors, add: `cmap='gray'`. to distinguish "all 1s" from "all 0s", add `vmin=0, vmax=1`
    * To change x ticks, add this (as another param): `extent=[xmin, xmax, 0, x.shape[1]]`

# Colors and colormaps

Some nice Tableau-like colors can be reached as named colors (strings) of form `tab:blue` etc. Not as aggressive as defaults.

A cool tutorial on creating custom colormaps and colorbars: https://matplotlib.org/stable/tutorials/colors/colorbar_only.html

* Creating a colormap from listed colors:  `cmap = mpl.colors.ListedColormap(['red', 'green', 'blue', 'cyan']`
* Getting color values from a listed colormap (each color value is a 4-element vector of RGB+alpha. RGB values are within 0-1; Alpha is always 1.0)
    * For a qualitative colormap: `[plt.cm.tab10(i) for i in range(n)]`
    * For a continuous colormap:  `[plt.cm.viridis(i/n) for i in range(n)]`

# Refs

* Brief intro from Brad Solomon: https://realpython.com/python-matplotlib-guide/
* Cheatsheet: [https://github.com/rougier/matplotlib-cheatsheet](<https://github.com/rougier/matplotlib-cheatsheet>)
* Notes from Brandon Rohrer: https://e2eml.school/blog.html#133