---
tags:
  - graph-theory
---
## Automorphism
An automorphism is an isomorphic mapping of some $G$ to itself (s.t. the edges are preserved)
i.e. For $K_n$ we can map any vertex to any other vertex

## Complement
The complement of a simple graph $G$, denoted by $\overline{G}$, has the following vertex and edge sets:
$V(\overline{G}) = V(G)$
$E(\overline{G}) = \{\forall u,v \in V(G): (u,v)\not\in E(G)\}$

# Walks, Trails, and Paths
Time to take a stroll!

## Walk
A walk is a list of vertices and edges s.t. each listing is adjacent/incident to the listing preceding and proceeding

## Trail
A trail is a walk where edges do not repeat

## Path
A path is a trail where vertices do not repeat

$W:\{a,e_1,b,e_3,c,e_3,b,e_3,c,e_4,d,e_6,d,e_7,e\}$ is equivalent to
$W:\{e_1,e_3,e_3,e_3,e_4,e_6,e_7\}$
$T:\{e_1,e_3,e_4,e_6,e_7\}$
$P:\{e_1,e_3,e_4,e_7\}$
The above is an $a,e$-path

A walk/trail/path that starts and ends at the same vertex is closed. Otherwise it is open.
(A closed path is a cycle)

### Length
The length of a walk/trail/path is the number of edges traversed

### Hop
A hop is the traversal of a single edge

![[GT Class 1#Connected]]

## Connected Component
A connected component is a maximal connected subgraph of $G$

### Maximal vs Maximum
Maximal means it cannot be made larger
Maximum means the largest possible
Same goes for minimal vs minimum

## Cut Vertex
A cut vertex is some $v\in V(G)$ s.t. $G-v$ has more connected components than $G$

## Cut Edge
A cut edge is some $e\in E(G)$ s.t. $G-e$ has more components than $G$

# Proofs
## Weak Induction
With weak induction, we prove the basis $P(1)$, then to prove the inductive step $P(n=k+1)$, we assume what we're trying to prove holds for $P(k)$

## Strong Induction
With strong induction, the only difference is we assume what we're trying to prove holds $\forall k, 1\leq k < n$

## Example Proof
Show every closed odd walk contains an odd cycle
We use strong induction on the length of the walk

Basis: $P(1):\ V(G)=\{a\},\ E(G)=\{e_1\},\ e_1=(a,a)$
Inductive step: $P(n>k\geq1)$ Prove that $P(n)$, which is of length $n$, odd, closed
Case 1: no vertices repeat on our walk => trivial case, our odd walk is just a cycle
Case 2: at least one vertex $v$ repeats on our walk $w$. We can then split our walk into $w_1$ and $w_2$, the part before $v$ and after $v$, $|w| = |w_1| + |w_2|$. Since $w$ is odd, this implies wlog $|w_1|$ is also odd. Since $|w_1| < |w|$, by our induction hypothesis we know $P(w_1)$ contains an odd cycle. Adding $w_2$ to $w_1$ does not delete a cycle, so $w$ retains the odd cycle from $w_1$ $\blacksquare$ 