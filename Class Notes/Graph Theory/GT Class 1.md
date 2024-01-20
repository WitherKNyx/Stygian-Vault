---
tags:
  - graph-theory
---

```
 O
/|\  <--- Euler :)
 A
```

Bridges of Konigsberg is an early example of Graph Theory.
This is the inspiration for the eulerian circuit AKA a Euler Circuit or a Euler Path

## Euler Circuit
A Eulerian Circuit on a graph $G$ is a path that goes through every edge in $G(E)$, that returns to the original starting location.

# Basic Definitions
## Graph
A graph is a tuple of vertices and edges.
$G = \{V(G),\ E(G)\}$, or $G = \{V,\ E\}$ in simpler cases.

## Edge
An edge in $G$ has two endpoints (vertices).
Some edges have weights, and they can be either directed or undirected.

## Multi-edge
Multi-edges are edges which share the same endpoints.
$e_1 = e_2 = (u,v)$

## Loops
A loop is an edge which has a duplicate endpoint.
$e = (u,u)$

## Incident
A vertex $u$ is incident to an edge $e$ if $u$ is one of the endpoints of $e$.

## Adjacent
A vertex $u$ is adjacent to a vertex $v$ if there is an edge $e = (u,v)$ connecting them.
We say $v$ is in the neighborhood of $u$ and vice versa.

## k-Hop Neighborhood
A k-hop neighborhood is the vertices you can get to from a vertex $u$ by travelling at most $k$ edges.

## Degree
The degree of a vertex $u$ is the number of vertices adjacent to it. In other words, the size of its neighborhood.

## Order
The order of a graph is the number of vertices in $G$.
$|V(G)|$

## Size
The size of a graph is the number of edges in $G$.
$|E(G)|$

## Connected
A graph is connected if any vertex $u$ can be reached by any other vertex $v$ by some series of edges.
$\forall\ u,v \in V(G): \exists\ u,v$-path

## Disjoint
A graph is disjoint if the graph is empty.

# Graph Classes
## Graph Class
A graph class is the set of all possible graphs having some property.

## Simple Graph
A simple graph is a graph $G$ that contains no multi-edges or loops.

## Loopy Graph
A loopy graph is a graph $G$ that contains loops.

## Multi-Graph
A multi-graph is a graph $G$ that contains multi-edges

## Loopy Multi-Graph
A loopy multi-graph is a graph $G$ which is both a loopy graph and a multi-graph.

## Null Graph
A null graph is a graph $G$ where $V(G) = E(G) = \emptyset$.

## Trivial Graph
A trivial graph is a graph $G$ where $|V(G)| = 1$ and $E(G) = \emptyset$.

## Empty Graph
An empty graph is a graph $G$ where $|V(G)| \geq 1$ and $E(G) = \emptyset$.

# Basic Graphs
## Path
A path is a simple graph $G$ where the vertices can be listed in an order where two vertices $u,v$ are adjacent iff they are consecutive in the list. Denoted as $P_n$, where $n = |V(G)|$.

## Cycle
A cycle is a simple graph $G$ where the vertices can be placed on the boundary of a circle where two vertices $u,v$ are adjacent iff they are consecutive on the circle. Denoted as $C_n$, where $n = |V(G)|$. $C_1,C_2$ are degenerate cases.

## Star 
A star is simple graph $G$ where the vertices can be arranged as one vertex in the center, and all other vertices in a circle around the middle vertex. The only edges go from outer vertices to the center vertices. Denoted as $S_n$, where $n = |V(G)|$. $S_1, S_2, S_3$ are degenerate cases.

## Tree
A tree is a simple connected graph $G$ with no cycles (acyclic).

## Complete Graph
A complete graph, or a clique, is a graph $G$ where every vertex is adjacent to every other vertex. Denoted as $K_n$, where $n = |V(G)|$.
$\forall\ u,v \in V(G): \exists\ e=(u,v)$

## Bipartite Graph
A bipartite graph is the union of two disjoint independent (meaning no two vertices within a set are connected) sets.

## Complete Bipartite Graph
A complete bipartite graph, or a biclique, is a bipartite graph $G$ where each vertex in one set has a connection to every vertex in the other set. Denoted as $K_n,m$, where $n = |S|,\ m = |T|,\ |V(G)| = n + m$

## Subgraph
A subgraph of a graph $G$ is a graph $H$ which is contained within $G$. AKA, $H \subseteq G$, $V(H) \subseteq V(G)$ , $E(H) \subseteq E(G)$  

# Graph Storage Formats
## Adjacency Matrix
An adjacency matrix of a graph $G$, denoted as $A(G)$, is an $n\times n$ matrix ($n = |V(G)|$) where a non-zero value at $a_{ij}$ denotes that many edges from $v_i$ to $v_j$.
The sum of non-zeros in a row $i$ is equal to the degree of $v_i$.

## Incidence Matrix
An incidence matrix of a graph $G$, denoted as $M(G)$, is an $n\times m$ matrix 
($n = |V(G)|,\ m = |E(G)|$) in which a value of 1 at $m_{ij}$ indicates that $v_i$ is incident on edge $e_j$. The sum of non-zeros in a row $i$ is equal to the degree of $v_i$.

## Adjacency List
An adjacency list of a graph $G$ is a list of length $n\ (n = |V(G)|)$, where each entry corresponds to a vertex $v_i$. Each entry contains a list containing the edges originating from $v_i$.

## Compressed Sparse Row (CSR)
A CSR of a graph $G$ uses two arrays. $L$ is an array of length $2|E(G)|$ which stores the adjacencies of $v_1$, then $v_2$, up to $v_n$. $O$ is an array of length $|V(G)| + 1$ lists the offsets for where each vertex $v_i$'s adjacencies begins. The degree for $v_i$ is $O[i+1] - O[i]$.