---
tags:
  - computational-geometry
---
## Motivation
There are a bunch of grocery stores spread across a country.
You're planning to open another grocery store at a specific location.
How many customers can you expect at the new store location? Customers will only choose the new store if it is closer to their home than their current store.
Assumptions we make:
* The price of a good is the same at every site
* The cost of acquiring the good is the price + transportation to the site.
* The cost of transportation is the Euclidean Distance times a fixed price per unit distance
* Consumers try to minimize cost of acquiring the good

### Metrics
* Euclidean Distance - straight-line distance between two points
* Manhattan Distance - only travel along axis aligned roads
* Geodesic Distance - shortest distance between two cities, accounting for the fact the earth is a spheroid

## Voronoi Observations
Points on the edge between two Voronoi cells are equidistant from two Voronoi sites
Edges of Voronoi cells are perpendicular bisectors of two Voronoi sites

All points that lie on one side of the perpendicular bisector are the half-space of points that will choose site A over site B because site A is closer.
This suggest a brute force algorithm

Because a Voronoi cell is the intersection of half-spaces, a Voronoi cell must be convex
Some Voronoi cells are unbounded
If all Voronoi sites are collinear - a degenerate configuration, it is easy to construct:
Sort by y-coordinate, get adjacent perpendicular bisectors , these are walls of unbounded slabs
 \* | * | *   |   *  |  * | *
 All edges are parallel lines and all cells are unbounded areas
 If even one site is not collinear, every perpendicular bisector will intersect with at least one other perpendicular bisector, and every Voronoi edge will be bounded on one or both directions

Given $n$ Voronoi sites, the diagram will have $n$ Voronoi cells/faces
A single cell may have many edges, as many as $n-1$
By [[CG Class 3 - Map Overlay & Adjacency Data Structures#Euler's Formula|Euler's Formula]], $$\begin{align*}V\leq2n-5\\E\leq3n-6\end{align*}$$
To apply Euler's Formula, we create $V_\infty$ to connect all unbounded edges
Sum of all vertex degrees = $2E\leq3(V+1)$
Every edge touches 2 vertices, with a minimum vertex degree of 3

# Brute Force Voronoi Algorithm
Given $n$ Voronoi sites
* For every point: $\implies O(n)$
	* For every other point:$\implies O(n)$
		* Construct perpendicular bisector between two points, defines a half-spaces$\implies O(1)$
	* Intersect $n-1$ half-spaces, Divide & Conquer recursion, polygonal Sweep Line overlay$\implies O(n\log n)$
Overall:$\implies O(n^2\log n)$

### Voronoi Vertices
A Voronoi vertex, $q$ is the center of a circle that passes through 3+ Voronoi sites and does not contain any other sites inside of it
It is also the intersection of 3+ perpendicular bisectors of those Voronoi sites

## Alternate Brute Force Construction Algorithm
* For all triples of Voronoi sites:$\implies O(n^3)$
	* If they are not collinear
	* Construct the circle
	* Check to see if any other Voronoi site lies within the circle$\implies O(n)$
	* If not, keep the circle center as a Voronoi Vertex
 Overall: $O(n^4)$

# Voronoi History
Ukranian Mathematician Georgy Voronoy
PhD advisor, Andrey Markov: Markov chains & Markov processes
PhD student, Waclaw Sierpinski: Sierpinski Triangle
PhD student, Boris Delaunay: Delaunay Triangulation
AKA
Dirichlet Tesselation, by German Mathematician Peter Gustav Lejeune Dirichlet
AKA
Thiessen Polygon, by Meteorologist Alfred H. Thiessen

### How to Graph a Parabola
All points on a parabola are equidistant from a point, the focus $f$, and a line, the directrix $d$
For $x^2$, $f=(0,0)$ and $d= \{y=0\}$
For a parabola $ax^2+bx+c$, rewrite as $(x-h)^2=4p(y-k)$
$f=(h,k+p)$ and $d=\{y=k-p\}$

# Sweep Line Algorithm to Construct a Voronoi Diagram
AKA Fortune's Algorithm, by Steven Fortune
* Sort the Voronoi sites vertically
* Move sweep line from top to bottom
* Region above the "beach line" is known
* Region between the beach line and the sweep line may be impacted by Voronoi sites below the sweep line!
* ![[Beach Front.png]]
### Beach Line
The beach line is a sequence of parabolic segments.
Each Voronoi site above the sweep line creates a parabola, where the site is the focus and the sweep line is the directrix 
Beach line is the lowest point of all of the parabolas for every $x$ value
Bach line is $x$-monotone, every vertical line intersects it exactly once.
Breakpoints - intersection of two parabolic arcs - trace edges between two Voronoi cells
[Video of Sweep line algorithm](https://www.youtube.com/watch?v=k2P9yWSMaXE)

## Events:
* New Parabola Appears
	* Only happens when the sweep line reaches the next Voronoi site.
	* Initially the parabola is degenerate, with width of 0
	* New Voronoi edge
		* New Parabolic Arc adds two new breakpoints, with a new Voronoi edge traced between those breakpoints
* Parabolic Arc is absorbed
	* Arc disappears when beach line reaches point $q$, a Voronoi vertex, center of a circle that passes through 3+ Voronoi sites/parabola foci

## Analysis
* For $n$ Voronoi sites
* New Arc events: Sort Voronoi sites vertically $\implies O(n\log n)$
* Keep a horizontal sorted ordering of the parabolic arcs on the current beach line (max of $2n$ arcs)
* (Potential) Arc absorption events: For each triple of neighboring arcs $\alpha,\alpha',\alpha''$ on the beach line, compute the circle and tangent sweep line $\implies O(n)$ Voronoi vertices
* Move sweep line to next event
Overall: $O(n\log n)$
