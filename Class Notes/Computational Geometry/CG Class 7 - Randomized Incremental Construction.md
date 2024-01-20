---
tags:
  - computational-geometry
---
## Motivation/Applications
### Collision Detection
* VR/Video Games
* Robotics
* Scientific Simulations

 1. Simulation over time
 2. Detect collisions
 3. Compute response
	 1. Force of impact
	 2. Damage (deformation or fracture)
	 3. Bounding/change of direction

#### Intersect two spheres
To compute whether two spheres intersect, compute if the distance between the centers is less than the sum of the radii.

#### Cost of collisions
If we have $n$ bouncing ping pong balls inside of a box?
If we add a stationary bunny statue w/ 60,000 faces inside the box?
If we add $b$ bunnies bouncing around inside the box?
Naive method:
Every frame of animation/simulation, intersect every sphere/triangle in motion with every other sphere/triangle both stationary and in motion
$\therefore O((n+bf+6)(n+bf)) \sim O((n+bf)^2)$

### Ray Tracing
Cast $g$ rays to simulate photons bouncing off of objects
Naive method:
Intersect every ray with every triangle
$\therefore O(gf)$

#### Conservative Bounding Region
Check for a ray intersection with a conservative bounding region
If it doesn't intersect the bounding shape, then we don't need to check against every triangle!
Requirements:
* tight --> avoid false positives
* fast to intersect
* easy/fast/perfect construction (less important)

### Robot Placement
We need a fixed-base robot to reach a bunch of objects from a set of $n$ known positions
What is the smallest arm length necessary?
Where should the base be located?

# Brute Force Minimal Smallest Bounding Circle
Input: $n$ 2D vertices
Assume "General Position":
* No 3 points are collinear
* No 4 points lie on the same circle
Output: 3 of those vertices uniquely define a circle such that all other points lie inside of that circle
In 3D, we would output 4 vertices to uniquely define a sphere

How do we fit 3 points to a circle? Given points $(x_1,y_1),\ (x_2,y_2),\ (x_3,y_3)$, find circle with center $(x,y)$ and radius $r$.
Solve system of equations: $$\begin{align*}(x_1-x)^2+(y_1-y)^2=r^2\\(x_2-x)^2+(y_2-y)^2=r^2\\(x_3-x)^2+(y_3-y)^2=r^2\end{align*}$$
To test if a point is inside/outside circle, evaluate $(x_1-x)^2+(y_1-y)^2$ and compare to $r^2$.
$<\ \implies$ Inside the circle
$=\ \implies$ On the circle edge
$>\ \implies$ Outside the circle

## Construction Algorithm
Input: $n$ vertices in 2D
* For every triplet of points $\implies O(\binom{n}{3})=O(n^3)$
	* Compute circle $\implies O(1)$
	* Check against all other points $\implies O(n)$
		* Reject if any are outside circle
Overall: $O(n^4)$

# Bounding Circle by Center of Mass
* Let the center = average of all of the vertices $\implies O(n)$
* Find point furthest from center, use that to set the radius $\implies O(n)$
* Are all points on or inside this circle? Yes!
* Overall running time? $O(n)$
* This may not be an optimal solution, but its ok for graphics applications!

# Incremental Construction of Smallest Bounding Circle
Given $P=\{p_1,p_2,p_3,\dots,p_n\}$
Make a circle with the first 3 points $p_1,p_2,p_3$, then loop over the remaining points.
For $4\leq i\leq n$
* If $p_i$ is inside the circle, then the solution for $\{p_1,\dots,p_{i-1}\}$ is the solution for $\{p_1,\dots,p_i\}$
* If $p_i$ is outside the circle, then compute a new circle. $p_i$ is on the circle solution for $\{p_1,\dots,p_i\}$
If the current circle is fit to points $a,b,c$, can we prove/disprove that adding $p_i$ will be a circle fit with $p_i$ and two of $a,b,c$? Would be constant time
Do we need to consider all other points? Yeah :(
* If we know one point is on the boundary
	* Make a circle with $p_i,p_1,p_2$
	* For $3\leq j\leq n$
		*  If $p_j$ is inside the circle, then the solution for $\{p_i,p_1,\dots,p_{j-1}\}$ is the solution for $\{p_i,p_1,\dots,p_j\}$
	* If $p_j$ is outside the circle, then compute a new circle. $p_j$ is on the circle for $\{p_i,p_1,\dots,p_j\}$
* If we know two points are on the boundary
	* Make a circle with $p_i,p_j,p_1$
	* For $2\leq k\leq n$
		*  If $p_k$ is inside the circle, then the solution for $\{p_i,p_j,p_1,\dots,p_{k-1}\}$ is the solution for $\{p_i,p_j,p_1,\dots,p_k\}$
	* If $p_k$ is outside the circle, the solution for $\{p_i,p_j,p_1,\dots,p_k\}$ is the circle fit to $p_i, p_j, p_k$

## Analysis
* Incremental construction with two known points is $O(n)$
	* Have to check each of the $n$ points
	* Compute a new circle at most $n$ times
* Incremental Construction with one known point
	* Worst case: $O(n^2)$ 
		* If we compute a new circle, calling two known points function, $n$ times
	* Best case: $\Omega(n)$
		* Never or rarely call the two known points function
Overall: 
$O(n^3)$ if we have to compute a new circle, the one known point function, $n$ times
$\Omega(n)$ if we never or rarely call the one known point function

# Randomized Incremental Construction
If we randomize the initial order of the points, we will rarely need to call the helper functions to compute the circles
To see why, work backwards removing one point from our set at a time
We only need to shrink the minimal bounding circle if we select one of our 3 circle defining points
Expected: $O(1)$ circle recomputes $\times O(n)$ per recompute $\implies O(n)$
It's not magic though! We can't use this on every problem
Randomized Incremental Construction (RIC) only works if:
* It is fast to test if new item works with the current optimal solution
* When new item does not work,
	* Current solution can be used to compute the new optimal
	* and it will be faster than starting over from scratch

