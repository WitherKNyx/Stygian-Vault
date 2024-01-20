---
tags:
  - design-and-analysis-of-algorithms
---
# Dynamic Programming
## Weighted Interval Scheduling
```
1 |-----------| 2
2    |--------------| 4
3              |--------| 4
4      |-------------------| 7
5                       |----| 2
6                         |----| 1
```

Given: Intervals $1\dots n$
Interval $i$ has $s_i$ (start time), $f_i$ (finish time), and $w_i$ (weight)

Goal: Compute largest weight set of disjoint intervals

Sort by $f_i$
$f_1 \leq f_2 \leq \dots \leq f_n$

### Simple Observation
Take some $OPT$ from the set of optimal solutions
Looking at the last interval $n$, either $OPT$ contains $n$ or not
Consider the best solution which contains $n$ and the best solution which does not contain $n$
$OPT$ is the better of the two solutions

$p(j)$ = last interval $i<j$ s.t. $i\ \&\ j$ are disjoint
$OPT(j) =$ value of optimal solution using only the first $j$ intervals
$O(j) =$ the solution

Rewriting our definition of $O(n)$
$$O(n) = \text{best }\Biggl\{ \begin{array}{l}O(p(n))\ \bigcup\ \{n\} \\ O(n-1)\end{array} \Biggr.$$
$$OPT(j) = \max(OPT(p(j)) + w_j, OPT(j-1))$$
w/ $O(1) = {1}$ and $OPT(1) = w_1$

```
s = {start times}
f = {finish times}
w = {weights}
p(j):
	for i = j - 1 to i = 0, do:
		if f_j >= s_i:
			return i
	return 0
OPT(j):
	If j = 0: return 0
	else
		max(w_j + OPT(p(j)), OPT(j-1))
O(j):
	if j = 0: return {}
	else:
		if w_j + OPT(p(j)) > OPT(j-1):
			return {j} U O(p(j))
		else:
			return O(j-1)
```

### Proof of Correctness for Recursive WIS
We prove by strong induction that $OPT(j)$ is correct
Base Case: $OPT(0)$ = 0, which is correct
Induction Step: Assuming $\forall k, 0\leq k < j$ are correct,
We know by our inductive hypothesis, $OPT(j-1)$ and $OPT(p(j)$ are correct. $OPT(j)$ then picks the better of these two solutions, which is the correct solution
Thus, $OPT(j)$ is correct

### Runtime Analysis for Recursive WIS
At each recursive step, we create 2 subproblems to solve. Assuming in the worst case that all intervals are part of our solution, we compute $j-1$ twice per recursive step. Recombining the subproblems is a simple comparison, so $O(1)$ work
Thus, the runtime of this algorithm is $O(2^n)$
Easy way to prove:
Fibonacci: $T(n) = T(n-1) + T(n-2)$
Recursive WIS: $T(n) = \max(T(n-1), T(n-2))$

## How To Improve
Instead of calculating each $OPT(n)$ each time in the algorithm, we can memoize the results

### Memoization
Store the results when you compute them

Related to memoization is the technique of Dynamic Programming (its still memoization)

## DP WIS
```
Create an array of size $n+1$ 

OPT = [ ][ ][ ][ ][ ][ ][ ]
       0  1  2  3  4  5  6

for j=1 to n:
	OPT[j] = max(OPT[p(j)] + w_j, OPT[j-1])
return OPT[n]
```

This runs in quadratic time, since we are just doing a for loop over a $O(n)$ operation, $p(j)$
$O(n^2), n =$ # of intervals
We can simplify to $O(n\log n)$ by using binary search for $p(j)$
This makes sense, since we need to sort the jobs by $f_i$ to begin with

If we want an algorithm to get the set, not just the optimum time:
Starting from $n$, we compare $OPT[n]$ to $OPT[n-1]$. If they are different, then $n$ is in your solution. Set $n=p(n)$ and repeat. Otherwise, decrement $n$ by 1