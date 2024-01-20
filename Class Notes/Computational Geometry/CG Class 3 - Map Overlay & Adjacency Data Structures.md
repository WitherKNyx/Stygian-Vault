---
tags:
  - computational-geometry
---

### Motivations
* Cartography is more complicated than [[CG Class 2 - Line Segment Intersections|Line Intersections]]
* How to describe and store regions
* How to overlay, intersect, and union regions
* Find total length of lines through regions
* Find total area of regions overlapping regions

### Boolean Operations
Union of two polygons is the combined area
Intersection of two polygons is the area shared by both polygons
Difference is when a polygon blots out part of another polygon

## How to represent regions of a plane
A single map layer will divide the plane into non-overlapping regions
These regions:
* Are two-dimensional, AKA planar
* May not be convex
* May have holes
* May be disconnected

## Planar Subdivision
Edges are straight lines
Edges do not include endpoints AKA edges are open
A face doesn't include any points on edges nor any vertices
Exactly one face, the "outer face", is unbounded

### Euler's Formula
Given V vertices, E edges, and F faces on a connected graph:
$$V - E + F = 2 \Longleftrightarrow V + F = E + 2$$
For unconnected graphs:
$$V - E + F > 2$$

## Minimal Representation
 List of Edges?
	Hard to identify number of faces if graph is unconnected. If connected, use Euler's formula. 
	Harder to identify any particular face, ***ESPECIALLY*** if there is a hole.

List of Polygons?
	It is expensive (& not robust) to query faces adjacent to other faces
	How do you represent holes?

List of Unqiue Vertices & Indexed Faces
-> Define vertices and then define faces by indexing vertices
	Good for storage! Efficient
	Expensive to query what faces use a specific vertex

All these list representations have no neighbor/adjacency information and feature many linear time searches
Adjacency is implicit in a structured mesh, but not in unstructured meshes

# Adjacency Data Structures
In addition to geometric data (position) and attribute information (color, texture, temperature, etc.), let's store topological data (adjacency, connectivity)

## Exhaustive Adjacency
Each element (vertices, edges, and faces) contains a list of pointers to all incident elements
Queries depend only on local complexity of mesh
Data structures do not have a fixed size
Slow, big, lots of work to maintain

## Fixed Storage - Winged Edge
Edges store everything! Vertices and faces point to an edge
Edges store the two incident vertices, the two incident faces, and Left+, Left-, Right+, and Right-.
These last four represent the next and previous edge of the left and right face in clockwise order
To gather faces around a vertex, we can walk Left+, Right-, Left+, Right-... until we get back to our original edge. # of edges traversed is faces
>Is this true?

### Consistent Edge Orientation
It's desirable to have a consistent orientation for edges that define the boundary of a face, this would indicate which points are inside the face, especially if their are holes
**Most meshes cannot be labeled such that the edges of every face are consistently oriented**

## Fixed Computation - Half-Edge/Doubly-Connect Edge
Every edge is represented by two directed half edge structures.
Each edge stores 
* the vertex at the end of its direction
* the face to the left of the edge
* the next half-edge counter-clockwise around the face on left
* a symmetric half-edge
Orientation can be done consistently like this

### How to...
Find other vertex of the edge?
	Symmetric half-edge, vertex

Other face of the edge?
	Symmetric half-edge, face

Clockwise edge around the face at the left?
	Loop around face

All edges surround the face at the left?
	{ Next edge } until we repeat an edge

All the faces surrounding the vertex?
	{ Next edge, Symmetric half-edge } until we repeat an edgeF

### Analysis
Data Structure size is fixed (Unless interior holes are allowed, then faces wil lneed to store a list with one edge for each hole)
Can store geometric information at vertices, attribute information in vertices, half-edges, and/or faces, and topological information half-edges only
Orientable surfaces only
Local consistency everywhere implies global consistency
Time complexity is linear in amount of info gathered

# Line Sweep Algorithm for Map Overlay
Input: Doubly connected, half-edge representation for planar subdivisions $S_1$ and $S_2$
Output: Doubly connected, half-edge representation for overlay subdivision $O(S_1,S_2)$
Every face in overlay is labeled with the attribute info from a face from $S_1$ and $S_2$

Steps:
1. Copy all half-edges from $S_1,\ S_2$ to $D$
2. Perform the [[CG Class 2 - Line Segment Intersections#Line-Sweep/Plane-Sweep Algorithm|line sweep edge intersection algorithm]] from Lecture 2 to identify intersections between segments in $S_1$ and $S_2$
	These edges in $D$ will need to be edited - cut at intersection points. New vertices, edges, and faces.

Events that will be encountered:
* Vertex in $S_1$
* Vertex in $S_2$
* Intersection between Vertex in $S_1$ and Vertex in $S_2$
* Intersection between Edge in $S_1$ and Vertex in $S_2$ (Need to add new edges and edit previous edges)
* Intersection between Vertex in $S_1$ and Edge in $S_2$ (same as above)
* Intersection between Edge in $S_1$ and Edge in $S_2$ (Need to add new edges, edit previous edges, and add a vertex)
Also need to reconnect symmetric edges and update next edge cycles

Then need to construct new faces
* Determine cycles of edges
* Determine outer boundaries
* Create unbounded face
* Determine inner components of each face
* Determine connected components

## Analysis
Let $S_1$ and $S_2$ be a subdivision of complexity $n_1$ and $n_2$ with $n = n_1 + n_2$ (complexity ~ edges)
Overlay of $S_1$ and $S_2$ can be constructed in $O(n\log n + k\log n)$ where $k$ is the complexity of the overlay

Steps:
1. Copy edges from $S_1$ and $S_2$ $\implies O(n)$
2. Planar sweep $\implies O(n\log n + k\log n)$
3. Constructing faces $\implies O(k)$
4. Labeling faces with the face attributes from $S_1$ and $S_2$ $\implies O(n\log n + k\log n)$
Check book for step 4

### Complexity of the overlay
![[Worst Case Polygon Intersection.png]]

Each shape has 6 vertices, 6 edges, and 2 faces
Overlay has 48 vertices, 84 edges, and 38 faces
In the worst case $k$ is $O(n_1n_2)=O(n^2)$