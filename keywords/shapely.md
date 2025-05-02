# Shapely (and by extension, geopandas)

Parents: [[python]], [[gis]]
See also: [[crs]], [[pandas]]

#gis #python


# Object creation

* `shapely.Point([x, y])`
* line 🔥 
* polygon 🔥 

# Set operations
* `a.union(b)` - union of  two objects
* `unary_union()` - union of many objects
* `a.intersection(b)`
* `a.difference(b)` - subtract b from a
* `a.symmstric_difference(b)` - xor
* `shapely.contains(polygon, x, y)` - if a polygon contains a point with this pair of coordinates

When subracting two _almost identical_ objects from each other one can get an error of `TopologyException: found non-noded intersection`. It happens when two lines are almost identical, but not quite, resulting in a difference that is small enough to become a shape. Some stackoverflow solutions recommend rounding coordinates down, but for GPS shapes, even just 4 digits after the point can still produce this effect. A better solution if "almost identical shapes" are expected is to buffer the shape to be subtracted, to make sure it subtracts everything clean (say, 0.000001 of GPS is about 10 cm, which is usually negligible).

# Properties
* `s.area` - area
* `s.bounds`- bounding box (as a 4-element tuple, xyxy)

# Type manipulation

Note that these guys are all class properties, and not methods, so they don't need brackets, and throw an error if you try to add them, as you can't call an object like that!

* `p.exterior` - exterior line of a simple polygon; can run `.coords` on that. Multipolygons don't have an exterior, but do have a boundary (next)
* `mp.boundary` - for simple polygons, same as `exterior`; for multipoligons - a multiline
* `l.coords` - for points and lines, returns coordinates. For multi-objects, throws an error. 
    * Returns a "Coordinate sequence", which seems to be a lazy iterable
    * It's also possible to do `l.coords.xy`, which is a tuple of tuples and can be cast to array by `np.array()`. Note that you always get a 2D array, even for a point (so for a point, do a `.squeeze` on it)
* `mp.explode` - explodes a multipolygon into a bunch of polygons. Probably works for multilines as well?

# Shape modification
* `a.buffer(0.5)` - grow, or erode, if called with a negative value. Can buffer points and lines, no problem.
* `a.buffer(0.5, single_sided=True)` - in one direction only; change the sign to change the direction
* `a.simplify(tolerance)` - re-polygonize
* `a.parallel_offset(distance, left, resolution=16, join_style=1, mitre_limit=5.0)` - draws a line nearby
    * `distance` - positive float
    * `side` - either `left` or `right`
    * `resolution` - how many points to use (same as in `buffer()`)
    * `join_style` - 1=round, 2=mitre (pointy, up to `mitre_limit` parameter that can be set separately), 3=bevel

# Vectorized methods

Several key methods (most critically, the `contains()` method are avialble in a fast vectorized form. To use them, separately import `shapely.vectorized` (it's not imported if you just do `import shapely`), and then instead of calling commands sirectly, add `.vectorized` suffix after shapely, e.g. `shapely.vectorized.contains(...)`

Note that as of some recent update, vectorized contains became picky about the type of `x` and `y` in this example, and while it still technically takes any iterable, it throws a warning if it's not a numpy array. So better cast once more, and give it a numpy array (e.g. add `.values` after Pandas series).

# Todo
* MultiPoint.convex_hull 🔥 
* object.envelope
* object.minimum_rotated_rectangle
* efficient unions
* affine transforms
* voronoi diagrams
* delaunay triangulation

# Refs

https://shapely.readthedocs.io/en/stable/manual.html
