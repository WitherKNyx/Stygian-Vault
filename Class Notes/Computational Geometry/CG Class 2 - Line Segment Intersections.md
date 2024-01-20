---
tags:
  - computational-geometry
---

Intersection of 2 lines on a plane: If they have the same sloper they do not intersect. Otherwise they do.
Intersection of two lines $y=m_1x+b_1$ and $y=m_2x+b_2$ can be found with $x=\dfrac{b_2-b_1}{m_1-m_2}$
#### REMEMBER: compare floats like this -> (`fabs(m1-m2) < epsilon`)
Note, we cant represent mx + b for a vertical line

### Parametric Equation for line segment
$$L=\begin{bmatrix}x_1\\ y_1\end{bmatrix} + t\begin{bmatrix}x_2-x_1\\ y_2-y_1\end{bmatrix}$$
Vertical lines are not a problem
Can check if lines are parallel
For segments, check if t/u < 0  or > 1

To find intersections:
$$L_1=\begin{bmatrix}x_1\\ y_1\end{bmatrix} + t\begin{bmatrix}x_2-x_1\\ y_2-y_1\end{bmatrix},\qquad L_2=\begin{bmatrix}x_3\\ y_3\end{bmatrix} + t\begin{bmatrix}x_4-x_3\\ y_4-y_3\end{bmatrix}$$
$$t=\frac{\begin{vmatrix}x_1-x_3 & x_3-x_4\\ y_1-y_3 & y_3-y_4\end{vmatrix}}{\begin{vmatrix}x_1-x_2 & x_3-x_4\\ y_1-y_2 & y_3-y_4\end{vmatrix}}=\frac{(x_1-x_3)(y_3-y_4)-(y_1-y_3)(x_3-x_4)}{(x_1-x_2)(y_3-y_4)-(y_1-y_2)(x_3-x_4)}$$
$$u=\frac{\begin{vmatrix}x_1-x_3 & x_1-x_2\\ y_1-y_3 & y_1-y_2\end{vmatrix}}{\begin{vmatrix}x_1-x_2 & x_3-x_4\\ y_1-y_2 & y_3-y_4\end{vmatrix}}=\frac{(x_1-x_3)(y_1-y_2)-(y_1-y_3)(x_1-x_2)}{(x_1-x_2)(y_3-y_4)-(y_1-y_2)(x_3-x_4)}$$
# Line segment intersection algorithms
## Brute Force solution
iterate over every pair of line segments and check if they intersect.

This is $O(n^2)$. Checking if two lines intersect is $O(1)$

### Definition: Output Sensitive
When algorithm running time depends on the size of the output for the specific input.
See the gift-wrapping algorithm from [[CG Class 1 - Convex Hulls#Gift wrapping algorithm|Lecture 1]]

For worst case with many intersections, $O(n^2)$ is the best we can do. 
The worst case for $n$ line segments is $\dfrac{n(n-1)}{2}$.

## Line-Sweep/Plane-Sweep Algorithm
Incrementally focus on a subset of the data at a atime
Sweep line will move from top to bottom across our dataset.
![[Plane Sweep Algo.png]]

We only look for intersections between green segments. Red lines and blue lines will never cross so we never need to check them.

Handle Events in Event Queue
We can pre-schedule events by sorting by y-coordinates of input line segments. (Line segment added and line segment remove from active set)
![[Plane Sweep Events.png]]

We don't know when segments will intersect though.
![[Plane Sweep Adjacency List.png]]

We can just check in neighboring active segments intersect. Maintain the segments ordered by the x-position of intersection with the current sweep line.

When a segment is added, the newly adjacent segments are checked for intersection
When a segment is removed, the newly adjacent segments are checked for intersection
When the sweep line reaches an intersection, swap the positions in the horizontal ordering and check the newly adjacent segments for intersections

If an intersection is in the past, do nothing.
If an intersection is in the future, add to Event Queue if not already in there

------------------
## Data Structure
Data structures to consider:
* Array
* Linked List
* Priority Queue
* Binary Search Tree
* Hash Table

What data structure do we use for the Event Queue?
>Events are sorted by y-coordinate. 
>We remove one at a time and insert events when detected
>Need to check for existence before adding a duplicate

Solution: Priority Queue or Binary Search Tree

What data structure do we use for the Active Segment Status Structure?
>Initially start with an empty set. 
>Segments are added, removed, and swapped
>Adjacent neighbors are queried often.

Solution: Binary Search Tree

## Analysis

$n$ = # of input segments
$k$ = # of output intersections W.C. $k\approx n^2$
$s$ = max # of items on sweep line / in status structure at one time $s\leq n$

### Running Time

Step 1: Create add segment and remove segment events, sort, and initialize the Event Queue.
Need to add 2n segments. BST and PQ are selfsorting, so multiply 2n by insertion time
$\implies O(n\log n)\ /\ O(n)$ for BST/PQ

Step 2: For each entry in Event Queue, $\implies O(n+k)$
* Update Active Segment Status Structure $\implies O(\log s)$
* Compute intersections between newly adjacent segments $\implies O(1)$
* Add new intersections to the Event Queue $\implies O(\log(n+k))\rightarrow O(\log(n+n^2))\rightarrow O(\log(n))$

Total: $O(n\log n + (n+k)(\log(n)))\rightarrow O((n+k)(\log(n)))$
Lower bound: $\Omega(n\log n+k)$


### Memory
Step 1: Create add segment and remove segment events, sort, and initialize the Event Queue.
In place sorting, $\implies O(1)$

Step 2: For each entry in Event Queue (max size $O(n+k)$)
* Update Active Segment Status Structure (max size $O(s)$)

Total: $O(n+k)$ extra memory
Better: don't store future intersections of non-adjacent segments $O(n)$ extra memory

## Corner Cases / Degeneracies
* Overlapping lines (> 3 points on a line)
	* Solution: disallow in input
* More than 2 intersections at a point
	* Solution: Check any adjacents to the intersections
* Intersection at the endpoint
	* Solution: Process intersections before removals
* Lines parallel to sweep line
	* Solution: Prefilter
* 2 or more simultaneous events
	* Solution: Program better
* floating point :(
	* Solution: cry :(