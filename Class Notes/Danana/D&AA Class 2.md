---
tags:
  - design-and-analysis-of-algorithms
---
# Greedy Algorithms & Exchange Argument
smthn smthn TSP

Exchange Argument is a proof technique woooo

## Scheduling Problem
We're given $n$ jobs, and we want to schedule them so we complete them within their given deadlines.

Given: $n$ jobs, Job $i$ has runtime $t_i$ and deadline $d_i$

Goal: Form Schedule to minimize max lateness $L = max l_i$

### Lateness
$l_i = f(i) - d_i$, where $f(i)$ is the finish time of job $i$ in our schedule

Example:

| $i$     | Job 1 | Job 2 | Job 3 |
| ----- | ----- | ----- | ----- |
| $t_i$ | 1     | 2     | 3     |
| $d_i$ | 2     | 4     | 6      |
Solution: Schedule Job 1, then 2, then 3

| $i$ | 1 | 2 | 3 | 4 | 5 | 6 |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| $t_i$ | 3 | 2 | 1 | 4 | 3 | 2 |
| $d_i$ | 6 | 8 | 9 | 9 | 14 | 15 |
| $s_i$ | 3 | 6 | 8 | 5 | 11 | 13 |

Let's try just doing the quickest jobs first (Shortest Job First AKA SJF)

3 ,2, 6, 1, 5, 4
$f(3) = 1$, on-time
$f(2) = 3$, on-time
$f(6) = 5$, on-time
$f(1) = 8$, $l_1 = 8 - 6 = 2$
$f(5) = 11$, on-time
$f(4) = 15$ $l_4 = 15 - 9 = 6$
$L_{max} = 6$

How about in order of deadlines, then by SJF? (Earliest Deadline First AKA EDF)

1, 2, 3, 4, 5, 6
$f(1) = 3$, on-time
$f(2) = 5$, on-time
$f(3) = 6$, on-time
$f(4) = 10$, $l_1 = 10 - 9 = 1$
$f(5) = 13$, on-time
$f(4) = 15$, on-time
$L_{max} = 1$

This turned out better! But is it good in general?

Last algo to try: In order of "slack-time" $s_i = d_i - t_i$ (Shortest Slack First AKA SSF)
1, 4, 2, 3, 5, 6
$f(1) = 3$, on-time$f(1) = 3$, on-time
$f(4) = 7$, on-time
$f(2) = 9$, $l_2 = 9 - 8 = 1$
$f(3) = 10$, $l_3 = 10 - 9 = 1$
$f(5) = 13$, on-time
$f(6) = 15$, on-time
$L_{max} = 1$

Well which ones can be broken?
SJF vs EDF vs SSF

| $i$   | 1   | 2   | 3   | 4   |
| ----- | --- | --- | --- | --- |
| $t_i$ | 1   | 1   | 1   | 10  |
| $d_i$ | 20  | 20  | 20  | 10  | 
In this example, SJF would choose the sequence $1,2,3,4$, which would make $L_{max}=3$, compared to the sequence $4,1,2,3$, which would have $L_{max}=0$
`hehe got my example chose :D`

| $i$ | 1 | 2 | 3 |
| ---- | ---- | ---- | ---- |
| $t_i$ | 100 | 1 | 1 |
| $d_i$ | 100 | 2 | 2 |
| $s_i$ | 0 | 1 | 1 |

In this case, SSF would choose the sequence $1,2,3$, which would make $L_{max} = 100$, compared to the sequence $2,3,1$, which would make $L_{max}=2$

In then end, EDF is the best greedy algorithm for this problem. But, how do we prove that this is the best??

Let's first rename jobs:
$d_1 \leq d_2 \leq \dots d_n$

## Claim about $L_{max}$
Max lateness stays the same no matter how we order the jobs with the same deadline

Let's prove this:

Say we have $n$ jobs with deadline $d$.
| 1 | 2 | 3 | ... | n |
No matter the order, the one at the end will have lateness $l_i - d$, so changing the order wont affect the lateness.
Anything before or after is unaffected since they will still complete at the same times regardless.

# Exchange Argument
Consider an optimal solution which is most similar to the greedy solution (our solution)
There are 2 cases:
### Case 1: OPT = Greedy
In this trivial case, our solution is optimal, and we don't need to do anything to show it's optimal.

### Case 2: OPT $\leq$ Greedy \[IMPOSSIBLE]
Try to change OPT to be:
- more like Greedy
- without making it worse
We chose OPT to be the most similar to Greedy, so we cannot make it more like greedy without making it worse.
This is a contradiction! So this case is impossible!

## Inversion
An inversion is 2 jobs s.t. $i$ is before $j$ in schedule, but $d_j < d_i$

## Lemma about Inversions
If an inversion exists, then there exists a pair of adjacent jobs which are inverted.
let's say we have sequence $i,k_1,k_2,\dots k_m,j$ 
Either $i,k_1$ | $k_i,k_{i+1}$ | $k_m,j$ will be out of order 

Consider an optimal schedule which has fewest inversions from our greedy solution.

## Case 1: OPT has 0 inversions.
Then we are done

## Case 2: OPT has $n > 0$ inversions
We claim that swapping adjacent inverted jobs in OPT does not increase $L_{max}$
and we need to prove this is a contradiction

This is the basis of the Exchange Solution

Let's actually prove this for our scheduling problem

Let's say we have job $j$ followed by job $i$, where $d_i < d_j$. $j$ starts at time $F$ and $i$ finishes at $F_i$.
Before:
$l_i = F + t_j + t_i - d_i$ and $l_j = F + t_j - d_j$
After swapping $i$ and $j$:
$l_i = F + t_i - d_i$         and $l_j = F + t_i + t_j - d_j$
We see that old $l_i$ and new $l_j$ are equal except for $d_i$ and $d_j$, and because we already know $d_i < d_j$, we then get:
new $l_j = F + t_j + t_i - d_j < F + t_i + t_j - d_i =$ old $l_i \leq L_{max}$

