# Matplotlib

Parents: [[01_Tools]] / [[python]]
See also: [[numpy]], [[pandas]]

#tools


# A library of tricks

* log plot: `plt.xscale('log')`
* Area plot: `plt.fill_between(x, ylo, yhi, alpha=0.2)`
* To quickly visualize NaNs in a numpy array: `plt.imshow(np.isnan(x).T, aspect='auto')`
    * For bw colors, add: `cmap='gray'`. to distinguish "all 1s" from "all 0s", add `vmin=0, vmax=1`
    * To change x ticks, add this (as another param): `extent=[xmin, xmax, 0, x.shape[1]]`

**Colors**
Some nice colors (tableau-like) can be reached as `'tab:blue'` etc. Not as aggressive as defaults.

# Refs

* Brief intro from Brad Solomon: https://realpython.com/python-matplotlib-guide/
* Cheatsheet: [https://github.com/rougier/matplotlib-cheatsheet](<https://github.com/rougier/matplotlib-cheatsheet>)
* Notes from Brandon Rohrer: https://e2eml.school/blog.html#133