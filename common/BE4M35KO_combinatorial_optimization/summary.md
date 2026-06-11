# BE4M35KO — Combinatorial Optimization (KO) — Study Summary

**Lecturer:** prof. Zdeněk Hanzálek (+ Přemysl Šůcha), CTU FEE, course KO.
**Language:** English-primary (slides are in English); key terms also in Czech *(kurzívou v závorkách)* for the oral exam.
**Sources:**

- `materials/1_Basic_KO.pdf` … `8_CP_KO.pdf` — official 2026 lecture slides (cited as *[L1]…[L8]*).
- `materials/Dumbshit guide to KO.pdf` — student study guide (informal, cross-check only).
- Course page: <https://cw.fel.cvut.cz/wiki/courses/ko/start>.
- Textbook: **Korte & Vygen**, *Combinatorial Optimization: Theory and Algorithms*, Springer (cited as *KV*). Also Ahuja–Magnanti–Orlin (*AMO*) for flows.

**Structure** mirrors the 7 official exam topics (see `../../00_subjects_overview.md`):

0. [Preliminaries — graph theory & complexity](#0-preliminaries)
1. [Integer Linear Programming](#1-integer-linear-programming-ilp)
2. [Shortest Paths](#2-shortest-paths)
3. [Network Flows](#3-network-flows)
4. [Knapsack Problem](#4-knapsack-problem)
5. [Traveling Salesman Problem](#5-traveling-salesman-problem-tsp)
6. [Scheduling](#6-scheduling)
7. [Constraint Satisfaction Problem](#7-constraint-satisfaction-problem-csp)

⭐ = exam emphasis. Complexities below are taken from the slides and cross-checked against standard sources.

---

## 0. Preliminaries

### 0.1 Graph theory recap *(teorie grafů)* (*[L1]*)

- **Digraph** *(orientovaný graf)* $(V,E,\Psi)$, $\Psi: E \to \{(v,w): v\neq w\}$; **undirected** $\Psi: E\to\{X\subseteq V: |X|=2\}$.
- **Simple graph** = no parallel edges; written $G=(V,E)$.
- **Subgraph** *(podgraf)*; **spanning subgraph / factor** *(faktor)*: $V(H)=V(G)$, drop only edges; **induced subgraph** *(indukovaný podgraf)*: drop vertices + incident edges.
- Degrees: $\delta^+(v)$ out-edges, $\delta^-(v)$ in-edges, $\delta(v)$ incident; $\sum_v|\delta(v)|=2|E|$; **#odd-degree vertices is even**.
- $\delta^+(X),\delta^-(X),\delta(X)$ — edges leaving / entering / bordering a vertex **set** $X$ (used everywhere in flows).
- ⭐ **Walk** *(sled)* = any alternating sequence of vertices/edges — repeats allowed; **trail** *(tah)* = walk with no repeated **edge**; **path** *(cesta)* = walk with no repeated **vertex**; **cycle/circuit** *(cyklus/kružnice)* = closed trail/path. (Filip 2018 asked exactly "definujte tah, sled a cestu" — these three, in Czech, with the differences.)
- **Tree** *(strom)* = connected acyclic undirected graph; $n$ nodes ⇒ $n-1$ edges; unique path between any two vertices; **spanning tree** *(kostra)* exists for every connected graph.
- **Strongly connected** *(silně souvislý)* digraph: path both ways for every pair; **topological order** exists ⇔ graph is a **DAG** *(acyklický)*.

### 0.2 Complexity reminders

- LP polynomial; **ILP is NP-hard**. **CSP / SAT NP-complete**. Several problems below are **NP-hard** (knapsack, TSP, most scheduling) vs **polynomial** (shortest paths, flows, matching).
- **Pseudopolynomial** *(pseudopolynomiální)*: polynomial in the numeric *value* of the input (e.g. knapsack DP $O(nC)$). **Strongly NP-hard** = remains NP-hard even when all numbers are bounded by a polynomial in input size ⇒ **no pseudopoly algorithm unless P=NP** (e.g. TSP).
- Optimization problems have three versions: **decision** *(rozhodovací)*, **evaluation** *(vyhodnocovací)*, **optimization** *(optimalizační)* — polynomially equivalent for the problems here.

---

## 1. Integer Linear Programming (ILP)

> Official subpoints: ILP; shortest-path & TSP ILP formulations; Branch & Bound; problem formulations via ILP; special poly-solvable ILP. (*[L2]*)

### 1.1 Problem statement ⭐

$$\max\{\,c^T x : A x \le b,\ x \in \mathbb{Z}^n\,\},\quad A\in\mathbb{R}^{m\times n},\,b\in\mathbb{R}^m,\,c\in\mathbb{R}^n.$$

- If only **some** variables are integer ⇒ **MIP** *(smíšené celočíselné)*; the course says "ILP" once ≥1 variable is integer.
- **ILP vs LP:** LP feasible region is a convex polyhedron, solvable in polynomial time. ILP feasible set is **not convex**; **NP-hard**.
- **Rounding the LP solution is wrong:** it can be infeasible, or feasible but far from optimal. (Classic example: LP optimum $(6.06,1.21)$, rounding gives infeasible or suboptimal; true integer optimum lies elsewhere.)

### 1.2 Branch & Bound (B&B) ⭐⭐ (*[L2]*)

Splits the solution space into disjoint subsets (a **tree**). Idea:

1. **Relax** integrality, solve the LP. If all $x_i$ integer → done (for this branch).
2. Else pick a fractional $x_i$, let $k=\lfloor x_i\rfloor$, and **branch** into two subproblems: $x_i\le k$ and $x_i\ge k+1$.
3. Recurse. Keep $z^*$ = best integer solution found.
4. **Bounding:** discard a node whose LP objective $z_{LP}\le z^*$ (for max) — it cannot beat the incumbent (the LP relaxation is an **upper bound** for a maximization problem).

Uses **dual simplex** to re-optimize after adding a constraint without restarting. The B&B solution space is a **tree** (contrast with the **diamonds** of dynamic programming, §2.4).

```
ILP(A,b,c, zb,xb):           # zb,xb = best known
  zLP,xLP := solve LP relaxation
  if infeasible: return -inf
  if zLP <= zb: return zb,xb            # bound
  if xLP integral: return zLP,xLP
  pick fractional x_i; k := floor(x_i)
  z',x'  := ILP(A + [x_i<=k],   ..., zb,xb); update zb,xb
  z'',x'':= ILP(A + [x_i>=k+1], ..., zb,xb); update zb,xb
  return zb,xb
```

### 1.3 ILP formulations to know ⭐

**Shortest path (LP/ILP):** variable $x_j=1$ iff edge $j$ used; flow-conservation $\sum_j w_{ij}x_j = b_i$ with $b_s=1,b_t=-1,0$ elsewhere; $\min\sum_j c_j x_j$. The matrix is the incidence matrix $W$ → **totally unimodular** → LP already returns integral $x$ (§1.4).

**TSP (asymmetric, MTZ subtour elimination)** ⭐: $x_{ij}=1$ iff $i$ immediately precedes $j$.
$$\min\sum_{i,j} c_{ij}x_{ij}\quad \text{s.t.}\quad \sum_j x_{ij}=1\ (\text{leave once}),\ \sum_i x_{ij}=1\ (\text{enter once}),\ x_{ii}=0,$$
$$s_i + c_{ij} - (1-x_{ij})M \le s_j\quad (i=1..n,\ j=2..n)\ \text{— Miller–Tucker–Zemlin subtour elimination.}$$
The enter/leave constraints alone allow disjoint sub-tours; the $s_i$ ("time of entering node $i$") + big-$M$ forbid them.

⭐ **The alternative formulation Šůcha asks about** ("jak by šlo zadefinovat TSP ještě jinak?", 2023): **DFJ (Dantzig–Fulkerson–Johnson) subtour-elimination constraints** — for every proper subset $S \subset V$, $2 \le |S| \le n-1$:
$$\sum_{i \in S}\sum_{j \notin S} x_{ij} \ge 1 \qquad\text{(every subset must be left at least once)},$$
equivalently $\sum_{i,j\in S} x_{ij} \le |S|-1$. There are **exponentially many** such constraints, so they are added **lazily** *(lazy constraints)*: solve without them, find a subtour in the integer solution, add only the violated constraint, re-solve. This was the KO homework — that's why he probes it.

| | MTZ | DFJ (lazy) |
|---|---|---|
| #constraints | $O(n^2)$ — polynomial, all upfront | $O(2^n)$ — added on demand |
| LP relaxation | weak (big-$M$) | **tight** — basis of branch-and-cut |
| Practice | small instances, simple to state | what real solvers (Concorde) use |

**2-partition** *(rozdělení na 2 stejné části)*: $x_i\in\{0,1\}$, $\sum_i x_i p_i = \tfrac12\sum_i p_i$. One of the "easiest" NP-complete problems; equivalent to $P2||C_{\max}$ scheduling. Fractional variant ($x_i\in\langle0,1\rangle$) is LP, hence polynomial (≈ preemptive scheduling).

**Knapsack** *(batoh)*: $\max\sum_i c_ix_i$ s.t. $\sum_i w_ix_i\le W$, $x_i\in\{0,1\}$. The "buildings" selection example on the slides is exactly this 0/1 form (see §4). Cf. §4 for the DP/FPTAS algorithms.

**Hamiltonian cycle (HC) / path** *(hamiltonovská kružnice / cesta)* ⭐: the TSP model above **is** the Hamiltonian-cycle formulation — the enter-once / leave-once degree constraints plus MTZ subtour elimination describe *exactly* a single closed tour through all $n$ nodes. For pure **HC feasibility** (does a Hamiltonian cycle exist?) drop the objective (set all $c_{ij}=0$, or minimize $0$) and ask whether the system is feasible. For a **Hamiltonian path $s\to t$**, keep the degree + MTZ constraints but relax them at the endpoints: $\sum_j x_{sj}=1$, $\sum_i x_{is}=0$, $\sum_i x_{it}=1$, $\sum_j x_{tj}=0$ (start leaves but is never entered; target is entered but never left). This is the canonical way "graph NP problems" enter ILP — via the TSP/HC constraint skeleton.

### 1.3b Catalog: classic NP problems as ILP ⭐⭐

"Problem formulations using ILP" is its own exam subpoint. ILP can encode essentially every problem from TAL's NPC catalogue (cross-ref `../../common/BE4M01TAL_theory_of_algorithms/summary.md §3.7`). Standard 0/1 formulations:

| Problem | Variables | Objective | Constraints |
|---|---|---|---|
| **Vertex cover** *(vrcholové pokrytí)* | $x_v\in\{0,1\}$ (vertex chosen) | $\min\sum_v x_v$ | $x_u+x_v\ge1$ for every edge $\{u,v\}$ |
| **Independent set** *(nezávislá množina)* | $x_v\in\{0,1\}$ | $\max\sum_v x_v$ | $x_u+x_v\le1$ for every edge $\{u,v\}$ |
| **Max clique** *(klika)* | $x_v\in\{0,1\}$ | $\max\sum_v x_v$ | $x_u+x_v\le1$ for every **non-**edge $\{u,v\}$ (clique = IS in complement) |
| **Set cover** *(pokrytí množinami)* | $x_S\in\{0,1\}$ (set $S$ chosen) | $\min\sum_S x_S$ | $\sum_{S\ni e}x_S\ge1$ for every element $e$ |
| **Graph $k$-coloring** *(barvení)* | $x_{v,c}\in\{0,1\}$ (vertex $v$ gets color $c$) | feasibility / $\min$ colors | $\sum_c x_{v,c}=1$; $x_{u,c}+x_{v,c}\le1$ per edge & color |
| **SAT (CNF)** | $x_i\in\{0,1\}$ per variable | feasibility | per clause $\sum_{\text{pos lit}}x_i+\sum_{\text{neg lit}}(1-x_i)\ge1$ |
| **Assignment** *(přiřazení)* | $x_{ij}\in\{0,1\}$ | $\min\sum c_{ij}x_{ij}$ | $\sum_j x_{ij}=1,\ \sum_i x_{ij}=1$ (matrix is **TU** → LP suffices, §1.4) |

Note the pattern: vertex cover and independent set are complementary ($x_v\mapsto1-x_v$), and clique on $G$ = independent set on $\bar G$ — the same dualities TAL proves via polynomial reductions, here visible directly in the constraints.

### 1.3c ILP modeling toolbox (big-$M$ tricks) ⭐⭐ (*[L2]*)

These appear repeatedly on the exam and underpin the scheduling models (§6):

- **Logical constraints via binaries:** "buy building $i$" $x_i\in\{0,1\}$; "if 1 then 2" ⇒ $x_2\ge x_1$; "not both 3 and 4" ⇒ $x_3+x_4\le1$; "exactly one of 5,6" ⇒ $x_5+x_6=1$ (XOR).
- ⭐ **Fixed costs** *(fixní náklady)* — Hanzálek asks for this by name (2013, 2016): pay a one-off cost $d_i$ whenever activity $x_i > 0$ runs at all. Binary switch $y_i\in\{0,1\}$, **linking constraint** $x_i\le M\,y_i$ (forces $y_i=1$ whenever $x_i>0$), objective $\min \sum_i (c_i x_i + d_i y_i)$. The same linking trick covers "activate machine $i$ only if used".
- **One of several discrete values** (e.g. capacity $\in\{40,80,120,160\}$): $\sum_k a_k v_k$ with $\sum_k v_k=1$, $v_k\in\{0,1\}$.
- **Disjunction — at least one of two constraints (OR)** via big-$M$ and a switch $y\in\{0,1\}$:
$$f_1(x)\le b_1+M\,y,\qquad f_2(x)\le b_2+M\,(1-y).$$
For $y=0$ the first is active; for $y=1$ the second. This is the **non-convex** modeling primitive (a union of half-spaces).
- **At least $K$ of $N$ constraints** hold: $f_i(x)\le b_i+M\,y_i$ with $\sum_i y_i\le N-K$.
- **Non-preemptive scheduling $1\mid r_j,\tilde d_j\mid C_{\max}$ as ILP** (the lecture's flagship disjunctive model, cf. §6.2 Bratley): for each task pair introduce $x_{ij}=1$ iff $T_i$ precedes $T_j$ and post the **disjunction** "$T_i$ before $T_j$ **or** $T_j$ before $T_i$":
$$s_j\ge s_i+p_i-M\,(1-x_{ij}),\qquad s_i\ge s_j+p_j-M\,x_{ij},$$
plus $s_j\ge r_j$ and $s_j+p_j\le\tilde d_j$. The two big-$M$ inequalities are exactly the OR primitive above.

### 1.4 Special poly-solvable ILP — Totally Unimodular matrices ⭐⭐ (*[L2]*)

**Definition.** $A$ is **totally unimodular (TU)** *(zcela unimodulární)* if every square submatrix has determinant $\in\{-1,0,+1\}$. (Necessary: all $a_{ij}\in\{-1,0,1\}$.)

**Integral-polyhedron lemma.** If $A$ is TU and $b\in\mathbb{Z}^m$, then **every vertex of $\{x: Ax\le b\}$ is integer** ⇒ the simplex method returns an **integer** optimum although we solved an LP. (KV Thm 8.1 / Schrijver.) So a TU-structured ILP is solvable in polynomial time (via a polynomial LP algorithm).

**Sufficient conditions** for TU:
1. Each **column** has at most one $+1$ and at most one $-1$ (rest 0). → incidence matrix of a digraph (shortest path, flows).
2. Entries in $\{0,1\}$, ≤ two 1s per column, rows partitionable into two sets so each column has ≤ one 1 in each set. → bipartite-graph / assignment matrices.
3. **Consecutive-ones property** *(interval matrix)*: the 1s in each column form one contiguous block after a row permutation. → workforce shift scheduling.

TU is preserved by: row/column permutation, multiplying rows/cols by $-1$, transpose, appending identity $(A,I)$ or $\binom{A}{I}$.

---

## 2. Shortest Paths

> Official subpoints: Dijkstra, Bellman–Ford, Floyd–Warshall; shortest paths in DAGs; problem formulations. (*[L3]*)

### 2.1 Problem & basic facts ⭐

- **Instance:** digraph $G$, weights $c:E\to\mathbb{R}$, nodes $s,t$. **Goal:** min-weight $s$–$t$ path, or "unreachable".
- **Negative weights allowed, negative cycles NOT** — with a negative cycle the shortest *path* problem is **NP-hard** (you can loop forever; reduces from Hamiltonian cycle with all weights $-1$).
- $l(s,t)$ = distance = length of shortest path. **Triangle inequality** (no negative cycle): $l(i,j)\le l(i,k)+l(k,j)$.
- **Bellman's principle of optimality** *(princip optimality)*: a sub-path of a shortest path is itself shortest. **Bellman equation:** $l(s,w)=\min_{v\ne w}\{l(s,v)+c(v,w)\}$.
- Variants: single-pair (a), single-source SPT (b), single-sink (c, reverse edges → b), all-pairs (d). Longest path → negate weights. Node weights → split node $v$ into $v_1\!\to\!v_2$.

### 2.2 Dijkstra [1959] — nonnegative weights ⭐⭐

**Label-setting** *(štítkovací s trvalým štítkem)*: once a node is removed it is permanent/optimal.

```
l(s):=0; l(v):=inf for v!=s; R:=∅
while R != V:
  pick v in V\R with minimal l(v); R := R ∪ {v}
  for each (v,w), w∉R:  if l(w) > l(v)+c(v,w): l(w):=l(v)+c(v,w); p(w):=v
```

- **Complexity** $O(n^2)$, or $O(m+n\log n)$ with a (Fibonacci-heap) priority queue.
- Correctness by induction (Bellman equation holds for the chosen $v$ because all $c\ge0$).
- Can stop when $t$ enters $R$. **A\*** = Dijkstra + admissible lower-bound heuristic $h(v,t)$ (needs $c(v_1,v_2)\ge h(v_1,t)-h(v_2,t)$; Euclidean distance works). **Bidirectional Dijkstra** searches forward from $s$ and backward from $t$.
- ⚠️ Dijkstra (SPT) ≠ Prim (MST): SPT minimizes distances from a root, MST minimizes total tree weight.

### 2.3 Bellman–Ford [1958] — handles negative weights ⭐⭐

```
l(s):=0; l(v):=inf
for i := 1 to n-1:
  for each edge (v,w):  if l(w) > l(v)+c(v,w): l(w):=l(v)+c(v,w); p(w):=v
```

- **Complexity** $O(nm)$. Best known for SPT with (allowed) negative weights. Early-stop if a full pass makes no change.
- Correctness: after $k$ outer iterations, $l_k(w)\le$ length of the shortest $s$–$w$ path with $\le k$ edges (induction + Bellman's principle); a shortest path has $\le n-1$ edges.
- **Negative-cycle detection:** run one more pass; if any edge still relaxes ⇒ negative cycle reachable from $s$. To catch cycles not reachable from $s$, add $s'$ with 0-weight edges to all nodes.

### 2.4 Dynamic Programming perspective (*[L3]*)

Both DP attributes hold: **optimal substructure** (Bellman equation) + **overlapping subproblems** (store $l(\cdot)$ once). DP state space contains **diamonds** (keep the better of two partial solutions) — contrast with the **tree** of B&B.

### 2.5 Shortest paths in DAGs ⭐

Process vertices in **topological order**; a shortest path to $v_i$ can use only $v_1..v_{i-1}$.

```
l(v1):=0; l(vi):=inf
for i:=2 to n:  for each edge (vj,vi):  relax
```
**Complexity $O(|V|+|E|)$** — linear (simplified Bellman–Ford; acyclic ⇒ no negative-cycle worries).

### 2.6 Floyd–Warshall [1962] — all pairs ⭐⭐

```
l[i][j] := c(i,j) (inf if no edge), l[i][i]:=0
for k:=1 to n:
  for i:=1 to n:
    for j:=1 to n:
      if l[i][j] > l[i][k]+l[k][j]: l[i][j]:=l[i][k]+l[k][j]; p[i][j]:=p[k][j]
```

- $l^k_{ij}$ = shortest $i$–$j$ path using only intermediate nodes $\{1..k\}$; recurrence $l^k_{ij}=\min(l^{k-1}_{ij},\,l^{k-1}_{ik}+l^{k-1}_{kj})$.
- **Complexity $O(n^3)$**; best known for all-pairs without negative cycles.
- **Negative cycle iff** some $l_{ii}<0$. **Johnson's algorithm** (Dijkstra+Bellman–Ford reweighting) is better for sparse graphs: $O(|V|\,|E|\log|V|)$.

---

## 3. Network Flows

> Official subpoints: max flow & min cut; Ford–Fulkerson; feasible flow with balances; min-cost flow & cycle-canceling; formulations via flows; max cardinality matching. (*[L4]*)

### 3.1 Network & flow ⭐

- **Network** *(síť)* = 5-tuple $(G,l,u,s,t)$: digraph $G$, lower/upper capacities $l,u:E\to\mathbb{R}^+_0$, source $s$, sink $t$.
- **Flow** $f:E\to\mathbb{R}^+_0$ satisfies **Kirchhoff's law** *(zákon zachování)* at every node except $s,t$: $\sum_{e\in\delta^-(v)}f(e)=\sum_{e\in\delta^+(v)}f(e)$.
- **Feasible flow** *(přípustný tok)*: $l(e)\le f(e)\le u(e)$. (May not exist when some $l(e)>0$.)

### 3.2 Maximum flow & Ford–Fulkerson [1956] ⭐⭐

**Max flow** = maximize $\sum_{\delta^+(s)}f-\sum_{\delta^-(s)}f$. LP-formulable (so polynomial).

**Augmenting path** *(zlepšující cesta)* $s\to t$ ignoring orientation: **forward** arc with $f(e)<u(e)$, **backward** arc with $f(e)>l(e)$. Capacity $\gamma=\min(\min_{\text{fwd}}u(e)-f(e),\ \min_{\text{bwd}}f(e)-l(e))$.

```
1. find any feasible flow f
2. find augmenting path P (labeling procedure); if none → STOP
3. augment by γ along P (forward +γ, backward −γ); goto 2
```

- **Backward arcs are essential** — without them you can't reach the maximum.
- **Labeling procedure** finds $P$ by marking from $s$: mark $j$ from marked $i$ if forward $f<u$ or backward $f>l$. Reaching $t$ ⇒ path; stuck ⇒ none.

**Complexity:** an augmenting path takes $O(|E|)$. Integer capacities: each iteration adds ≥1, so $O(|E|\cdot F)=O(|E|^2 U)$ ($U=\max u$, $F=$ max-flow value). **Edmonds–Karp** (always take **shortest** augmenting path via BFS) → $O(|V|\,|E|^2)$, independent of capacities. (Non-integer capacities + bad choices: FF may not even terminate.)

⭐ **Why Edmonds–Karp is $O(|V||E|^2)$** — Berezovskyj's follow-up (2019) that the student didn't know: with BFS augmenting paths, the residual distance $d(s,v)$ **never decreases** during the run. Each augmentation **saturates** at least one bottleneck edge; a saturated edge $(u,v)$ can only reappear in the residual graph after flow is pushed back, which requires $d(u)$ to have grown by $\ge 2$. Distances are bounded by $|V|$ ⇒ each edge is critical $O(|V|)$ times ⇒ $O(|V||E|)$ augmentations × $O(|E|)$ per BFS $= O(|V||E|^2)$. (His other favorite: "how big an instance in 1 hour at $10^8$ ops/s" — set $nm^2 \approx 3.6\cdot10^{11}$, e.g. sparse $m\approx n$ gives $n \approx 7000$.)

### 3.3 Minimum cut & max-flow = min-cut ⭐⭐

**Cut** *(řez)* $\delta(A)$ given by vertex set $A$ with $s\in A,\ t\notin A$; **capacity** $C(A)=\sum_{e\in\delta^+(A)}u(e)-\sum_{e\in\delta^-(A)}l(e)$.

**Max-flow Min-cut theorem (Ford–Fulkerson 1956):** value of max flow = capacity of min cut (follows from LP duality). When the labeling procedure halts, the **labeled vertices form a min cut**; then $f(e)=u(e)$ on $\delta^+(A)$ and $f(e)=l(e)$ on $\delta^-(A)$.

**Integrality theorem (Dantzig–Fulkerson):** integer capacities ⇒ an integer-valued max flow exists (from TU of the incidence matrix; or: $\gamma$ stays integer in FF).

### 3.4 Feasible flow with balances ⭐ (multiple sources/sinks)

**Instance** $(G,l,u,b)$ with **balances** $b:V\to\mathbb{R}$, $\sum_v b(v)=0$ ($b>0$ supply, $b<0$ demand). **Goal:** decide existence of $f$ with $\sum_{\delta^+(v)}f-\sum_{\delta^-(v)}f=b(v)$.

**Reduction to max flow:** add super-source $s$ with edges $(s,v),u=b(v)$ for $b(v)>0$; super-sink $t$ with edges $(v,t),u=-b(v)$ for $b(v)<0$; solve max flow; feasible **iff** all $s$/$t$ edges saturate.

⭐⭐ **Initial feasible flow with $l(e)>0$** — Hanzálek's favorite ("za nenulový počáteční tok byl hodně šťastný", asked 2012, 2013, 2018). FF needs *some* feasible flow to start; with all $l(e)=0$ take $f\equiv0$, but with $l(e)>0$ zero is infeasible. Full recipe to deliver at the board:

1. **Close the network:** add a circulation arc $t\to s$ with $u=\infty$ — now Kirchhoff must hold at *every* vertex (a circulation problem).
2. **Shift out the lower bounds:** substitute $f(e) = f'(e) + l(e)$, i.e. pretend the compulsory $l(e)$ units already flow. New bounds: $0 \le f'(e) \le u(e) - l(e)$. The pre-pushed $l$-flow violates Kirchhoff at vertices where in-lower-bounds ≠ out-lower-bounds; record the imbalance as a **balance** $b(v)=\sum_{e\in\delta^-(v)}l(e)-\sum_{e\in\delta^+(v)}l(e)$.
3. **Solve Feasible Flow with Balances** (§3.4 super-source/super-sink construction) as a max-flow problem with zero lower bounds — FF can start from $f'\equiv0$ there.
4. If all super-source/sink edges saturate → $f(e) = f'(e) + l(e)$ is a feasible initial flow; otherwise **no feasible flow exists**.

Chain to remember: *nonzero lower bounds → circulation + substitution → feasible flow with balances → max flow from zero.*

### 3.5 Minimum cost flow & cycle-canceling ⭐⭐

**Instance** $(G,l,u,c,b)$: edge costs $c:E\to\mathbb{R}$, balances $b$. **Goal:** feasible $f$ minimizing $\sum_e c(e)f(e)$. LP-formulable.

Very general: **max flow** reduces to it (arc $t\to s$, cost $-1$, others $0$); **shortest path** reduces ($b(s)=1,b(t)=-1$, $c=$ weights); **Chinese postman** reduces ($l(e)=1,u=\infty$; closed tour using each edge ≥ once; Eulerian iff every vertex has in-degree = out-degree).

**Cycle-canceling algorithm** *(rušení cyklů)*:
```
1. find feasible flow f (Feasible Flow with Balances)
2. build residual graph G_f; find a negative-cost cycle C (Bellman-Ford). none → STOP
3. γ := min_{e∈C} u_f(e); augment f along C by γ; goto 2
```
**Residual graph** $G_f$: forward residual $u_f(i,j)=u(i,j)-f(i,j)$ cost $c$; backward $u_f(j,i)=f(i,j)-l(i,j)$ cost $-c$; keep edges with $u_f>0$.

**Complexity** $O(|V|\,|E|^2\,C\,U)$ ($C=\max|c|,U=\max u$); each cancel improves cost by ≥1. Improvement: **Minimum-Mean Cycle-Canceling** $O(|V|^2|E|^3\log|V|)$ (most-negative cycle is NP-hard, so use min *mean* cycle).

### 3.6 Multicommodity flow (*[L4]*)

Commodities $M$, flow $f^m(e)$; shared capacity $l(e)\le\sum_m f^m(e)\le u(e)$. LP-solvable in polynomial time, but the matrix is **not TU** (shared capacity) ⇒ integer solution not guaranteed; in practice use ILP.

### 3.7 Matching ⭐⭐ (*[L4]*)

**Matching** *(párování)* $P\subseteq E$ (undirected) = no two edges share a node; **perfect** *(perfektní)* if every node is covered. Problems: (a) max-cardinality, (b) max-cardinality in **bipartite**, (c) min-weight (max-cardinality) matching, (d) **min-weight perfect matching in complete bipartite = Assignment problem** *(přiřazovací úloha)*. All polynomial.

- **(a) Augmenting-path theorem:** $M$ is maximum **iff** there is no $M$-augmenting path (an $M$-alternating path with both endpoints uncovered). Algorithm: repeatedly find an augmenting path and flip it.
- **(b) Bipartite via max flow:** add $s\to X$, $Y\to t$, all caps 1; max flow = max matching.
- **(d) Assignment via min-cost flow**, or the **Hungarian algorithm** *(maďarský algoritmus)*:

**Hungarian algorithm** — $G$ complete bipartite, $|X|=|Y|=n$, cost matrix $c_{ij}\ge0$.
Idea: **node potentials** $p(v)$; transformed cost $c^p_{ij}=c_{ij}-p_i-p_j$. A **feasible rating** keeps $c^p_{ij}\ge0$; the **equality graph** $G^p$ keeps only zero-cost edges. **Theorem:** if $G^p$ has a perfect matching $P$, $P$ is optimal (its $c^p$-cost is 0, none cheaper).
```
1. p_i := min_j c_ij  (i∈X);  p_j := min_i (c_ij - p_i)  (j∈Y)
2. build equality graph G^p (edges with c_ij - p_i - p_j = 0)
3. find max-cardinality matching P in G^p; if perfect → DONE
4. find A⊆X, B⊆Y (B incident to A in G^p) with |A|>|B|;
   d := min_{i∈A, j∉B} (c_ij - p_i - p_j);  p_i+=d (i∈A); p_j-=d (j∈B); goto 2
```
**Complexity $O(n^4)$** (basic version; $O(n^3)$ improved).

---

## 4. Knapsack Problem

> Official subpoints: approximation algorithm, dynamic programming, approximation scheme. (*[L5]*)

### 4.1 Definitions ⭐

- **0/1 Knapsack** *(problém batohu)*: items with costs $c_i$, weights $w_i$, capacity $W$. Find $S\subseteq\{1..n\}$ with $\sum_S w_j\le W$ maximizing $\sum_S c_j$. **NP-hard** (one of the "easiest"); only **weakly** NP-hard.
- **Fractional Knapsack** *(zlomkový batoh)*: $x_j\in\langle0,1\rangle$. **Polynomial.**

### 4.2 Fractional — greedy (Dantzig 1957) ⭐

Sort by **density** $c_1/w_1\ge\dots\ge c_n/w_n$; fill whole items until the first that overflows, take a **fraction** of it. **Complexity $O(n\log n)$** (sorting; $O(n)$ via weighted median).

### 4.3 2-approximation for 0/1 Knapsack ⭐

Drop items with $w_j>W$. Output the **better of** $\{1,\dots,h-1\}$ (densest prefix that fits) **and** $\{h\}$ (the single overflowing item), where $h=\min\{j:\sum_{i\le j}w_i>W\}$. Since $\sum_{i=1}^h c_i$ is an upper bound on OPT, the better half $\ge \tfrac12$OPT. **$r=2$, complexity $O(n)$.** ($r$-approx for max: $J^A(I)\ge\frac1r J^*(I)$, $r\ge1$; relative deviation $\epsilon=r-1$.)

### 4.4 Dynamic programming — pseudopolynomial ⭐⭐

$x^j_k$ = minimum weight achievable with **exactly cost $k$** using items $\{1..j\}$:
$$x^j_k=\min\big(x^{j-1}_k,\ x^{j-1}_{k-c_j}+w_j\big)\quad(\text{keep the lighter option}).$$
Take the largest $k$ with $x^n_k\le W$; backtrack via stored choices $s^j_k$. **Complexity $O(nC)$** with $C=\sum_j c_j$ (or $O(nW)$ if you index by weight) — **pseudopolynomial** (polynomial in the value $C$, not its encoding). State space has **diamonds** (DP, not a tree).

### 4.5 FPTAS — approximation scheme ⭐⭐

Knapsack admits a **fully polynomial-time approximation scheme**: arbitrary $\epsilon>0$.
```
1. run 2-approx → S1 with cost c(S1)
2. t := max(1, ε·c(S1)/n);  c'_j := floor(c_j / t)   # scale & round costs
3. run DP on scaled costs, upper bound C := 2c(S1)/t → S2
4. output the better of S1, S2
```
Choosing $t=\epsilon c(S_1)/n$ gives a **$(1+\epsilon)$-approximation**. **Complexity $O(nC)=O(n^2\cdot\frac1\epsilon)$** — polynomial in both $n$ and $1/\epsilon$. (If $\epsilon$ tiny, $t=1$ ⇒ exact DP.)

---

## 5. Traveling Salesman Problem (TSP)

> Official subpoints: double-tree & Christofides for metric TSP; local search $k$-OPT. (*[L6]*)

### 5.1 Definitions & hardness ⭐⭐

- **TSP (symmetric)** *(problém obchodního cestujícího)*: complete undirected $K_n$ ($n\ge3$), weights $c:E\to\mathbb{R}^+$. Find min-weight **Hamiltonian circuit** *(hamiltonovská kružnice)*. Asymmetric TSP ⇒ digraph.
- Related decision problem **HC (Hamiltonian circuit)** is **NP-complete**.
- **TSP is strongly NP-hard:** NP-hard even with all distances $\in\{1,2\}$ (reduce from HC: edge → 1, non-edge → 2; HC exists ⇔ optimal tour $=n$). ⇒ **no pseudopolynomial algorithm** unless P=NP.
- ⭐ **No polynomial $r$-approximation for general TSP** (any $r\ge1$) unless P=NP. (Reduce HC: non-edge weight $=2+(r-1)n$; an $r$-approx would distinguish "tour $=n$" from "$\ge rn+1$", solving HC.)

### 5.2 Metric TSP ⭐

Add the **triangle inequality** $c(i,j)+c(j,k)\ge c(k,i)$. Still strongly NP-hard (weights 1,2 preserve $\triangle$), **but constant-factor approximations exist**. (Adding a constant $h$ to every edge makes any instance metric but does **not** help approximate non-metric TSP.)

- **Nearest neighbor** *(nejbližší soused)*: greedy, $O(n^2)$ — **not** an approximation algorithm (no guarantee).

### 5.3 Double-tree algorithm — 2-approximation ⭐⭐

```
1. minimum spanning tree T of K_n
2. double every edge → Eulerian multigraph; find Eulerian walk L
3. shortcut L to a Hamiltonian circuit H (skip already-visited nodes)
```
**$r=2$, $O(n^2)$.** Proof: $c(L)\ge c(H)$ (shortcuts don't lengthen, by $\triangle$); $\text{OPT}\ge c(T)$ (removing a tour edge gives a spanning tree); $c(L)=2c(T)$. ⇒ $2\text{OPT}\ge 2c(T)=c(L)\ge c(H)$.

### 5.4 Christofides' algorithm [1976] — 3/2-approximation ⭐⭐

```
1. minimum spanning tree T
2. W := odd-degree vertices of T  (|W| even)
3. minimum-weight perfect matching M on W (in K_n)
4. Eulerian walk L on (V, E(T)∪M)   (all degrees now even)
5. shortcut L to Hamiltonian circuit H
```
**$r=3/2$, $O(n^3)$** (matching dominates). Proof adds: $\text{OPT}/2\ge c(M)$ (the optimal tour splits into two perfect matchings on $W$; $M$ is the cheaper), and $c(L)=c(T)+c(M)\le \text{OPT}+\tfrac12\text{OPT}$.

> Per slides ([L6]/KV) these bounds "cannot be improved" within these methods. *Context note: the metric-TSP $3/2$ barrier was slightly beaten in 2021 (Karlin–Klein–Oveis Gharan) — beyond the course.*

### 5.4b Exact TSP algorithms ⭐ (2022 exam question: "best known 0-error algorithms, how large instances, their essence, complexity")

- **Brute force:** $(n-1)!/2$ tours — dead by $n\approx15$.
- **Held–Karp dynamic programming** [1962]: state = (subset $S\ni1$ of visited cities, last city $j\in S$); $D(S,j)=\min_{i\in S\setminus\{j\}} D(S\setminus\{j\},i)+c_{ij}$. **$O(n^2 2^n)$ time, $O(n2^n)$ memory** — the best *guaranteed* bound known; practical to $n\approx20$–$25$ (memory dies first).
- **Branch & cut** (= B&B + cutting planes): solve the LP relaxation of the DFJ formulation (§1.3), add violated subtour-elimination (and comb) inequalities lazily, branch on fractional variables. Essence: **tight LP lower bounds prune the B&B tree**. This is **Concorde**, which has solved real instances with **tens of thousands of cities** (record: 85,900 — pla85900, 2006) — exponential worst case, spectacular in practice.
- Components to explain if asked "jednotlivé části": LP relaxation (lower bound, polynomial per node), separation oracle for violated cuts (min-cut computation), branching, plus a good initial tour from heuristics (k-OPT / Lin–Kernighan) as the upper bound.

### 5.5 Local search — $k$-OPT ⭐

Start from any tour; repeatedly **remove $k$ edges and reconnect** with other edges if it shortens the tour.
- **2-opt:** remove 2 edges → exactly one reconnection (reverses a segment).
- **3-opt:** remove 3 edges → several reconnections (incl. ones reversing no segment).
- **4-opt:** e.g. the "double bridge" move.
Heuristic (no guarantee), but one of the best in practice. Real-life extensions: CVRP, VRPTW, VRPPD.

---

## 6. Scheduling

> Official subpoints: notation; one resource (Bratley, Horn); parallel identical (list scheduling, DP); project scheduling with temporal constraints (relative-order & time-indexed ILP). (*[L7]*)

### 6.1 Terminology & Graham notation $\alpha|\beta|\gamma$ ⭐⭐

- **Task** *(úloha)* params: release time $r_j$, processing time $p_j$, due date $d_j$, **deadline** $\tilde d_j$. Variables: start $s_j$, completion $C_j$, **lateness** $L_j=C_j-d_j$, **tardiness** $T_j=\max(C_j-d_j,0)$, flow $F_j=C_j-r_j$.
- **$\alpha$ — resources:** $1$ single; $P$ parallel identical; $Q$ uniform; $R$ unrelated; $O$ open-shop, $F$ flow-shop, $J$ job-shop (dedicated); $PS$ project scheduling. $\alpha_2$ = number ($\emptyset$ arbitrary, $2$, $m{,}R$).
- **$\beta$:** `pmtn` preemption; `prec`/`in-tree`/`chain`/`temp` precedence; $r_j$; processing-time restrictions; $d_j,\tilde d_j$; etc.
- **$\gamma$ — criterion:** $C_{\max}$ makespan, $\sum C_j$, $\sum w_jC_j$, $L_{\max}$, or $\emptyset$ (decision/feasibility).
- **Off-line** (task set known) vs on-line. Result = a **schedule** (Gantt chart).

### 6.2 One resource — $C_{\max}$ & Bratley ⭐⭐

Easy cases: $1|prec|C_{\max}$ (topological order), $1||C_{\max}$, $1|r_j|C_{\max}$ (nondecreasing $r_j$), $1|\tilde d_j|C_{\max}$ (EDF, nondecreasing $\tilde d_j$). **$1|r_j,\tilde d_j|C_{\max}$ is strongly NP-hard** (reduction from **3-Partition**; polynomial only if all $p_j=1$).

**Bratley's algorithm** = B&B for $1|r_j,\tilde d_j|C_{\max}$. Node label = (task order)/(completion of last task). Pruning:
1. **(i) deadline miss:** prune a node exceeding any $\tilde d_j$, and all its siblings (the critical task must be scheduled anyway).
2. **(ii) decomposition by idle waiting:** if $C_i$ of the last task $\le r_j$ of all unscheduled tasks, no need to backtrack above this node — it becomes a new root.
3. **Optimality test — BRTP** (Block with Release-Time Property): a block where the first task starts at its $r$ and all run without idle waiting, with $r_{[1]}\le r_{[i]}$. **If a BRTP exists, the schedule is optimal** (search stops). BRTP is sufficient, not necessary; otherwise tighten deadlines to $C_{\max}-\epsilon$ for bounding.

A **position-based ILP** ($x_{iq}=1$ iff task $i$ at position $q$) is also given for $1|r_j,\tilde d_j|C_{\max}$.

### 6.3 One resource — $\sum w_jC_j$ ⭐

- $1||\sum C_j$: **SPT** (Shortest Processing Time first). $1||\sum w_jC_j$: **Weighted SPT** (nondecreasing $p_j/w_j$).
- $1|r_j|\sum C_j$, $1|prec|\sum C_j$, $1|\tilde d_j|\sum w_jC_j$: **NP-hard**.
- **$1|prec|\sum w_jC_j$ via B&B+LP:** ILP with $x_{ij}=1$ iff $T_i$ precedes $T_j$; precedence ($x_{ij}\ge e_{ij}$), completeness ($x_{ij}+x_{ji}=1$), no-cycle ($1\le x_{ij}+x_{jk}+x_{ki}\le2$); objective $\sum_j w_j\sum_i p_i x_{ij}$. Relax integrality → LP lower bound on "remaining work" for bounding.

### 6.4 One resource — $L_{\max}$: EDD, Horn/EDF ⭐⭐

- **$1||L_{\max}$ → EDD** (Earliest Due Date first), $O(n\log n)$. Optimality by exchange argument (swap an out-of-order adjacent pair, $L_{\max}$ never increases).
- **$1|pmtn,r_j|L_{\max}$ → Horn's algorithm** (preemptive EDD): at every moment run the **ready** task with the earliest due date; preempt when a new task is released or the current finishes.
```
while tasks remain:
  t1 := current time (min release among remaining)
  ready := tasks with r_j ≤ t1; pick T_k in ready with min d_k    # EDD
  run T_k until t2 = next release or completion; preempt if needed
```
- **$1|pmtn,r_j,d_j=\tilde d_j|L_{\max}$ = EDF** (Earliest Deadline First): same algorithm; minimizes $L_{\max}$ **and decides schedulability** ($L_{\max}\le0$).
- **With precedence** $1|pmtn,prec,r_j|L_{\max}$: **Chetto–Silly–Bouchentouf** modifies $r'_j=\max(r_j,\max_h r'_h+p_h)$ and $\tilde d'_j=\min(\tilde d_j,\min_k \tilde d'_k-p_k)$, then runs EDF on the now-independent tasks.

### 6.5 Parallel identical resources — $C_{\max}$ ⭐⭐

- **$P2||C_{\max}$ NP-hard** (2-Partition); **$P||C_{\max}$ NP-hard**; **$P|prec|C_{\max}$ NP-hard**.
- **$P|pmtn|C_{\max}$ easy — McNaughton, $O(n)$:** $C^*_{\max}=\max\big(\max_i p_i,\ \tfrac1R\sum_i p_i\big)$; fill resources up to $C^*_{\max}$, wrapping each over-long task to the next resource (≤1 preemption per task).
- **List Scheduling (LS)** for $P|prec|C_{\max}$ (and $P||C_{\max}$): whenever a resource is free, assign the first **available** task from list $L$. Graham 1966: **$r_{LS}=2-\tfrac1R$** for arbitrary list. Exhibits **anomalies** (makespan can *increase* when $p_i$ decrease, precedences removed, or $R$ grows).
- **LPT (Longest Processing Time first)** for $P||C_{\max}$: LS with $L$ sorted by nonincreasing $p_i$. **$r_{LPT}=\tfrac43-\tfrac1{3R}$**, $O(n\log n)$.
- **DP — Rothkopf**, $P||C_{\max}$ pseudopolynomial: $x_i(t_1,..,t_R)=1$ iff tasks $1..i$ fit with resource $v$ busy on $\langle0,t_v\rangle$; recurrence ORs over the $R$ resources. **Complexity $O(n\cdot UB^R)$** — pseudopoly for **fixed $R$** (weakly NP-hard); for $R=\text{poly}(n)$ the problem is **strongly NP-hard** (3-Partition).
- ⭐ **"How would you solve $P||C_{\max}$ optimally?"** (Šůcha 2023) — expected answer: **ILP**. Variables $x_{ij}\in\{0,1\}$ ($=1$ iff task $i$ runs on resource $j$) and makespan $C_{\max}\ge0$: $\min C_{\max}$ s.t. $\sum_j x_{ij}=1$ for every task (assigned exactly once) and $\sum_i p_i\,x_{ij}\le C_{\max}$ for every resource (no machine exceeds the makespan). He then probes the variables — be ready to name them and both constraint families. (Other exact options: Rothkopf DP above, B&B.)
- **$P|pmtn,prec|C_{\max}$ — Muntz–Coffman level algorithm:** assign by task **level** (longest remaining path to a sink), splitting capacity; exact for $P2|pmtn,prec|C_{\max}$ and $P|pmtn,forest|C_{\max}$, else **$r=2-\tfrac2R$**, $O(n^2)$.

### 6.6 Project Scheduling with temporal constraints ⭐⭐

**Temporal constraint** *(časové omezení)* = each edge $i\to j$ with length $l_{ij}$ encodes $s_i+l_{ij}\le s_j$. Covers normal precedence ($l_{ij}=p_i$), min/max time-lags ($l_{ij}>p_i$, or $0<l_{ij}<p_i$), and **relative deadlines** ($l_{ij}<0$). A finite $l_{ij}\ge0$ and $l_{ji}<0$ together = **relative time window**.
- **Absence of a positive cycle** in $G$ is necessary for schedulability; with **unlimited resources** it is also sufficient and a schedule is found in polynomial time by **longest paths** ($s_j\ge\max_i l_{ij}$) or LP.
- **$PS1|temp|C_{\max}$** (single resource) and **$PSm,1|temp|C_{\max}$** ($m$ dedicated unit-capacity resources) are **NP-hard** (reduction from Bratley's $1|r_j,\tilde d_j|C_{\max}$).

**Two ILP models** (both: temporal + resource constraints):

- **Time-indexed** *(s časovým indexem)*: $x_{it}=1$ iff $s_i=t$ (integer processing times). Resource constraint $\sum_i\sum_{k=t-p_i+1}^{t}x_{ik}\le1$ for all $t$. Size grows with $UB$. (+) extends easily to parallel identical resources; (+) few constraints; (−) grows with $UB$.
- **Relative-order** *(s relativním uspořádáním)*: $x_{ij}=1$ iff $T_i$ precedes $T_j$ (real processing times). Resource constraint via big-$M$: $p_j\le s_j-s_i+UB\cdot x_{ij}\le UB-p_i$. (+) size independent of $UB$; (−) many constraints ($\sim n^2$).
- For **$PSm,1|temp|C_{\max}$** the resource constraint is posted only for task pairs on the **same** resource ($a_i=a_j$).

If only the **order** $x_{ij}$ is fixed (e.g. from a heuristic), feasible start times follow from an LP with temporal constraints only. A violated partial schedule does not always mean the order is infeasible.

---

## 7. Constraint Satisfaction Problem (CSP)

> Official subpoint: CSP; AC-3 algorithm. (*[L8]*)

### 7.1 Definitions ⭐⭐

- **CSP** = triple $(X,D,C)$: variables $X=\{x_1..x_n\}$, finite domains $D=\{D_1..D_n\}$, constraints $C=\{C_1..C_t\}$. A constraint $C_i=(S_i,R_i)$: scope $S_i\subseteq X$ and relation $R_i$ over their domains. **CSP is NP-complete.**
- **Solution** = complete assignment satisfying all constraints (decision problem). **CSOP** adds objective $f(X)$ (search continues to optimum, e.g. via B&B). **CP** = Constraint Programming = constraint satisfaction + constraint solving.
- **Model + Solver** paradigm: model = variables/domains/constraints; solver = **propagate + search**. Propagation alone is incomplete; search splits into subproblems, then propagate again.

### 7.2 Arc consistency & AC-3 ⭐⭐

- **Binary CSP** → digraph (arc per ordered constrained pair). **Arc $(x_i,x_j)$ is arc-consistent** iff every $a\in D_i$ has some $b\in D_j$ satisfying the constraint. AC is **directional** ($AC(x_i,x_j)\not\Rightarrow AC(x_j,x_i)$). A CSP is AC iff all arcs are.
- **REVISE$(D_i,D_j)$:** delete every $a\in D_i$ with no support $b\in D_j$.
- **AC-3** maintains a queue of arcs to revise; after a domain shrinks, re-enqueue the arcs that point **into** it:
```
Q := all arcs
while Q not empty:
  (xk,xm) := pop Q
  if REVISE(Dk, Dm):
    if Dk == ∅: return FAIL
    Q := Q ∪ { (xi,xk) : (xi,xk)∈E, i≠m }
```
**Complexity $O(e\,d^3)$** (classic, Mackworth–Freuder 1985; $e$ = #arcs, $d$ = max domain size); improvable to **$O(e\,d^2)$** (AC-4/AC2001 — optimal). **AC is incomplete** (an AC, non-empty CSP can still be infeasible).

### 7.3 Stronger / weaker consistencies (*[L8]*)

- **Path consistency (PC):** for every pair $(a,b)$ on $x_1,x_m$ consistent with their binary constraint, there is an assignment of intermediate vars making all neighbouring constraints hold. PC ⇒ AC; **CSP is PC iff all length-2 paths are PC**; still incomplete; worse strength/efficiency ratio than AC.
- **Generalized AC (GAC):** AC for one constraint over **>2** variables; value $a$ of $x_i$ is GAC w.r.t. $C_j$ if the other vars have supporting values. Checking GAC is NP-hard for some constraints.
- **Bounds consistency (BC):** GAC required only for the **min/max** domain values (domains seen as intervals). Variants $BC_\mathbb{Z}$ (integer interval, ignores holes) and $BC_\mathbb{R}$.

### 7.4 Backtracking search ⭐

DFS of a search tree; **backtrack when some domain empties**; stop at a solution or when the tree is exhausted. **Branching** must be exhaustive + mutually exclusive (e.g. binary choice $x=v$ / $x\ne v$).

**Look-ahead strategies** (propagation during search):
- **Chronological backtracking** — no propagation (check only fully-instantiated constraints).
- **Forward checking** — maintain AC on constraints with exactly **one** uninstantiated variable.
- **Maintaining Arc Consistency (MAC)** — full (G)AC after each assignment; **Maintaining Bounds Consistency** similarly.

**Restarts + nogood recording:** restart to revisit early choices; record **nogoods** (forbidden partial assignments) so the search after restart doesn't repeat work.

### 7.5 Global constraints — Alldifferent ⭐

A **global constraint** captures structure for efficient specialized propagation. **`alldifferent`$(x_1..x_n)$** (all pairwise $\ne$) instead of $(n^2-n)/2$ inequalities; propagated via **bipartite matching** (variables ↔ values) — Hall's condition, GAC by Régin's algorithm. Other globals: scheduling (edge-finder), bin-packing, cycle/clique on graphs. (Course capstone: model **Sudoku** with 27 `alldifferent` constraints over $9\times9$ domain-$\{1..9\}$ variables.)

---

## Key results cheat-sheet ⭐

| Problem | Status | Algorithm(s) & complexity |
|---|---|---|
| LP / **ILP** | poly / **NP-hard** | simplex/interior-point; **B&B**; TU ⇒ integral LP |
| Shortest path (≥0) | poly | **Dijkstra** $O(m+n\log n)$ |
| Shortest path (neg., no neg-cycle) | poly | **Bellman–Ford** $O(nm)$; DAG $O(n+m)$ |
| All-pairs SP | poly | **Floyd–Warshall** $O(n^3)$; Johnson $O(\lvert V\rvert\,\lvert E\rvert\log\lvert V\rvert)$ |
| Max flow | poly | **Ford–Fulkerson** $O(\lvert E\rvert^2U)$; **Edmonds–Karp** $O(\lvert V\rvert\,\lvert E\rvert^2)$; max-flow=min-cut |
| Min-cost flow | poly | **Cycle-canceling** $O(\lvert V\rvert\,\lvert E\rvert^2CU)$; min-mean-cycle $O(\lvert V\rvert^2\lvert E\rvert^3\log\lvert V\rvert)$ |
| Bipartite matching | poly | via max flow; **Assignment**: Hungarian $O(n^4)$ |
| **Knapsack** | **weakly NP-hard** | greedy frac. $O(n\log n)$; 2-approx $O(n)$; **DP** $O(nC)$; **FPTAS** $O(n^2/\epsilon)$ |
| **TSP general** | **strongly NP-hard** | no poly $r$-approx unless P=NP |
| **Metric TSP** | **strongly NP-hard** | **double-tree** 2-approx $O(n^2)$; **Christofides** 3/2 $O(n^3)$; $k$-OPT |
| $1\mid\mid L_{\max}$ | poly | **EDD** $O(n\log n)$ |
| $1\mid pmtn,r_j\mid L_{\max}$ | poly | **Horn/EDF** |
| $1\mid r_j,\tilde d_j\mid C_{\max}$ | **strongly NP-hard** | **Bratley** B&B (BRTP test) |
| $1\mid\mid\sum w_jC_j$ | poly | **Weighted SPT** |
| $P\mid pmtn\mid C_{\max}$ | poly | **McNaughton** $O(n)$ |
| $P\mid prec\mid C_{\max}$ | **NP-hard** | **List Scheduling** $r=2-1/R$; **LPT** $r=4/3-1/3R$; Rothkopf DP $O(n\,UB^R)$ |
| $PS1\mid temp\mid C_{\max}$ | **NP-hard** | time-indexed / relative-order **ILP** |
| **CSP** | **NP-complete** | propagation (**AC-3** $O(ed^3)$) + backtracking search |

---

## References

- **Lecture slides (2026):** Hanzálek (+ Šůcha), *Course KO* — `materials/1_Basic_KO.pdf` … `8_CP_KO.pdf`.
- **Course page:** [cw.fel.cvut.cz/wiki/courses/ko/start](https://cw.fel.cvut.cz/wiki/courses/ko/start).
- **Textbook:** B. Korte, J. Vygen, *Combinatorial Optimization: Theory and Algorithms*, Springer (4th ed., 2008).
- **Flows:** Ahuja, Magnanti, Orlin, *Network Flows*, 1993.
- **TSP:** Applegate, Bixby, Chvátal, Cook, *The Traveling Salesman Problem: A Computational Study*, 2007.
- **AC-3 complexity** cross-checked: classic $O(ed^3)$ (Mackworth–Freuder 1985), optimal $O(ed^2)$ (Zhang–Yap / Bessière). **Edmonds–Karp $O(VE^2)$**, **LPT $4/3-1/3m$**, **List Scheduling $2-1/m$** (Graham 1966) — confirmed against standard literature.

---

## Frequently asked at the exam

See `questions.md` in this folder for past committee questions cross-referenced to the sections above (to be populated from `../../frequently_asked_questions/`).

