# CRS = Coordinate Reference Systems

Home: [[gis]]

#gis


### CRS systems that are useful in practice

* 4326 - [[gps]] systems work in this one, so it's kinda a "raw data" system
* 3875 - best system to show maps (pseudo-Mercator?), as it preserves shapes, but it doesn't preserve scale
* 3035 - a good system to measure distances and areas (all measurements are in meters), but not a good one to plot
* 4647 - I was looking for a crs to draw metric circles, and this one seems to be working, but technically it seems to just be a regional projection for Europe. It seems to be working for North America as well (at least the bond box for a "circle" ends up being reasonably square), but I kinda expect it not to work for some location, as technically to draw a circle you need to use a local projection for this location specifically, not just one "general projection".