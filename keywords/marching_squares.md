# Marching squares

#algo #image

Path: [[algos]]

A funny algorithm for turning pixely data into vector contours. Based on lookup in a binary table, and interpolation. 

The main idea is that for every square of 4 pixels we'll decide what to draw in the middle of this square. Each pixel is set to either 0 or 1 (thresholding), so we have 16 combinations of binary numbers for every inter-pixel node (4 corners numbered in a circle). A lookup table of 16 elements, from solid colors to a bunch of edges and corners. For saddle points (0101) two solutions are possible; one option is to go back to raw collor numbers (before binary thresholding), look at the average (is it darkish or lightish), and decide accordingly.

A variety with terniary thresholding (high, mid, low) is called **isoband**. It is also possible to do it with triangles rather than with squares. A 3D generalization is also possible.

# Refs

* https://en.wikipedia.org/wiki/Marching_squares