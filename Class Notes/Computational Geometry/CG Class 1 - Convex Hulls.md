---
tags:
  - computational-geometry
---

A shape is convex iff for every pair of points $p$ and $q$, $pq$ lies entirely within the shape. Else it is concave.
A convex hull is the shape with the smallest area which contains all of a set of points.
How to find it?
### Real-world algorithm
Points are nails on a board.
Take a rubber band and stretch it around all points.
Release rubber band. The resting position of the band is the convex hull.

Important consideration, generally we don't like > 2 colinear points.

### Naive algorithm
For each pair of points, check if any points lie on the left side of the line. Then sort the points in counterclockwise order and print them.

Checking if a point lies on a side of a line can be achieved with the cross product.
This is $O(n^3)$

This algorithm is prone to floating point errors when dealing with colinear/near colinear points

## Upper hull algorithm
Sort points by x coordinates and start with leftmost point, which must be on the hull. Walk from left to right and add the next point to the hull. check if the angle formed by the new point makes a left bend. If so, remove the second to last point and repeat test until it fails.

This is $O(n\log n)$, dominated by sorting. Removing points is $O(n)$ max total cost

## Gift wrapping algorithm
Find smallest x-coordinate point. Starting there, at each point find the point with the smallest outer angle to the previous points. Repeat until you get to $p_0$ again. 

This is $O(nh)$, h is points on hull. This is better if h is minimal, i.e. $h < n\log n$, but worse if h is maximal, i.e. h ~ n

## Divide and conquer algorithm
Split step: sort points by x-coord. split into 2 equal groups, recurse
Merge step: find rightmost point in left hill and leftmost point in right hull. Walk down to find lower tangent and up to find upper tangent, and discard points in-between.

This is $O(n\log n)$ because of sorting points.