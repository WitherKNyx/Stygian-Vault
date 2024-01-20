---
tags:
  - computational-geometry
---

## Motivation
Art Gallery Problem!
What is the minimum # of cameras with 360deg rotation to get 100% coverage of a 2D floor plan?
### Simple Polygon
A simple polygon is a single, single closed polygonal chain boundary which is connected and has no holes or self-intersections

## Art Gallery Problem
How many cameras are necessary for 100% coverage? Where should we place these cameras?
The optimal solution is NP hard :(

Given a convex polygon, we can place a camera anywhere inside to see everywhere in the polygon.
Triangles are always convex
We can chop up a concave polygon into convex polygons and place a camera in each polygon to get 100% coverage

# Triangulation
## Triangulation Theorem
Every simple polygon admits a triangulation, and any triangulation of a simple polygon with $n$ vertices consists of exactly $n-2$ triangles
### Proof By Strong Induction
* A polygon with 3 vertices is a triangle.
* Assume every polygon with $n-1$ or fewer vertices can be triangulated.
* Given a polygon with $n$ vertices, we will draw a diagonal line between two vertices that cuts this shape into two smaller polygons which can be triangulated.
#### Diagonal lines
Diagonals should connect two non-adjacent vertices on the polygonal boundary.
Diagonals must not
* be outside the polygon
* cross any edge
* pass through any other vertex

## How to find a Valid Diagonal
* Start at the leftmost vertex $v$
	* If multiple have the same $x$, choose the one with smaller $y$
* Find vertices $u$ and $w$ adjacent loop of edges with to $v$
* Check if the line $uw$ is a valid diagonal
	* The line does not pass through $v$
	* Does it intersect other line segments | $O(n)$
		* There must be one or more vertices inside $uvw$
		* Starting at the intersection, walk along the boundary to find those vertices
		* Choose $v'$ furthest from the line segment $uw$
		* Draw diagonal from $v$ to $v'$
	* Does it pass through any other vertices | $O(n)$
	* Does it lie completely outside of the polygon | $O(?)$

### How many triangles necessary?
When we draw a diagonal and split the polygon with $n$ vertices into two smaller polygons with $n_1$ and $n_2$ vertices.
Every vertex will be used in exactly one of the two smaller polygons, except for two vertices which are in both polygons
$$n_1+n_2=n+2$$
By induction, triangulations of these smaller polygons will have $n_1-2$ and $n_2-2$ triangles
$$n_1-2+n_2-2=(n_1+n_2)-(2+2)=n+2-4=n-2$$
This gives us $n-2$ cameras for our initial problem. Can we do better?
Yes! We can place cameras on the edge between two incident triangles. This cuts the number of cameras in half
We can place cameras on vertices instead! We can three color the vertices of the polygon and place cameras on the color used the least for $\leq\dfrac{n}{3}$ cameras
Problem: 3-color is NP-Complete, right? :(

### Dual Graph
We place a vertex in the dual graph at the center of every triangle in the primary graph.
We draw an edge in the dual graph connecting two vertices if the corresponding triangles in the primary graph are incident
This dual graph is a tree. We can find the leaves, which are "extremities" of the polygon.
We can perform a depth-first tree walk and color the vertices without duplicate

...unfortunately we cannot do better than $\dfrac{n}{3}$ cameras
/.\\/.\\/.\\ . . . /.\\/.\\/.\
\-----------------
$\left\lfloor\dfrac{n}{3}\right\rfloor$ prongs

### Preliminary Analysis
What is the worst case running time to triangulate a non-convex simple polygon with $n$ vertices?

* Identify a legal diagonal $\implies O(n)$
* Split into two smaller polygons $\implies \Theta(n)$ / $\Omega(\log n)$
Overall $O(n^2)$

However, although we can't do better than n/3 cameras, we ***can*** do better than $O(n^2)$!
Does this work in 3D too? Can we tetrahedralize the interior of a polytope?

We can relate this to mesh simplification by identifying relatively unimportant vertices to remove, removing connected triangles and re-triangulating.