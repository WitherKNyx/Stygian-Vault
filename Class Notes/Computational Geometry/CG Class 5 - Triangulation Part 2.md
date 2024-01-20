---
tags:
  - computational-geometry
---

[[CG Class 4 - Triangulation Part 1#How to find a Valid Diagonal|Last time]] we talked about finding a diagonal, but this was slow, so how do we speed up and find a better algorithm?

Convex polygons are nice!
Triangle fans and triangle strips are easy ways to triangulate a convex polygon
Unfortunately its hard to make a concave polygon into a convex polygons

### Monotone with Respect to Y-axis
Iff the intersection of the polygon with any line perpendicular to the y-axis is connected, then that polygon is monotone with respect to that line.
This intersections is either
* Empty (outside the polygon)
* One point (top or bottom vertex)
* A line segment

If a polygon is monotone, we can start from the top vertex and walk down the left side to the bottom vertex. 
Each step moves down or horizontally, never upwards.
Same for the right side of the polygon

If a polygon is not monotone, we will need to break this polygon into pieces at the "turn vertices"

## Vertex Types
There are 5 types of vertices
* Regular vertices, which are unremarkable
* Start Vertices
* End Vertices
* Split Vertices
* Merge Vertices
From a Start or a Split vertex, connected edges go down
	Above, a Start vertex is bounded by the outer face, and the Split vertex is bounded by the polygon
	If edges are directed, check the angle between the incident edges. Outer angle > 180 is a Start vertex, < 180 is a Split vertex
From a End or a Merge vertex, connected edges go up
	Below, a End vertex is bounded by the outer face, and the Merge vertex is bounded by the polygon
	If edges are directed, check the angle between the incident edges. Outer angle > 180 is a End vertex, < 180 is a Merge vertex

To summarize,
* Start - outer angle > 180, connected edges go down
* End - outer angle > 180, connected edges go up
* Split - outer angle < 180, connected edges go down
* Merge - outer angle < 180, connected edges go up

##### Y-monotone Lemma
A polygon is y-monotone if it has no split or merge vertices.
Equivalent statement: If $\geq3$ points are on a line, there must be a split/merge vertex

## Eliminating Merge and Split vertices
Could we just take merges and splits and connect them left to right?
We can! But it can break :(
We may not have the same amount of merge and split vertices
We can have multiple merges connect to the same split, or vice versa
We might have no (split xor merge) vertices! We may still be able to connect them somewhere else
What if the diagonals intersect the original polygon? Or each other??

---
## UGH
---
## So what do we do?
Cut polygon on a diagonal going upwards from every split vertex and downwards from every merge vertex
Make sure these diagonals do not intersect!
### How to find the vertex to connect to
Perform a line sweep! from top to bottom
When we find split vertex $v_i$, connect it to a vertex above us
Find a line to the left, $e_j$, and to the right $e_k$ of $v_i$ on the current sweep line
Locate the lowest point between these two lines (merge vertex)
If none, take the upper end point of either $e_j$ or $e_k$

...we're still not done

# Triangulate a Monotone Polygon
We can't just make a triangle strip, it's more complicated than that
>Steps
1. Sort all of the points vertically
2. Push the top two points onto a stack data structure
3. Process the remaining points one at a time from top to bottom
4. If you can, make a triangle with the new point and the last two points on the stack
	1. Remove 1 point (point that can no longer be connected to)
	2. Repeat
5. If not, push the new point on the stack

Vertices currently on the stack form an upside down funnel on one side (A)
The next vertex may be from the other side (B) and create a fan, leaving only 2 vertices on the stack
![[Triangulation Fan.png]]
Or it may be on side A and 
* Bend the funnel further from the vertical axis
![[Triangulation Funnel.png]]
* Form one or more triangles
 ![[Triangulation Triangles.png]]
## Analysis
* Line sweep algorithm (cutting into monotone polygons $\implies O(n\log n)$
	* Sort all vertices vertically $\implies O(n\log n)$
	* Maintain horizontal sorting of active vertices $\implies O(\log n)$
	* Locate helper vertex for each split/merge $\implies O(\log n)$
* Use tack to triangulate monotone polygon $\implies O(n)$
	* Don't need to sort
	* Each vertex is added once $\implies O(1)$
	* Each vertex beyond the first two adds one triangle when it is removed from stack $\implies O(1)$
Overall: 
$\therefore O(n\log n)$

This algorithm also works for non-simple polygons (interior holes) and arbitrary planar subdivisions!

# Additional Triangulation Goals
Triangulation of a polygon is not unique!
For some applications, certain triangulations are better than others
![[Triangulation Goals.png]]

## Degenerate/Ill-conditioned 2D elements
aka how equilateral are triangles?
* Maximize the minimum angle
* Minimize the maximum angle
* Maximize the shortest edge
* Ratio of longest edge to shortest edge
* Ratio of area to area of circumscribed circle

## Degenerate/Ill-conditioned 3D elements
aka how equilateral are the tetrahedra
* Ratio of volume$^2$ to surface area$^3$
* Smallest solid angle
* Ratio of volume to volume of the smallest circumscribed sphere
### Important
![[Bunny Deformation.png]]
Video: https://docs.google.com/file/d/1kReAWAWIodhAfPdOQQ8TFjY8i4Alfsl7/preview