# BE4M35KO — Combinatorial Optimization (KO) — Frequently Asked Questions

Past-exam questions specific to **BE4M35KO**, condensed from `../../frequently_asked_questions/oi_wiki_committee_archive.md` (29 entries), cross-referenced to `summary.md`.

**Topic frequency:**

| Topic | Times asked | Summary section |
|---|---:|---|
| ILP — formulation, TSP via ILP, logical constraints | 15 | §1 |
| Network flows / cuts / Ford-Fulkerson | 11 | §3 |
| TSP — approximation, Christofides, k-OPT, exact | 10 | §5 |
| CSP & AC-3 | 9 | §7 |
| Shortest paths | 6 | §2 |
| Scheduling (Bratley / Horn / P\|\|Cmax) | 5 | §6 |

## Representative questions (with examiner expectations)

### ILP ⭐⭐

**Q2 — Šůcha 2023:** ILP formulation + TSP→ILP. Follow-up: "how else could TSP be defined?" → wanted **DFJ / lazy subtour constraints** (the homework). → §1.3 incl. MTZ vs DFJ table.

**Q4 — Werner 2018:** what is ILP, complexity class, LP relaxation, **totally unimodular matrix definition + example problem**. → §1.1, §1.4.

**Q5/Q7/Q10/Q13 — Hanzálek & Šůcha 2012–2016 (recurring formula):** ILP definition → **logical constraints, implication, OR via big-M, fixed costs (fixní náklady)** → formulate TSP. Q13 follow-ups: define two problems via ILP on the spot; "what do the numbers in $A$ and $b$ mean" (rows = constraints, columns = variables, $a_{ij}$ = coefficient of variable $j$ in constraint $i$). → §1.3, §1.3b catalog, §1.3c toolbox.

### Network flows ⭐⭐

**Q18 — Hanzálek 2018:** max flow, Ford–Fulkerson + complexity, minimum cut, **how to find the initial feasible flow**. → §3.2–3.4.

**Q19/Q22 — Hanzálek 2012/2013:** flows, cuts, LP formulation, FF, **nonzero lower bounds / initial feasible flow** ("za nenulový počáteční tok byl hodně šťastný"). Vyskočil interjects with "does it depend on #nodes/#edges/flow value?" → know which complexities depend on $U$ (basic FF) vs not (Edmonds–Karp). → §3.2, §3.4 recipe.

**Q24 — Berezovskyj 2019:** max flow ↔ bipartite matching relation (unweighted via flow; weighted → assignment/Hungarian); **why Edmonds–Karp is $O(nm^2)$** (he expects the saturated-edge/non-decreasing-distance argument); instance size solvable in 1 hour at $10^8$ ops/s. → §3.2 (EK proof sketch), §3.7.

### TSP ⭐

**Q9 — Hanzálek 2013:** TSP via ILP + **approximation algorithms with proofs** (double-tree, Christofides). → §5.3–5.4.

**Q12 — Hanzálek 2012:** show metric TSP hardness (reduction from HC, weights 1/2); **does an r-approximation exist for general TSP? Prove** (the second HC reduction with weight $rn+1$). → §5.1–5.2 — note these are *two different* HC reductions; don't mix them up.

**Q23 — 2022:** best exact (0-error) TSP algorithms, instance sizes, essence, complexity of the parts. → §5.4b (Held–Karp $O(n^22^n)$; branch-and-cut/Concorde with DFJ cuts).

### CSP

**Q1 — Faigl 2023:** CSP + AC-3. Winning strategy from the report: compare with ILP, formal definition, **own prepared example (Sudoku)**, AC-3 pseudocode + worked example — filled the slot, no follow-ups. → §7.1–7.2, §7.5.

### Shortest paths

**Q8 — Demlová 2014:** define before algorithms; Floyd–Warshall matrix after 3 passes; topological order ⇒ **linear** complexity (not "unchanged"!). → §2.5–2.6 (and PAL §2.7 — same question asked under PAL).

**Q26/Q27 — Kubr 2014, Hanzálek 2012:** algorithms + differences (Dijkstra vs Bellman-Ford vs Floyd; negative edges); priority-queue representation affects Dijkstra's complexity ($O(n^2)$ array vs $O(m+n\log n)$ heap). → §2.2–2.3, §2.6.

### Scheduling

**Q16 — Šůcha 2022:** define $1|r_j,\tilde d_j|C_{\max}$ + Graham notation; complexity (**strongly NP-hard via 3-Partition** — he asks for the reduction by name); Bratley + **pruning details**. → §6.1–6.2.

**Q3 — Šůcha 2023:** $P||C_{\max}$ vs $P|pmtn|C_{\max}$ — complexity classes ($P2||C_{\max}$ NP-hard via 2-partition; pmtn = relaxation → polynomial McNaughton); "how to solve $P||C_{\max}$ optimally" → **ILP with assignment variables** (he asks what the variables are); list scheduling as heuristic. → §6.5.

**Q28/Q29 — Hanzálek 2013, Šůcha 2012:** single-resource scheduling + $\sum w_jC_j$ (→ WSPT); $P||C_{\max}$ and $P|pmtn|C_{\max}$ algorithms + complexities, explain $P2||C_{\max}$ hardness. → §6.3, §6.5.

## Observations for preparation

- KO examiners (Hanzálek, Šůcha) are consistently described as friendly and **leading** — they want you talking at the board, and they steer with hints. Precise definitions still matter ("chtěl poměrně přesně definované odpovědi").
- The ILP question is nearly guaranteed: **definition → big-M toolbox (OR, implication, fixed costs) → TSP formulation**. Drill writing MTZ from memory and knowing DFJ as the alternative.
- For flows, the two differentiators between B and A are the **initial-feasible-flow construction** and knowing **which complexities depend on capacities**.
- Prepare one **worked CSP example** (Sudoku with alldifferent) — a student filled the whole slot with it and got no follow-ups.
