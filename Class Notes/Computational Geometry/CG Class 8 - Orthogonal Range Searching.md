---
tags:
  - computational-geometry
---
## Motivations
### 2D Database Queries
Return all data points with $x_0\leq x\leq x_1$ and $y_0\leq y\leq y_1$ AKA orthogonal range query
### Higher Dimensional Database Queries
### Photon Mapping
Photons bounce around a room and stored on each surface they hit
Find the tightest sphere capturing $k$ photons
Divide the energy from those photons by that sphere
What is the best data structure to store millions of photons

# Data Structure Choice
Quad tree from DS?
We need to query around a point and make a orthogonal query to the $k$d tree
The $k$d tree returns all cells that overlap with query box
Further processing necessary to filter points inside the red circle and find smallest circle capturing exactly $k$ photons

## 1D Binary Search Tree Review
Everything to the left is less than the root
Everything to the right is greater than the root
We'll assume no 2 data points have the same value in any dimension and that we're given all of the data at the start

### 1D BST Construction Algorithm
1. Sort the data by $x$-value $\implies O(n\log n)$
2. Put the median value at the root $\implies O(1)$
3. Create 2 sublists for left & right $\implies O(n)$
4. Recurse on steps 2 & 3 $\implies O(\log n)$
Overall: $O(n\log n)$

### 1D BST Query Agorithm
Given a desired range $[\mu,\mu']$
Locate the leaf storing $\mu$ and the leaf $\mu'$ $\implies 2*O(\log n)$
Increment from $\mu\rightarrow\mu'$ $\implies O(k)$ expected

### Analysis: 1D BST
Memory to store: $O(n)$
Time to construct: $O(n\log n)$
Time to query: $O(\log n + k)$

# 2D $k$D Trees & Higher Dimension $k$D Trees
Multidimensional Binary Search Tree
## 2D $k$D Tree Construction
Make 2 sorted lists, by $x$-value and by $y$-value
Alternate dimensions (split by $x$ then by $y$)
Find the median value
Make a copy of the sorted lists, omitting values from the other side
## 2D $k$D Tree Query Algorithm
At each split point
	Determine if the query box overlaps the split line
	Recurse down one or both branches
	If a subtree lies completely inside the box, return all items in that subtree
	Perform filtering in the leaves as necessary
### Analysis
1 item is stored per leaf node
For a query that will collect $k$ items
Best/Average Case: an approximately square query
* Overlaps $O(k$) leaves
* Gathering leaves $\implies O(\log n + k)$
Worst Case: skinny/lopsided query box
* Overlaps $O(\sqrt{n} + k)$ leaves
* Gathering leaves $\implies O(\sqrt{n} + k)$

## Analysis: 2D $k$D Tree
Memory to store: $O(n)$
Time to construct: $O(n\log n)$
Time to query: $O(\sqrt{n} + k)$ worst case

For higher dimensions ($d$) it becomes $O(n^{1-\frac{1}{d}})$, approaching $O(n)$ query time

# 2D Range Trees & Higher Dimension Range Trees
Idea: If we use more memory, can we reduce worst case query time of $k$D tree?
First, organize the data in a BST by $x$-value
At every node in the tree, store a pointer to a BST with the same data, organized by $y$-value

## 2D Range Tree Construction
Each point $p$ is stored once in the level 1 tree
And many times in the level 2 tree
* 1 tree with $n$ values
* 2 trees with $n/2$ values
* ...
* $n$ trees with 1 value
$O(n)$ memory for level 1 tree
$O(n\log n)$ memory in total for all the level 2 tree

## 2D Range Tree Query Algorithm
Search through level 1 tree for all intermediate nodes that fit completely inside the query's $x$-range
For each of those nodes:
	Search through the corresponding level 2 trees for all nodes and leaves that fit completely inside the query's $y$ range

## Analysis: 2D Range Tree
Memory to store: $O(n\log n)$
Time to construct: $O(n\log n)$
Time to query: $O(\log^2n + k)$

## Analysis: Higher Dimensional Range Trees
Memory to store: $O(n\log^{d-1}n)$
Time to construct: $O(n\log^{d-1} n)$
Time to query: $O(\log^dn + k)$

Using more memory gives us a faster runtime!