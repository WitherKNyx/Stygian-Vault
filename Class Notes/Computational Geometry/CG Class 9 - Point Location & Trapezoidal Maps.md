---
tags:
  - computational-geometry
---

# NOTE: LOOK AT BEGINNING SLIDES AFTER LOOKING OVER CLASS 8

## Motivating Applications
* Point Location
	Given a 2D coordinate (Latitude & Longitude), what region of the ocean contains this point?
	Access currents, weather, etc.
* 2D/3D Mouse Picking for Graphics
	Get the 3D world coordinates of a 2D mouse click
	Identify which object was selected and the point on the object closest to the click
* 3D Painting
	Painting a 3D model!

# Brute Force Point Location
## Pick by Ray Casting
Construct a ray from the eye through the image plane into the scene
Intersect with all objects in the scene
Keep the closest
Concerns:
* Cost of intersection
* How often are you asking?
	* On click?
	* Continuously?
* Position imprecision/noise
Given a planar subdivision and a query point $Q$, which triangle/polygon is $Q$ inside of?

### Is Query Point inside a specific Triangle?
Compare the point to each line segment
Are you on the right side of all three line segments?
Or are you on the wrong side of one or two segments?
Use the cross product again!

Does the half-edge adjacency data structure accelerate this query? NO!
While we can navigate to the adjacent neighbors, we can not do better than an $O(n)$ linear flood fill

## Point location by Vertical Slab
Given $v$ vertices, $n$ edges, and $f$ polygonal faces, which polygonal region contains the query point $Q$?
Let's slice the plane into vertical slabs
Draw a vertical line through every point
Let's assume "General Position":
* No two points have same $x$ coordinate
* There will be no vertical segments!
* The query point will not be on a vertical segment or a vertex

Within this slab, the line segments:
* Do not cross (guaranteed by planar subdivision construction)
* Do not start or stop (split at every vertex)
We can sort the line segments vertically (left endpoint's $y$-coordinate)
Which trapezoid is $Q$ located within?
	Each trapezoid is mapped back to the original polygonal face

Is Query Point above or below line segment?
$P_1 < Q_x < P_2$
Is $0<\theta<180$
![[Point Above Line Segment.png]]

### Cross Product
If $0 < \theta < 180$, then $a\times b > 0$ in the z-axis
If $180 < \theta < 360$, then $a\times b < 0$ in the z-axis
If $a \parallel b$, then $a \times b = 0$
$|a\times b = \sin\theta$
$$a\times b=\begin{vmatrix}\hat{\textbf{i}}&\hat{\textbf{j}}&\hat{\textbf{k}}\\a_1&a_2&a_3\\b_1&b_2&b_3\end{vmatrix}$$
## Analysis: Running Time
* Algorithm Preprocessing
	* Sort slabs left to right $\implies O(n\log n)$
	* Within each slab, sort trapezoids from top to bottom$\implies O(n\log n)$
* Point Location Algorithm$\implies O(\log n)$
	* Binary search to locate the correct slab between two points $\implies O(\log n)$
	* Binary search to locate the correct trapezoid $\implies O(\log n)$
Overall: $O(n\log n)$ (theoretical)
But we should consider how much memory it uses
## Analysis: Memory Usage
This representation is very costly
We store many faces in many slabs
In the worst case, every polygon appears in nearly every slab! $\implies O(n^2)$
Avg./Expected case is $O(n\sqrt{n})$

# Trapezoidal Map and Adjacency Structure
Horizontally merge some of these cells
Split vertically at every vertex, but stop splitting when you reach the closest line segment above and below
This defines a planar subdivision with full coverage of the plane by non-overlapping convex trapezoids and triangles (which are degenerate trapezoids)

Can we connect these triangles and trapezoids with a classic half-edge adjacency data structure? No!
Many of the faces have one or more "T junctions" which is not generally allowed in a HEADS
![[Half Edge Problems.png]]

Instead, each trapezoid/triangle points to:
* Line segment *top*, which makes the upper boundary
* Line segment *bottom*, which makes the lower boundary
* Vertex *left_p*, which defines the left boundary
* Vertex *right_p*, which defines the right boundary
Additionally each trapezoid $\Delta$ may have up to 4 adjacent neighbors (or `NULL` if they don't exist)
* Upper left neighbor, shares *top* and *left_p*
* Lower right neighbor, shares *bottom* and *left_p*
* Upper left neighbor, shares *top* and *right_p*
* Lower right neighbor, shares *bottom* and *t_p*

Unfortunately this new adjacency structure does not allow us to navigate through the structure more efficiently than an $O(n)$ flood fill.
But we can build a DAG for this structure to perform queries

## Directed Acyclic Graph
Intermediate nodes are vertices and line segments
Leaves are the trapezoidal regions
![[Binary Search for Queried Trapezoid.png]]

### DAG Analysis
\# of leaves = # of trapezoids $\implies O(n)$
\# of intermediate nodes = # of vertices and # of line segments $\implies O(n)$
Height of dag: $\Omega(\log n)\ |\ O(n)$
Use Randomized Incremental Construction to achieve height
Expected case: $O(\log n)$

Randomize the order of the line segments
Insert the segments one at a time
Handle all cases

Memory to store DAG: $O(n)$
Height of DAG: $O(\log n)$
Query time to locate the trapezoid/polygon containing point $Q$: $O(\log n)$
Cost to construct: $O(n\log n)$

This is the same runtime as vertical slabs but with linear memory usage instead of quadratic!

## Graphics Hack!
Take advantage of fast GPU hardware rendering
Color each object a different, unique color w/ no lighting/shading
Grab the color the pixel from the frame buffer (obj id)
Grab the z-value (depth) from the depth buffer
With $2^8\cdot2^8\cdot2^8=16.7$ million colors, we can cover a 4k screen ($4096\times2160=9$ million pixels), but not quite an 8k screen ($7680\times4320=33$ million pixels)