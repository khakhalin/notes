# Matplotlib

Parents: [[01_Tools]] / [[python]]
See also: [[numpy]], [[pandas]]

#tools #python


# A library of tricks

* log plot: `plt.xscale('log')`
* Area plot: `plt.fill_between(x, ylo, yhi, alpha=0.2)`
* To quickly visualize NaNs in a numpy array: `plt.imshow(np.isnan(x).T, aspect='auto')`
    * For bw colors, add: `cmap='gray'`. to distinguish "all 1s" from "all 0s", add `vmin=0, vmax=1`
    * To change x ticks, add this (as another param): `extent=[xmin, xmax, 0, x.shape[1]]`

# Colors and colormaps

Some nice colors (tableau-like) can be reached as named colors of form `'tab:blue'` etc. Not as aggressive as defaults.

A really cool tutorial on creating custom colormaps and colorbars: https://matplotlib.org/stable/tutorials/colors/colorbar_only.html
Among other things, here's how you apparently create a colormap from individual colors:
```python
cmap = mpl.colors.ListedColormap(['red', 'green', 'blue', 'cyan']
```

# Refs

* Brief intro from Brad Solomon: https://realpython.com/python-matplotlib-guide/
* Cheatsheet: [https://github.com/rougier/matplotlib-cheatsheet](<https://github.com/rougier/matplotlib-cheatsheet>)
* Notes from Brandon Rohrer: https://e2eml.school/blog.html#133