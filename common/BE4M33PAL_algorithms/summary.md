# BE4M33PAL — Algorithms (PAL) — Study Summary

**Lecturer / materials:** Daniel Průša, Ondřej Drbohlav, Tomáš Mařík, Jiří Vyskočil, Petr Felkel et al., CTU FEE (course codes A4M33PAL → **BE4M33PAL**, *Pokročilá algoritmizace*).
**Language:** English-primary (slides are English-content); key Czech terms in parentheses *(kurzívou)* for the oral exam.
**Sources:** lecture decks in `materials/` (cited as *[pal01]*, *[mst3]*, *[pal04]*, *[2011pal02]*, *[2012pal03]*, *[isom]*, *[combi]*, *[pal06]*, *[pal07_2015]*, *[paska08b]*, *[paska09]*, *[pal09]*, *[paska11a/b]*, *[paska12b]*, *[paska13]*, *[paska13trie]*, *[automata101]*). Cross-checked against CLRS (Cormen et al.), Melichar–Holub–Polcár *Text Searching Algorithms*, Kreher–Stinson *Combinatorial Algorithms*, and the course page <https://cw.fel.cvut.cz/wiki/courses/be4m33pal/start>.

**Structure** mirrors the 5 official exam topics (see `../../00_subjects_overview.md`):

