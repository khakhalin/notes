# Open Street Map

Parents: [[gis]]
See also: [[maps]]

#gis #maps


# Overpass

A query language to get data from OpenStreetMap.
Link: https://overpass-turbo.eu/

Sample queries:

Get all eateries near a certain point
```
[out:json];
node["amenity"~"bar|biergarten|cafe|fast_food|food_court|ice_cream|pub|restaurant|snack_bar|food_stand|marketplace|deli|diner|bakery|brewery|butcher|coffee_shop|convenience|grocery|juice_bar|kiosk|supermarket|burger"](around:500,52.506937,13.448697);
out center;
```