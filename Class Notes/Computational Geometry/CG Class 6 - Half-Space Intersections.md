---
tags:
  - computational-geometry
---
## Motivation: Manufacturing by Mold Casting
When casting a mold, we have some rules to consider:
* The mold must be a single piece
* We cannot break the mold
* The mold must be rigid e.g. not silicone
* We will only mold polyhedral objects, so no curved surfaces
* Must remove object using a translation only

# Castable Problem Statement
Given a polyhedron with polygonal facets, can it be cast from a single mold?
What shape is the mold?
How is the part oriented in the mold?
Which is the top facet?
What direction is the object to be translated to remove it from the mold?
	This translation is not necessarily perpendicular to the top facet of the mold, and it may not be unique
We will only need to make one such translation, we will never need to change direction.

### Lemma 4.1
The polyhedron $P$ can be removed from its mold by a translation in direction $d$ iff $d$ makes an angle of at least 90$\degree$ with the outward normal of all ordinary facets of $P$. 
If the piece collides with mold facet $\hat{f}$ it must have angle $> 90\degree$, which would imply an angle $< 90\degree$ with the corresponding piece facet $f$.

#### Dot Product
For two unit vectors, $u\cdot v=u_xv_x + u_yv_y + u_zv_z$
$$u\cdot v =\left\{\begin{array}{lcr}u=v&\iff&1\\u\perp v&\iff&0\\u=-v&\iff&-1\end{array}\right.$$

## Dual Representation: Planar Constraints
Every upwards direction $d=(d_x,d_y,d_z)$ can be represented as a point on the $z=1$ plane: $d=(d_x,d_y,1)$
This isn't a unit vector but that's okay.
This lets us turn a 3D problem into 2D.
All valid solutions to the unmolding problem form a region on the plane.

Each facet places a linear constraint on the valid unmolding directions
$$n_xd_x+n_yd_y+n_z\leq0$$
This half-plane space can be visualized on our dual representation $z=1$
The region is the intersection of the linear constraints from each face of the piece.
That region is convex!
The region may be:
* Finitely bounded
* Unbounded
* A degenerate case of an intersection at a single point
* Infeasible with no solution

Given an input polyhedron with $n$ facets, try each facet as the top facet
Intersect the half-spaces of all other facets
If the result is non-empty, we have a solution

## Compute Half-space Intersection
Given $n$ linear constraints
Intersection will be a convex region in the $z=1$ plane with at most $n$ edges
Compute intersection via Divide & Conquer
* Split half spaces into two groups
* Compute Intersection
* Merge Intersections

### Merging Two Convex Regions
From [[CG Class 3 - Map Overlay & Adjacency Data Structures#Line Sweep Algorithm for Map Overlay|last lecture]] we found we can compute the intersection/overlay polygonal shapes in $O(n\log n+k\log n)$, $k$ is the # of faces on output polygon and $k\leq n$
In the worst case, sweep line horizontal complexity is constant, not $n$.
We need to track left & right faces of each shape $C_1\ \&\ C_2$
We can handle unbounded shapes by setting one or more of these edges to `NULL`

![[Half Space Intersection.png]]

## Is it Castable? Algorithm Analysis
Given an input polyhedron with $n$ facets. 
1. Try each facet as the top facet $\implies O(n)$
2. Intersect the half-spaces of all other facets
	1. Merge 2 convex regions $\implies O(n)$
	2. Divide & Conquer Recursion $\implies O(n\log n)$
3. If it is non-empty, we have a solution!
Overall $\therefore O(n^2\log n)$

We don't need every solution, we only need 1 solution!

# Incremental Linear Programming
We want to maximize an objective function $c_1x_1+c_2x_2+\dots+c_dx_d$ subject to constraints $a_{i,1}x_1+\dots+a_{i,d}x_d\leq b_i,\ 1\leq i\leq n$ 

Order the half-space constrains in some order $h_1,h_2,h_3,\dots,h_n$
We will solve incremental versions of the problem: $C_1,C_2,C_3,\dots,C_n$
Which have optimal solutions $v_1,v_2,v_3,\dots,v_n$
$C_i$ has half-space constraints $\{h_1,h_2,h_3,\dots,h_n\}$ with solution $v_i$

At each step we add the next half-space constraint $h_{i+1}$
There are 3 cases:
1. The problem is infeasible and there is no solution $\implies O(1)$
2. The problem remains satisfied, and $v_i=v_{i+1}\ \implies O(1)$
3. The problem remains satisfied but we must compute a new $v_{i+1} \implies O(n)$
	1. This new solution must lie on constraint $h_{i+1}$ and must intersect all previous half-spaces

## Analysis
Order the half-space constrains in some order $h_1,h_2,h_3,\dots,h_n$
We will solve incremental versions of the problem: $C_1,C_2,C_3,\dots,C_n$
$\implies O(n)
Which have optimal solutions $v_1,v_2,v_3,\dots,v_n$
$C_i$ has half-space constraints $\{h_1,h_2,h_3,\dots,h_n\}$ with solution $v_i$
Overall: $O(n^2)$
!!!This is worse!!!

## Randomized Linear Programming
Same program, but order the half-space constraint randomly!
Case 3 when adding a constraint becomes rare!
Overall: $\implies O(n)$ expected
In the best case, we get $O(n)$, where every additional half-space is satisfied by the current solution
In the worst case, we get $O(n^2)$, where every additional half-space requires the solution to be updated
On average, of all the possible orderings, very few are the worst case

## Is it Castable? Algorithm Analysis Summary
Given an input polyhedron with $n$ facets. 
1. Try each facet as the top facet $\implies O(n)$
2. Intersect the half-spaces of all other facets
	1. Worst case incremental linear programming $\implies O(n^3)$
	2. Divide & Conquer convex polygon intersection $\implies O(n^2\log n)$
	3. Expected randomized linear programming $\implies O(n^2)$