1. [Asymptotic complexity; graph basics, representations & matrices; traversals](#1-asymptotic-complexity-graph-basics-and-representations)
2. [MST, strongly connected components, Euler trail, Union-Find, isomorphism](#2-mst-scc-euler-trail-union-find-isomorphism)
3. [Combinatorial generation, Gray codes, primes, pseudorandom numbers](#3-combinatorial-generation-gray-codes-primes-prng)
4. [Search trees](#4-search-trees)
5. [Finite automata and text search](#5-finite-automata-and-text-search)

Plus: [Heaps (priority queues)](#appendix-heaps-priority-queues) · [Key results cheat-sheet](#key-results-cheat-sheet) · [References](#references).

⭐ = exam emphasis. Complexities are taken from the slides and cross-checked against standard sources.

---

## 1. Asymptotic complexity, graph basics and representations

### 1.1 Asymptotic growth ⭐ (*[pal01]*)

For non-negative $f,g:\mathbb{N}\to\mathbb{R}_{\ge0}$:

| Notation | Definition | Meaning |
|---|---|---|
| $f\in O(g)$ | $\exists c>0,n_0:\ f(n)\le c\,g(n)\ \forall n>n_0$ | upper bound (*horní odhad*) |
| $f\in\Omega(g)$ | $\exists c>0,n_0:\ c\,g(n)\le f(n)\ \forall n>n_0$ | lower bound (*dolní odhad*) |
| $f\in\Theta(g)$ | $f\in O(g)\cap\Omega(g)$ | tight bound (*optimální odhad*) |

**Multi-variable** $O$: $\exists c>0,n_0:\ f(n_1,\dots,n_k)\le c\,g(n_1,\dots,n_k)$ whenever every $n_i>n_0$.

**Limit rule.** With $f,g>0$: $\lim_{n\to\infty}f/g=0\Rightarrow f\in O(g)\setminus\Theta(g)$; $=a\in(0,\infty)\Rightarrow f\in\Theta(g)$; $=\infty\Rightarrow g\in O(f)\setminus\Theta(f)$.

**Useful facts:** $\log_a n\in\Theta(\log_b n)$ (change of base = constant factor); $(\log n)^k\in O(n)$ for fixed $k$ (L'Hôpital).

### 1.2 Graph terminology *(teorie grafů)* (*[pal01], [2011pal02]*)

- **Graph** $G=(V,E)$; **undirected** edge $\{u,v\}$, **directed** arc $(u,v)$. **Degree** $\deg(v)$; for digraphs **out-degree** $\delta^+(v)$, **in-degree** $\delta^-(v)$. Handshake: $\sum_v\deg(v)=2|E|$.
- **Walk** *(sled)* — edges/vertices may repeat; **trail** *(tah)* — no repeated **edge**; **path** *(cesta)* — no repeated **vertex**; **circuit/cycle** *(kružnice/cyklus)* — closed trail/path.
- **Subgraph** $H$: $V(H)\subseteq V(G)$, $E(H)\subseteq E(G)\cap\binom{V(H)}{2}$. **Spanning subgraph (factor)**: $V(H)=V(G)$. **Spanning tree** *(kostra)*: spanning subgraph that is a tree.
- **Connected component of $v$**: $C(v)=\{u\mid\exists$ path $u\!-\!v\}$. **DAG** *(acyklický orientovaný graf)*: digraph with no directed cycle ⇔ has a topological order.

### 1.3 ⭐ Graph representations and matrices

The course mainly uses two representations *([pal01])*; the official topic list also requires four **matrices** (standard definitions, verified against algebraic graph theory):

| Representation | Space | Edge test | Best for |
|---|---|---|---|
| **Adjacency matrix** $A$ *(matice sousednosti)* | $\Theta(\lvert V\rvert^2)$ | $O(1)$ | dense graphs |
| **Adjacency list** *(seznam sousedů)* | $\Theta(\lvert V\rvert+\lvert E\rvert)$ | $O(\deg)$ | sparse graphs; traversals |

- **Adjacency matrix** $A$: $A_{ij}=1$ iff $\{i,j\}\in E$ (or the edge weight). Symmetric for undirected graphs.
- **Distance matrix** *(matice vzdáleností)* $D^{\text{dist}}$: $D^{\text{dist}}_{ij}=$ length of a shortest $i\!-\!j$ path ($\infty$ if unreachable, $0$ on the diagonal). Produced e.g. by Floyd–Warshall (cf. KO §2.6).
- **Incidence matrix** *(matice incidence)* $B$ of size $\lvert V\rvert\times\lvert E\rvert$: column = edge. Unoriented: column of edge $\{i,j\}$ has two $1$s (rows $i,j$). **Oriented**: for arc $(i,j)$, $B_{ie}=-1$, $B_{je}=+1$, rest $0$. Then the (oriented) **Laplacian** factorizes as $L=BB^{\top}$.
- **Laplacian / Kirchhoff matrix** *(Laplaceova matice)* $L=D-A$, where $D=\mathrm{diag}(\deg v)$ is the degree matrix. Properties: symmetric PSD; row sums $0$ (so $\mathbf 1$ is an eigenvector with eigenvalue $0$); multiplicity of eigenvalue $0$ = number of connected components.
  - ⭐ **Kirchhoff's Matrix-Tree theorem:** the number of spanning trees of $G$ equals **any cofactor** of $L$ (equivalently $\tfrac1n\prod_{i=2}^{n}\lambda_i$, the product of nonzero Laplacian eigenvalues over $n$). Generalizes **Cayley's formula** $n^{\,n-2}$ for $K_n$.

### 1.4 ⭐ Traversals: DFS and BFS (*[pal01]*)

Both maintain per-vertex `state ∈ {FRESH/UNVISITED, OPEN, CLOSED}`, predecessor `p[]`; DFS additionally discovery/finish times `d[u]/f[u]`.

- **DFS** *(do hloubky)* — recursion/stack. On entering $u$ set `OPEN`, $d[u]$; recurse into FRESH neighbours; on exit set `CLOSED`, $f[u]$. **Space** $\Theta(h)$ where $h$ = current path length (so $\Theta(\log n)$ on a balanced tree, $\Theta(n)$ worst).
- **BFS** *(do šířky)* — FIFO queue; computes **edge-shortest paths** (fewest edges) and the distance layer of each vertex. **Space** $\Theta(\text{width})$ (e.g. $\Theta(n)$ on a complete/last layer).
- Both run in $\Theta(\lvert V\rvert+\lvert E\rvert)$ with adjacency lists, $O(\lvert V\rvert^2)$ with a matrix.

**Applications** *([pal01])*: connected components; **cycle detection** (DFS only — encountering an `OPEN` vertex ⇒ back edge ⇒ cycle); spanning tree/forest; **edge-shortest path** (BFS only); **topological order** (DFS only — vertices in **decreasing finish time** $f$).

### 1.5 DAG: topological order and longest path (*[pal01]*)

- **Topological order** *(topologické uspořádání)*: linear order of $V$ with every arc going forward. Exists iff $G$ is a DAG. Compute by DFS (decreasing $f$) in $\Theta(\lvert V\rvert+\lvert E\rvert)$, or by Kahn's repeated in-degree-0 removal.
- **Longest path in a DAG** *(nejdelší cesta)*: process vertices in topological order, DP $\ell(v)=\max_{(u,v)\in E}\big(\ell(u)+w(u,v)\big)$. $\Theta(\lvert V\rvert+\lvert E\rvert)$. (Longest path is NP-hard on general graphs — cf. TAL §3.7 — but linear on DAGs.)

---

## 2. MST, SCC, Euler trail, Union-Find, isomorphism

### 2.1 ⭐⭐ Minimum Spanning Tree *(minimální kostra)* (*[mst3], [2011pal02], [pal07_2015]*)

MST of weighted $G=(V,E,w)$ = spanning tree $K$ minimizing $\sum_{e\in K}w(e)$. All three algorithms are justified by one lemma.

**⭐ Cut property (lightest-edge lemma).** Let $F$ be a **cut** (the edges crossing a partition $U\mid V\setminus U$) and let $f$ be the **lightest** edge in $F$. If weights are distinct, **every** MST contains $f$.
*Proof (exchange):* if $f=(u,v)\notin K$, the tree path between $u,v$ in $K$ crosses the cut at some heavier $e$; $K'=K-e+f$ is a spanning tree of smaller weight — contradiction. The dual **cycle property**: the heaviest edge of any cycle is in no MST.

| Algorithm | Idea | Data structure | Complexity |
|---|---|---|---|
| **Jarník–Prim** *(Jarníkův)* | grow one tree from $v_0$; repeatedly add the lightest edge leaving it | array of best dist $D[v]$ / binary heap / Fibonacci heap | $O(\lvert V\rvert^2)$ (array); $O(\lvert E\rvert\log\lvert V\rvert)$ (binary heap); $O(\lvert E\rvert+\lvert V\rvert\log\lvert V\rvert)$ (Fib. heap) |
| **Kruskal** | sort edges ascending; add an edge iff it joins two different components | **Union-Find** | $O(\lvert E\rvert\log\lvert V\rvert)$ (sort dominates) |
| **Borůvka** *(Borůvkův/Sollin)* | each round, every component picks its cheapest outgoing edge; add all, merge | component labelling by DFS | $O(\lvert E\rvert\log\lvert V\rvert)$ |

- **Prim ≈ Dijkstra** *([mst3])*: identical loop; only the relaxation key differs — Prim uses `key = w(u,v)`, Dijkstra `key = u.dist + w(u,v)`.
- ⭐ **PQ trick** *([mst3])*: standard priority queues cannot *decrease-key* without a handle, so **insert a copy** of a vertex on each improvement and **skip already-CLOSED copies** when popped (`while(closed[v=pq.poll()]);`).
- **Borůvka** needs distinct weights; **terminates in $\le\lceil\log_2\lvert V\rvert\rceil$ rounds** because each surviving component at least doubles its size every round.
- ⭐ **Near-linear Kruskal** *([mst3])*: with union-by-rank + path compression the Union-Find work is $O(\lvert E\rvert\,\alpha(\lvert V\rvert))\approx O(\lvert E\rvert)$, so the bottleneck is the **edge sort**; if edges can be sorted in linear time (counting/radix/bucket) Kruskal is $\Theta(\lvert E\rvert)$.

### 2.2 ⭐⭐ Union-Find / disjoint sets *(problém Union-Find)* (*[2011pal02], [mst3]*)

Maintains a partition under `MakeSet`, **`Find(x)`** (return representative / *boss*), **`Union(rx,ry)`** (merge two roots). "Are $u,v$ in the same component?" ⇔ `Find(u)==Find(v)`.

| Implementation | Find | Union |
|---|---|---|
| representative array | $O(1)$ | $O(\lvert V\rvert)$ (relabel) |
| rooted tree, **union by size/rank** | $O(\log\lvert V\rvert)$ | $O(\log\lvert V\rvert)$ |
| **+ path compression** ⭐ | amortized $O(\alpha(\lvert V\rvert))$ | amortized $O(\alpha(\lvert V\rvert))$ |

- **Depth lemma:** a union-by-rank tree of depth $h$ has $\ge2^{h}$ nodes ⇒ $h\le\log_2\lvert V\rvert$.
- **Union by rank:** hang the lower-rank root under the higher; on a tie, increase the new root's rank by 1.
- **Path compression** *([mst3])*: `find(a){ if(boss[a]!=a) boss[a]=find(boss[a]); return boss[a]; }`.
- ⭐ With both heuristics, $m$ operations cost $O(m\,\alpha(n))$, where $\alpha$ is the **inverse Ackermann** function ($\alpha\le4$ for any conceivable input) — effectively constant.

### 2.3 ⭐ Strongly Connected Components *(silně souvislé komponenty)* (*[2012pal03], [pal04]*)

Digraph is **strongly connected** if every pair is mutually reachable; **SCC** = maximal such subgraph; $\mathrm{SCC}(v)=\{u\mid v\!\to\!u$ and $u\!\to\!v\}$.

- **Kosaraju–Sharir** — two passes: (1) DFS on $G$, pushing vertices onto a stack on **finish**; (2) DFS on the **transpose** $G^{\top}$, popping start vertices off the stack — each tree of the second forest is one SCC. **Two traversals**, $\Theta(\lvert V\rvert+\lvert E\rvert)$.
- **Tarjan** — single DFS with `index` (discovery number) and `lowlink` (lowest index reachable through the subtree + one back/cross edge into the on-stack part) and a vertex stack. A vertex with `lowlink==index` is an **SCC root**; pop the stack down to it. **One traversal**, $\Theta(\lvert V\rvert+\lvert E\rvert)$ — faster in practice (no transpose).

```
find_scc(v):                          # Tarjan
  v.index = v.lowlink = ++index; push(v); v.instack = true
  for w in succ(v):
    if w.index == 0: find_scc(w); v.lowlink = min(v.lowlink, w.lowlink)
    elif w.instack:  v.lowlink = min(v.lowlink, w.index)
  if v.lowlink == v.index:            # v is the root of an SCC
    repeat x = pop(); x.instack=false; add x to component until x == v
```

### 2.4 ⭐ Euler trail *(Eulerův tah)* (*[2012pal03]*)

An **Euler trail** uses **every edge exactly once**; closed ⇒ **Euler circuit/tour**. (Contrast: a *Hamiltonian* path visits every **vertex** once — NP-complete.)

- **Existence — undirected:** connected and **0 or 2 odd-degree vertices**. 0 odd ⇒ Euler circuit; 2 odd ⇒ open trail between the two odd vertices.
- **Existence — directed** (standard, beyond the slide's undirected statement): connected (in the underlying sense) and either every vertex has $\delta^+=\delta^-$ (circuit), or exactly one vertex with $\delta^+-\delta^-=1$ (start) and one with $\delta^--\delta^+=1$ (end).
- **Algorithm — Hierholzer** *([2012pal03]*, recursive): walk edges, removing each as you use it, recurse, and **emit edges on the way back** (push onto a stack). $\Theta(\lvert V\rvert+\lvert E\rvert)$. (Fleury's "avoid bridges" algorithm is the slower textbook alternative, $O(\lvert E\rvert^2)$.)

```
euler(v):
  for u in succ(v):
    remove edge (v,u); euler(u); push(edge (v,u))   # trail = stack of edges
```

### 2.5 ⭐ Graph isomorphism *(izomorfismus grafů)* (*[isom]*)

$G_1\cong G_2$ iff there is a **bijection** $f:V_1\to V_2$ with $\{x,y\}\in E_1\Leftrightarrow\{f(x),f(y)\}\in E_2$ (analogously for digraphs).

- **Invariants** *(invarianty)* — equal on isomorphic graphs, so a **mismatch proves non-isomorphism** (a match does not prove isomorphism): degree sequence, #edges, #triangles, connectivity, bipartiteness, girth, diameter/radius, **spectrum** (eigenvalues of $A$ or $L$), #automorphisms, chromatic/clique numbers, …
- **Complexity status:** GI is **not known to be in P** and **not known to be NP-complete** — the canonical "NP-intermediate" candidate (Babai 2015: quasi-polynomial, beyond the course). Practical solver: **nauty/Traces** (McKay–Piperno).
- **Course approach:** partition vertices into classes by invariant values, then **recursive backtracking** matching vertices only within compatible classes, pruning whenever the partial map violates the edge relation on already-matched neighbours.
- **Automorphism** = isomorphism of $G$ to itself; automorphisms form a **group**. Counts: $K_n\!:n!$; $K_{m,n}\!:2\,m!\,n!$ (for $m\ne n$, $m!\,n!\cdot$ swap if $m=n$); path $P_n\!:2$.

### 2.6 ⭐ Tree isomorphism *(izomorfismus stromů)* — AHU certificate (*[isom]*)

Unlike general GI, **tree isomorphism is (near-)linear** via a **canonical certificate** (a 0/1 string): two trees are isomorphic **iff their certificates are equal**.

- **Root at the center(s)** *(střed stromu)* — a tree has exactly **1 or 2 centers** (minimum-eccentricity vertices), found by repeatedly peeling leaves; rooting there makes the certificate canonical regardless of labelling.
- **AHU bottom-up naming:** label every vertex `01`; repeatedly, for each non-leaf $x$, take the multiset of its leaf-children labels (plus $x$'s own label stripped of its outer `0…1`), **sort them lexicographically**, concatenate, wrap with a leading `0` and trailing `1`; remove those leaves. Sorting children's labels makes sibling order irrelevant ⇒ canonical. End with 1 vertex (its label = certificate) or 2 (concatenate the two labels in lex order).
- Comparing two certificates is linear in their length; construction is near-linear (each vertex processed once; cost dominated by sorting children labels).

---

## 3. Combinatorial generation, Gray codes, primes, PRNG

### 3.1 Ranking / unranking framework (*[pal06]*, after Kreher–Stinson)

For a set $S$ of combinatorial objects, **rank** $:S\to\{0,\dots,|S|-1\}$ is a bijection and **unrank** its inverse; **successor**$(s)=$unrank$($rank$(s)+1)$. Uses: compact indexing and **uniform random generation** (pick a random integer in $[0,|S|)$, then unrank).

### 3.2 ⭐ Subsets of $\{1,\dots,n\}$

A subset ↔ its **characteristic vector** $(\chi_1,\dots,\chi_n)$; there are $2^n$. Lexicographic rank = the binary number with element $i$ as bit weight $2^{\,n-i}$ (element $1$ = most significant):

```
SUBSETLEXRANK(n,T):  r=0; for i=1..n: if i∈T: r += 2^(n-i); return r
SUBSETLEXUNRANK(n,r): T=∅; for i=n..1: if r odd: T∪={i}; r=r div 2; return T
```
Both $O(n)$.

### 3.3 ⭐ $k$-element subsets (combinations) — combinatorial number system

Store sorted $t_1<\dots<t_k$; there are $\binom nk$, listed lexicographically.

- **Successor:** find the rightmost $t_i$ not at its maximum ($n-k+i$), increment it, and minimally refill the tail $t_j=t_i+1+(j-i)$.
- **Rank** (combinatorial number system): $\displaystyle \text{rank}=\sum_{i=1}^{k}\sum_{j=t_{i-1}+1}^{t_i-1}\binom{n-j}{\,k-i\,}$ — sum the sizes $\binom{n-j}{k-i}$ of all blocks skipped before each chosen element.
- **Unrank:** for $i=1..k$, advance $x$ while $\binom{n-x}{k-i}\le r$, subtracting; set $t_i=x$.

> Worked example *([combi]*): rank of $\{6,8,9,11\}$ among 4-subsets of $\{1..12\}$ is $\binom{11}3+\binom{10}3+\binom93+\binom83+\binom73 + [\text{recurse on }\{8,9,11\}] = 460+11 = \mathbf{471}$.

### 3.4 ⭐ Permutations — factorial number system / Lehmer code

A permutation of $\{1,\dots,n\}$; there are $n!$, listed lexicographically.

- **Next permutation** (Narayana, amortized $O(1)$): (1) find rightmost $i$ with $\pi_i<\pi_{i+1}$; (2) swap $\pi_i$ with the smallest larger element to its right; (3) reverse the suffix after $i$.
- **Rank** (Lehmer code, $O(n^2)$): $\displaystyle\text{rank}=\sum_{j=1}^{n}\big(\rho_j-1\big)\,(n-j)!$, where after using $\pi_j$ the larger remaining symbols are decremented; digit $(\rho_j-1)$ = number of unused symbols smaller than $\pi_j$.
- **Unrank:** extract factorial digits $d=\lfloor (r\bmod (j{+}1)!)/j!\rfloor$ and re-insert symbols, shifting.

### 3.5 ⭐ Gray codes *(reflektovaný binární kód)* (*[pal06], [combi]*)

A binary code in which **successive values differ in exactly one bit**.

- **Reflect-and-prefix construction:** $\Gamma_1=[0,1]$; $\Gamma_n=[\,0\!\cdot\!\Gamma_{n-1}\ \text{forward},\ 1\!\cdot\!\Gamma_{n-1}\ \text{reversed}\,]$. E.g. $\Gamma_3=000,001,011,010,110,111,101,100$.
- **Conversions:** binary→Gray $g=b\oplus(b\gg1)$ (i.e. $g_j=b_j\oplus b_{j+1}$); Gray→binary is the prefix-XOR $b_j=\bigoplus_{i\ge j}g_i$.
- **Use:** rotary encoders (one-bit change avoids transition glitches); enumerating subsets in **minimal-change order** (each step adds/removes exactly one element) — handy for incremental algorithms.

### 3.6 ⭐ Primes and the Sieve of Eratosthenes (*[pal07_2015]*)

- **Prime counting** $\pi(n)$: for $n\ge17$, $\frac{n}{\ln n}<\pi(n)<1.25506\frac{n}{\ln n}$; $\pi(n)\sim n/\ln n$ (PNT). **Euler totient** $\varphi(n)=$ #integers $\le n$ coprime to $n$. **Mersenne prime** $M_p=2^p-1$.
- **Sieve of Eratosthenes** *(Eratosthenovo síto)*: cross out multiples of each prime starting at $i^2$.

```
sieve(n): A[2..n]=true
  for i=2..⌊√n⌋: if A[i]: for j=i², i²+i, …≤n: A[j]=false
  return {i : A[i]}
```
⭐ **Time $O(n\log\log n)$**, space $O(n)$.

- **Primality (context):** Fermat test (fooled by **Carmichael numbers** 561, 1105, …); **Miller–Rabin** randomized, error $\le4^{-k}$ (cf. TAL §5.8); **AKS** deterministic polynomial. **Pollard's $\rho$** factorization $\approx O(n^{1/4})$.

### 3.7 ⭐ Pseudorandom numbers & Linear Congruential Generator (*[pal07_2015]*)

Quality criteria: **uniformity** and **independence**; want long period, cheap formula.

- **LCG** *(lineární kongruentní generátor)*: $X_{n+1}=(a\,X_n+c)\bmod M$, with seed $X_0$, multiplier $a$, increment $c$, modulus $M$.
- ⭐ **Hull–Dobell theorem** — the period is **full (= $M$)** iff: (1) $\gcd(c,M)=1$; (2) $a-1$ is divisible by every prime factor of $M$; (3) if $4\mid M$ then $4\mid a-1$.
- **Low-bit weakness:** low-order bits are poorly random — output $X_n\div 2^{H}$ ($H\approx\tfrac12\log_2 M$).
- **Variants:** **Lehmer** (multiplicative, $c=0$; full period $M-1$ iff $M$ prime and $a$ a primitive root); **combined LCG** (L'Ecuyer); **Blum–Blum–Shub** $X_{n+1}=X_n^2\bmod M$ (cryptographic).

---

## 4. Search trees

Notation *([2011pal03a]*): $P(n)$ space, $Q(n)$ query, $I(n)$ insert, $D(n)$ delete. Baselines: unsorted array $Q=O(n)$; sorted array $Q=O(\log n)$ but $I,D=O(n)$; **interpolation search** $O(\log\log n)$ average (uniform data), $O(n)$ worst — motivating dynamic balanced trees.

### 4.1 Binary Search Tree (BST) *(binární vyhledávací strom)* (*[2011pal03a]*)

**Invariant:** keys in left subtree $<x.\text{key}<$ keys in right subtree. Operations Search/Min/Max/Successor/Predecessor/Insert/Delete are all $O(h)$.

- **Delete — 3 cases:** (a) leaf → remove; (b) one child → splice it out; (c) two children → replace key with its **predecessor/successor** (which has $\le1$ child) and delete that node.
- **Height:** $h=\Theta(\log n)$ balanced, but $h=\Theta(n)$ for sorted insertion (degenerates to a list) ⇒ **must balance**. Balance criteria: subtree heights (AVL), black-height (red-black), subtree weights (weight-balanced), #children (B-/2-3-4).

### 4.2 ⭐ AVL tree (*[2011pal03a]*)

Adelson-Velskij–Landis 1962. **Balance factor** $\text{bal}(x)=h(x.\text{left})-h(x.\text{right})\in\{-1,0,+1\}$ at every node (empty height $=-1$).

- **Rotations:** single **L**, single **R**; doubles **LR** (left-rotate child, then right-rotate node) and **RL**. After a BST insert, the first unbalanced node on the way up is fixed by exactly one rotation event:

| $\text{bal}$ | inserted side of taller child | fix |
|---|---|---|
| $+2$ | left | **R** |
| $+2$ | right | **LR** |
| $-2$ | left | **RL** |
| $-2$ | right | **L** |

- ⭐ **Height bound** (verified): minimum nodes of height $h$ is $N(h)=F_{h+3}-1$ (Fibonacci) ⇒ $\log_2(n+1)\le h\le \mathbf{1.4404\log_2(n+2)-0.328}$, i.e. $h\le\log_\varphi n$, $\varphi=\tfrac{1+\sqrt5}2$.
- **Complexities:** Search/Insert/Delete $O(\log n)$ worst case; insert needs $\le1$ rotation event.

### 4.3 ⭐ Red-Black tree (*[2011pal03c]*)

Approximately balanced ($h_{RB}\le 2h_{\text{ideal}}$), one **color** bit/node, black `nil` sentinels. **Five properties:** (1) each node red or black; (2) every leaf (`nil`) black; (3) a red node has two black children (no two reds in a row); (4) all root→leaf paths have the same number of black nodes; (5) root black.

- **Black-height** $bh(x)$ = #black nodes on a path from $x$ down to a leaf (excluding $x$). ⭐ **Height bound** $h\le 2\lg(n+1)$ (proof: subtree of $x$ has $\ge2^{bh(x)}-1$ internal nodes; $bh\ge h/2$).
- **Insert:** new node red, BST-insert; fix the "two reds" violation by **recoloring** (uncle red) or **rotation** (uncle black, cases 2→3). **$\Theta(\log n)$, $\le2$ rotations.**
- **Delete:** BST-delete; if a black node is removed, propagate a "double-black" up, fixed by sibling cases 1–4. **$\Theta(\log n)$, $\le3$ rotations.**
- Equivalent to a **2-3-4 tree** (B-tree of degree 2–4).

### 4.4 ⭐ B-tree and B+ tree (*[paska11b], [2011pal03c]*)

Bayer–McCreight 1972 — "BST on disk": each node = one disk block; minimize **disk accesses** (one disk seek ≈ millions of instructions).

- **B-tree of order $m$** (max $m$ children, $m-1$ keys): all leaves at the **same depth**; every non-root node has between $\lceil m/2\rceil$ and $m$ children; root has $2..m$. (Cormen's **minimum degree $t$**: $t-1..2t-1$ keys; $m=2t$ "optimized/single-phase" or $m=2t-1$ "multiphase".)
- **Search** $O(\log_m n)$ block reads; **height** $h\le\log_{\lceil m/2\rceil}\frac{n+1}2$.
- **Insert:** *multiphase* — insert in leaf; on overflow split around the **median**, promote it upward (bottom-up). *Single-phase* — split every full node on the way down (top-down). Root split grows the tree by one level.
- **Delete:** internal-node key swapped with leaf predecessor/successor; on underflow **borrow** from a richer sibling (rotation through parent) else **merge** node+separator+sibling and recurse up.
- **B+ tree:** records only in **leaves** (internal nodes = routers); **leaves linked** in a list ⇒ fast sequential and ⭐ **range queries** $\Theta(b\log_b n + k/b)$ ($k$ = #results). Find/Insert/Delete $\Theta(b\log_b n)$.

### 4.5 ⭐ Splay tree (*[paska12b]*)

Sleator–Tarjan 1985. **Self-adjusting** BST; no balance/extra fields. Every accessed/inserted node is **splayed** to the root by rotations:

- **Zig** ($x$'s parent is root): one rotation. **Zig-zig** ($x,p$ same-side children): rotate at **grandparent first**, then parent. **Zig-zag** ($x,p$ opposite sides): rotate at parent then grandparent (= AVL double).
- **Find/Insert** = BST op then splay; **Delete** = splay node to root, remove, join the two subtrees via the max of the left part.
- **Worst single op $O(n)$** (can be a chain), **amortized $O(\log n)$** for all ops. Read access mutates the tree. **Dynamic optimality conjecture** (open): within a constant factor of any rotation-based algorithm.

### 4.6 ⭐ k-d tree (*[paska13]*)

Multidimensional BST over $D$-dim points; a node at depth $h$ splits on **cutting dimension $h\bmod D$** (dimensions cycle): left $<$ node's coordinate, right $\ge$. Balanced build by **median** splits ⇒ height $O(\log n)$.

- **Find/Insert** $O(h)$ (compare only the current cutting coordinate).
- **FindMin(dim)** (needed by Delete): if node's cutting dim $=$ dim, search only the left subtree; else **both** subtrees. ⭐ **$O(n^{1-1/d})$** ($=O(\sqrt n)$ for $d=2$).
- ⭐ **Nearest-neighbour search** — recurse, **nearer child first**, keeping the best distance `close`; **prune** a subtree whose cell (hyper-rectangle) is farther than `close`; **backtrack** to the far child only if its cell could contain something closer. Expected $\approx O(2^D+\log n)$ (good only when $2^D\ll n$ — *curse of dimensionality*), worst $O(n)$.
- (Orthogonal **range search** is $O(n^{1-1/d}+k)$ in standard references; not stated on these slides.)

### 4.7 ⭐ Skip list (*[paska11a]*)

Pugh 1990 — a randomized, layered ordered linked list; "probabilistic alternative to balanced trees."

- Each node has a random number of forward pointers; level chosen by coin flips with parameter $p$ (geometric): fraction $p$ of level-$k$ nodes reach level $k+1$.
- **Search:** from the header's top level, advance while `forward.key < target`, drop a level on overshoot.
- **Insert/Delete:** record per-level predecessors in `update[]`, then splice/unsplice; list level rises by $\le1$ per insert.
- ⭐ **Expected $O(\log n)$** search/insert/delete; **worst $O(n)$**. Expected space $N/(1-p)$ pointers. $p=\tfrac12$ ⇒ ≈$\log_2 n$ search, 2 ptrs/node; $p=\tfrac14$ ⇒ less space (~1.33 ptrs), more variance. Augmenting pointers with span lengths gives expected-$O(\log n)$ indexed (rank) access.

---

## 5. Finite automata and text search

Framing follows **Melichar–Holub–Polcár**, *Text Searching Algorithms* (CTU, 2005).

### 5.1 ⭐ Finite automata & regular languages (*[automata101], [paska08b]*)

- **DFA** *(deterministický KA)*: $(Q,\Sigma,\delta,q_0,F)$ with $\delta:Q\times\Sigma\to Q$ — always in one state. **NFA** *(nedeterministický)*: $\delta:Q\times\Sigma\to\mathcal P(Q)$ — in a **set** of states; accepts if **some** run ends in $F$. Running a DFA on text of length $n$ is $O(n)$, $O(1)$ per symbol.
- FAs recognize exactly the **regular languages** (Type-3 in the Chomsky hierarchy; cf. TAL).
- **Regular expression** *(regulární výraz)*: built from $\varepsilon$, symbols, and $e_1+e_2$ (union), $e_1e_2$ (concat), $(e_1)^*$ (star).
- **Operations & closure** *([paska09]*): regular languages are closed under **union, intersection, concatenation, star, complement**. Constructions: union/concat/star via $\varepsilon$-transitions; **intersection** via the **Cartesian product** automaton $Q_1\times Q_2$; complement by swapping final/non-final states of a **complete DFA**.
- **Determinization (subset construction)** *([paska08b]*): each DFA state = a subset of NFA states; start $=\{q_0\}$ (or its $\varepsilon$-closure), accepting if it contains an NFA final state. **Worst case exponential** ($2^{|Q|}$): e.g. $a(a+b)^{n}$ needs $\Theta(2^n)$ DFA states.

### 5.2 ⭐ Search automata & bit-parallel simulation (*[paska08b], [paska09]*)

- **Acceptor → text-search automaton:** add a **$\Sigma$-self-loop at the start state**, so a match is found anywhere (even overlapping) in the text. The exact-pattern search NFA is the linear chain $q_0\!\xrightarrow{p_1}\!q_1\cdots\xrightarrow{p_m}\!q_m$ plus the start self-loop; report when a final state is active.
- **Bit representation of an NFA** *([paska09]*): transition table entry $T[i,k]$ is a **bit vector of length $|Q|$** with bit $j$ set iff $q_j\in\delta(q_i,a_k)$; the active-state set is a bit vector $S$. One step is the **bitwise OR** of the transition vectors of all active states:
```
S0 = 100…0
for each text symbol t[i]:
   report match if (S & F) != 0
   S = OR over active j of T[j][t[i]]      # bit-parallel transition
```
- ⭐ **Shift-Or / Shift-And** = this OR-step made $O(1)$ when the search NFA is the linear "string" shape (state set fits one machine word). Precompute a mask $B[a]$ per symbol; **Shift-And** $D\leftarrow((D\!\ll\!1)\,|\,1)\,\&\,B[t_j]$ (match when bit $m{-}1$ set). ⭐ **Complexity $O(n\lceil m/w\rceil)$** ($w$ = word width) $=O(n)$ when $m\le w$ — independent of pattern and alphabet.

### 5.3 ⭐ Exact pattern matching (*[paska08b], [pal09]*)

Text $t_1\dots t_n$, pattern $p_1\dots p_m$, $m\ll n$.

- **Naïve** *(naivní)*: try every alignment; mismatch ⇒ shift by 1. **$O(mn)$** worst case.
- **Knuth–Morris–Pratt (KMP)**: precompute the **failure/prefix function** $\pi[i]$ = longest proper prefix of $p_1..p_i$ that is also its suffix; on mismatch reset the pattern index to $\pi[i]$ without moving the text pointer. **Preprocess $O(m)$, search $O(n)$, total $O(m+n)$** (= running the deterministic search automaton). *(Standard course content; the slides build the equivalent search automaton rather than the $\pi$ table.)*
- **Boyer–Moore** *([pal09]*): match the pattern **back-to-front**; shift by $\max$ of two heuristics:
  - **Bad-character shift (BCS)** — table over $\Sigma$ giving each symbol's **distance from the pattern's end** (pattern length if absent); align the mismatched text symbol with its last occurrence in the pattern.
  - **Good-suffix shift (GS)** — when a suffix matched but its preceding char mismatched, shift to the next occurrence of that suffix (or of a suffix that is also a prefix), else by the whole length.
  - Sublinear in the best case (longer pattern ⇒ bigger jumps); preprocess $O(m+|\Sigma|)$; basic variant $O(mn)$ pathological, linear-worst-case variants exist (Galil).

### 5.4 ⭐ Approximate pattern matching (*[paska09], [pal09]*)

- **Hamming distance** *(Hammingova vzdálenost)*: #mismatches between equal-length strings (substitutions only). Text search via DP, $d[i][k]=d[i-1][k-1]+[p_i\ne t_k]$ with $d[0][k]=0$. **$O(mn)$**.
- **Levenshtein (edit) distance** *(Levenshteinova vzdálenost)*: min #of **insert / delete / substitute** ops; any-length strings. DP:
$$D[i][j]=\begin{cases}D[i-1][j-1] & p_i=t_j\\ 1+\min\big(D[i-1][j-1],\,D[i-1][j],\,D[i][j-1]\big) & \text{else}\end{cases}$$
with $D[i][0]=i,\ D[0][j]=j$. **$O(mn)$.** For **text search**, set $d[0][k]=0$ (any position may start a match); $d[m][k]$ = best edit distance of $P$ to a substring ending at $k$ (the start position must then be recovered by checking candidates).
- **NFA-based** *([paska09]*): a ladder of $m{+}1$-state rows, one extra row per allowed error. Diagonal "error" edges labelled $\bar x=\Sigma\setminus\{x\}$ so the **row index = number of errors**. Levenshtein NFA adds **vertical edges = insert** and **$\varepsilon$-edges = delete** (Hamming uses substitution/diagonal only).

### 5.5 ⭐ Dictionary automata: trie, Patricia, Aho–Corasick (*[pal09], [paska13trie]*)

A **dictionary** *(slovník)* is a finite set of patterns — a finite (hence regular) language, searchable by an NFA/DFA.

- **Trie** *(písmenkový strom)* (Sedgewick): prefix tree; the node at depth $d$ branches on the $d$-th symbol/bit. Keys in leaves; no key may be a prefix of another. $N$ random $w$-bit keys ⇒ ≈ $1.44N$ nodes, ≈ $\lg N$ comparisons; structure independent of insertion order. **Patricia trie** (Morrison 1968) stores the bit-index to test per node, collapsing one-way chains — best for **long keys**.
- ⭐ **Dictionary NFA → DFA without blow-up:** because the dictionary NFA is a **trie rooted at the start (with a start $\Sigma$-loop)**, determinization **does not increase the number of states**.
- ⭐ **Aho–Corasick** — the dictionary DFA built from the trie with three functions:
  - **goto** = trie edges (prefixes of the dictionary);
  - **failure (suffix) link** = to the longest proper suffix of the current string that is also a prefix in the trie (followed on a goto-miss);
  - **output** = patterns ending at this state (collected along failure links).
  Built by BFS over the trie. ⭐ **Search $O(n+z)$** ($n$ = text length, $z$ = #occurrences reported).
- A finite-automaton search can thus locate: an exact pattern; any word of a regex/DFA language; strings within a Hamming/Levenshtein bound; any dictionary word; unions/intersections/concatenations of these; or any string **containing** the pattern as a subsequence (add $\Sigma$-loops at all states).

---

## Appendix: Heaps (priority queues) (*[pal04], [binary_heap], [binomial_heap]*)

Heaps back Jarník–Prim/Dijkstra and combinatorial selection.

| Heap | Insert | Extract-Min | Decrease-Key | Merge |
|---|---|---|---|---|
| **Binary heap** *(binární halda)* | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ | $O(n)$ |
| **$d$-ary heap** | $O(\log_d n)$ | $O(d\log_d n)$ | $O(\log_d n)$ | $O(n)$ |
| **Binomial heap** *(binomiální)* | $O(\log n)$ (amort. $O(1)$) | $O(\log n)$ | $O(\log n)$ | $O(\log n)$ |
| **Fibonacci heap** *(Fibonacciho)* | $O(1)$ | $O(\log n)$ amort. | $O(1)$ amort. | $O(1)$ |

- **Binary heap:** complete binary tree in an array; `bubble-up` on insert, `bubble-down` (heapify) on extract; `build-heap` in $O(n)$.
- **Binomial heap:** forest of binomial trees $B_k$ ($2^k$ nodes, height $k$); merge = binary-addition-style linking of equal orders.
- **Fibonacci heap:** lazy melding + cascading cuts; its $O(1)$ amortized decrease-key gives Prim/Dijkstra $O(\lvert E\rvert+\lvert V\rvert\log\lvert V\rvert)$.

---

## Key results cheat-sheet

| Topic | Result / algorithm | Complexity |
|---|---|---|
| Traversals | DFS / BFS (adjacency list) | $\Theta(\lvert V\rvert+\lvert E\rvert)$ |
| DAG | topological order; longest path | $\Theta(\lvert V\rvert+\lvert E\rvert)$ |
| MST | **Jarník–Prim** (Fib. heap) | $O(\lvert E\rvert+\lvert V\rvert\log\lvert V\rvert)$ |
| MST | **Kruskal** / **Borůvka** | $O(\lvert E\rvert\log\lvert V\rvert)$ |
| Disjoint sets | **Union-Find** (rank + path compression) | $O(m\,\alpha(n))$ |
| SCC | **Kosaraju–Sharir** (2 DFS) / **Tarjan** (1 DFS) | $\Theta(\lvert V\rvert+\lvert E\rvert)$ |
| Euler trail | **Hierholzer** | $\Theta(\lvert V\rvert+\lvert E\rvert)$ |
| Tree isomorphism | **AHU certificate** | near-linear |
| Graph isomorphism | open (NP-intermediate candidate) | quasi-poly (Babai) |
| Subset/comb/perm rank | combinatorial / factorial number system | $O(n)$ / $O(n^2)$ |
| Primes | **Sieve of Eratosthenes** | $O(n\log\log n)$ |
| Search trees | **AVL** ($h\le1.44\log_2 n$), **RB** ($h\le2\lg(n{+}1)$) | $O(\log n)$ |
| Search trees | **B/B+** (disk), **splay** (amort.), **skip list** (exp.) | $O(\log n)$ |
| k-d tree | nearest-neighbour | exp. $O(2^D+\log n)$, worst $O(n)$ |
| Exact matching | **KMP** $O(m+n)$; **Boyer–Moore** sublinear best | — |
| Bit-parallel / **Shift-Or** | linear search NFA | $O(n\lceil m/w\rceil)$ |
| Approximate matching | **Hamming / Levenshtein** DP | $O(mn)$ |
| Dictionary | **Aho–Corasick** | $O(n+z)$ |

---

## References

- **Course page:** [cw.fel.cvut.cz/wiki/courses/be4m33pal/start](https://cw.fel.cvut.cz/wiki/courses/be4m33pal/start). Lecture decks in `materials/`.
- **CLRS:** Cormen, Leiserson, Rivest, Stein, *Introduction to Algorithms*, 3rd/4th ed. (asymptotics, MST, Union-Find, SCC, RB/B-trees, number theory, string matching).
- **Text search:** Melichar, Holub, Polcár, *Text Searching Algorithms, Vol. I*, CTU FEE, 2005.
- **Combinatorial generation:** Kreher, Stinson, *Combinatorial Algorithms: Generation, Enumeration, and Search*, CRC, 1998.
- **Data structures:** Sedgewick, *Algorithms*; Pugh (1990) skip lists; Sleator–Tarjan (1985) splay trees; Bayer–McCreight (1972) B-trees.
- **Cross-checks:** Kirchhoff's matrix-tree theorem & graph Laplacian (algebraic graph theory); AVL height $N(h)=F_{h+3}-1$; Hull–Dobell theorem for LCG full period.

> **Source-coverage notes for Tomas:** (1) the **distance / Laplacian / incidence matrices** required by official topic 1 are *not* in the provided decks — definitions above are standard and were verified online; (2) the slides give the **undirected** Euler condition only (directed condition added as standard); (3) the decks build search **automata** rather than spelling out **KMP**'s $\pi$-table and an explicit **Shift-Or** — both added as standard course material and flagged. If you have additional PAL decks covering these, drop them in `materials/` and I'll align the text.



