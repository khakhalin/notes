# Bezier curve

#algo

A nice spline to connect two points with a pretty smooth curve.

Algorithm: two points (P1 and P2, to connect them), and a 3d point (P3), to define the shape. At every step (t runs from 0 to 1):
* point A linearly slides from P1 to P3
* point B linearly slides from P3 to P2
* point on a Bezier curve C linearly slides between A and B (linearly slides on a moving segment)

In vector form:

$C(t) = t \big( t \cdot p_1 + (1-t) \cdot p_3 \big) + (1-t) \big( t \cdot p_3 + (1-t) \cdot p_2 \big)$