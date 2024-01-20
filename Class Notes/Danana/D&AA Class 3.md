---
tags:
  - design-and-analysis-of-algorithms
---
[[D&AA Class 2#Exchange Argument|See last lecture for Exchange Argument]]

# Minimum Spanning Trees
Given: graph $G=(V,E)$ with edge costs $c_e\geq 0$ 
Goal: cheapest set of edges s.t. all of $V$ is connected
(Note: it being a tree is sort of defaulted in, we don't gain connectedness via cycles)

## Kruskal's Algorithm
1. Order edges by increasing cost
2. Starting from the first edge, add to the tree as long as it does not create a cycle

## Prim's Algorithm
1. Start with a root node
2. Add cheapest edge from $S$ (nodes which are connected so far) to $V\setminus S$

## Reverse-Delete
1. Start with all edges in the graph
2. Starting with the most expensive edge, remove it unless it would disconnect the graph

## Cut Property
If edge $e$ is min-cost in a cut $(S, V\setminus S)$, then $e$ is in every MST.
(Note: $S$ is not necessarily connected)

### Proof for the Cut Property
Take some MST $T^*$. Suppose to the contrary that $e\not\in T^*$.

Consider $T^* + e$. This graph has one cycle. Let $f$ be the other edge in this cycle which crosses the cut. We know that $e$ is the min-cost edge across the cut, so $c_e\leq c_f$. Thus, by replacing $f$ with $e$, we end up with a new set of nodes ($T = T^*+e-f$) with a lower cost than $T^*$. We can see that $T$ is an MST since $T*+e$ had one cycle, and $T$ removes the other edge in that cycle. Thus the graph remains connected and $T$ is an MST. Since $T*$ has a higher cost than $T$, it must not be an MST, so this is a contradiction!

## Proof for Kruskal's Algorithm
At every step, we are adding the next-cheapest edge that does not create a cycle. This edge connects $S$ to $V\setminus S$. If there were a cheaper edge across the cut, then we would've added it first since we sort by edge weights
$O(|E|\log |V|)$

## Proof for Prim's Algorithm
At every step, we are adding the cheapest edge from $S$ to $V\setminus S$. By definition, this edge is the min-cost edge across the cut, so it must be in the spanning tree
$O(|E|\log |V|)$

## Cycle Property
$C$ is a cycle, $e$ is the most expensive edge in cycle. Then, $e$ does not belong to any MST.

### Proof for Cycle Property
Let $T^*$ be an MST. Suppose to the contrary that $e\in T^*$

Consider $T^*-e+f$, where $f$ is another edge across the cut. This is still a spanning tree, since $f$ still connects the graph. Since $e$ is the heaviest edge across the cut, $c_e\geq c_f$, so $T^*-e+f$ has a lower cost than $T^*$. Thus $T^*$ cannot be an MST, which is a contradiction.

## Proof for Reverse-Delete Algorithm
At each step, there are two cases:
Case 1: this edge is not part of a cycle, then obviously it is necessary for the MST
Case 2: this edge is part of a cycle. Since this is the heaviest edge in the cycle, by the cycle property we do not include it