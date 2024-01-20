---
tags:
  - graph-theory
---

# Graph Properties
## Property
A property is a characteristic that some graph holds
Consider $K_3$, it has the following properties:
- $|V(K_3)| = |E(K_3)| = 3$
- $K_3$ is cyclic
- $\forall\ v\in V(K_3): d(v) = 2$ AKA $K_3$ is 2-regular

![[GT Class 1#Graph Class]]

## Graph Isomorphism
Graph $G=\{V,E\}$ is isomorphic to $G'=\{V',E'\}$ if there exists a bijective mapping using bijective function f ($f:V\mapsto V'$)
$\forall\ v \in V \mapsto f(v) = u \in V'$
$\forall\ e = (a,b) \in E \mapsto (f(a), f(b)) = h \in E'$
Isomorphism is:
- reflexive ($G\cong G'$)
- symmetric ($G\cong H \implies H\cong G$)
- transitive ($G\cong H$ and $H \cong J \implies G\cong J$)

### Bijection
A functional mapping $f:X\mapsto Y$ is bijective iff:
1. It is 1-to-1 (injective)
	$\forall\ x\in X:\exists$ exactly one $f(x) = y \in Y$ 
2. It is onto (surjective) 
	$\exists$ exactly one $g(y) = x\in X$, where $g:Y\mapsto X$

If $G\cong G'$:
	$|V(G)| = |V(G')|$
	$|E(G)| = |E(G')|$
	Sorted degree sequences are equivalent
	The [[GT Class 2#Diameter|diameters]] will be equal
	The [[GT Class 2#Girth|girths]] will be equal
	Constituent [[GT Class 1#Subgraph|subgraphs]] are identical
	Likewise a decomposition on $G$ also exists on $G'$

### Diameter
The diameter of a graph is the length of the longest shortest path.
To find the diameter, you can perform a BFS on any vertex, take the farthest vertex and perform BFS again, the longest distance is your longest shortest path

### Girth
The girth of a graph is the length of the shortest cycle

### Decomposition 
A decomposition on some graph $G$ is separating $G$ into edge-disjoint subgraphs, using all edges

NOTE: The properties $P$ are all necessary but not sufficient for isomorphism

In general: 
To show an isomorphic relationship doesn't hold, one just needs to show some property differs between the two graphs
To show an isomorphic relationship holds, need to find and validate the bijective mapping

# Subgraph Isomorphism
Determining whether some graph $H$ with order no greater than graph $G$ is a subgraph of $G$

## Induced Subgraph
An induced subgraph $H$ of a graph $G$ is a subgraph of $G$ where for all vertices which are present in $H$, the edges which are present in $G$ are present in $H$

Consider some vertex set of $H$ and a mapped vertex set of $G$, all edges among vertices in $G$ are present in $H$
