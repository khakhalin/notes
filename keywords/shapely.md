# Shapely

Parents: [[python]], [[gis]]
See also: geopandas, [[crs]]

#gis #python


# Object creation

point 🔥 
line
polygon

# Elementary operations on shapes

### Logic:
* `a.union(b)` - for many objects, use `unary_union()`
* `a.intersection(b)`
* `a.difference(b)` - subtract b from a
* `a.symmstric_difference(b)` - xor

When subracting two _almost identical_ objects from each other one can get an error of `TopologyException: found non-noded intersection`. It happens when two lines are almost identical, but not quite, resulting in a difference that is small enough to become a shape. Some stackoverflow solutions recommend rounding coordinates down, but for GPS shapes, even just 4 digits after the point can still produce this effect. A better solution if "almost identical shapes" are expected is to buffer the shape to be subtracted, to make sure it subtracts everything clean (say, 0.000001 of GPS is about 10 cm, which is usually negligible).

### Type manipulation
* `a.boundary` - (no parentheses) gets a boundary
* `a.buffer(0.5)` - grow, or erode, if called with a negative value. Can buffer points and lines, no problem.
* `a.buffer(0.5, single_sided=True)` - in one direction only; change the sign to change the direction
* `a.simplify(tolerance)` - repolygonize
* `a.parallel_offset(distance, left, resolution=16, join_style=1, mitre_limit=5.0)` - draws a line nearby
    * `distance` - positive float
    * `side` - either `left` or `right`
    * `resolution` - how many points to use (same as in `buffer()`)
    * `join_style` - 1=round, 2=mitre (pointy, up to `mitre_limit` parameter that can be set separately), 3=bevel

### Todo
* MultiPoint.convex_hull 🔥 
* object.envelope
* object.minimum_rotated_rectangle
* efficient unions
* affine transforms
* voronoi diagrams
* delaunay triangulation

# Refs

https://shapely.readthedocs.io/en/stable/manual.html
