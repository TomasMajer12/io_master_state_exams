# BE4M33PAL — Algorithms (PAL) — Frequently Asked Questions

Past-exam questions specific to **BE4M33PAL**, condensed from `../../frequently_asked_questions/oi_wiki_committee_archive.md` (36 entries), cross-referenced to `summary.md`.

**Topic frequency:**

| Topic | Times asked | Summary section |
|---|---:|---|
| Graphs: definitions, representations, traversal, MST | 23 | §1.2–1.4, §2.1–2.2 |
| Heaps (binary / binomial / Fibonacci) | 8 | Appendix |
| Search trees (B / B+ / AVL / RB / Splay / 2-3-4) | 8 | §4 |
| Strongly connected components | 3 | §2.3 |
| Shortest paths (Dijkstra / Floyd / Bellman-Ford) | 3 | §2.7 |
| Text search / finite automata | 2 | §5 |

## Representative questions (with examiner expectations)

### Graphs, representations, MST ⭐⭐

**Q2 — Mařík 2022 (verbatim):** definition of directed graph; definition & properties of SCC; properties of the condensation; SCCs of a DAG; demonstrate one SCC algorithm on an example, state the principle of the second. → §2.3 (covers every sub-question incl. the demo example).

**Q3 — Vyskočil 2022:** graph representations (he wanted adjacency list, adjacency matrix, **Laplacian, incidence matrix**); define MST; describe one algorithm; **derive Kruskal's complexity**. → §1.3, §2.1.

**Q5 — Berezovskyj 2017:** when is shortest/longest path polynomial vs. exponential; MST in a complete graph with float weights — interval $(100,200)$ vs. only 21 distinct values; how large a graph in 1 s. → §2.7 (longest path NP-hard, DAG linear), §2.1 (near-linear Kruskal via bucket sort).

**Q6 — Mařík 2016:** representations, DFS+BFS, MST algorithms; **what is a cut and why the algorithms are correct**; **when Borůvka fails** (equal weights). → §2.1 correctness block + Borůvka tie example.

**Q9 — Velebil 2015:** MST algorithms; the detail he dug for: spanning tree **contains all vertices**. → §1.2 warning.

**Q13/Q16/Q18/Q21–Q23 — Mařík, Vyskočil, Šára, Macek, Kubr 2012–2014:** recurring formula: *define (un)directed graph → representations + comparison (when matrix vs. list) → DFS/BFS → spanning tree → demonstrate Prim/Kruskal/Borůvka on an example*. Šára follow-ups: multigraph, complete graph. → §1.2 definitions block, §2.1 worked example.

**Q17 — 2012:** DFS+BFS + a flood of definitions (tree, spanning tree, topological order, complete graph); uses of topological ordering. → §1.4–1.5.

### Heaps ⭐

**Q4 — Mařík 2018:** heap properties, representation, Insert/AccessMin/DeleteMin/Delete on a binary heap + complexities; binomial & Fibonacci — where and **why** (wanted: amortized-faster insert + definition of amortized complexity). → Appendix.

**Q25 — Čmolík 2015:** all heap types and operations; insisted on **array representation with index arithmetic** (not pointer objects); Navara follow-up: complexity of merging two binary heaps **distinguishing the two sizes**. → Appendix (merge paragraph).

**Q32 — Mařík 2015:** binary heap ops; when binomial vs. Fibonacci; wanted: **Fibonacci consolidation can be slow → bad for real-time**. → Appendix.

**Q33–Q35 — 2012 (Vyskočil):** binary vs. binomial/Fibonacci complexity comparison + **amortized complexity definition** (difference from average-case). → Appendix.

### Search trees ⭐

**Q26 — Mařík 2022 (verbatim):** define B-tree; demonstrate find & insert (multiphase) on an example; what is B+ and how it differs; what other search trees do you know. → §4.4 incl. worked insert.

**Q27 — Lisý 2017:** define B, B+, RB, 2-3-4, splay; B-tree insertion both strategies; **connection RB ↔ 2-3-4**. → §4.3–4.5.

**Q29 — Píša 2015:** **compare AVL and RB heights**; kinds of binary trees and what each is good for; **which structure for reading from a hard disk** (→ B/B+). → §4.2–4.4.

### Shortest paths

**Q8 — Mařík 2015:** define the problem, complexity classes; Dijkstra variants + complexity; Floyd for digraph; wanted **Bellman's principle** (shortest paths from shortest sub-paths) + heap use. → §2.7.

**Q15 — Demlová 2015:** define before describing Dijkstra; **Floyd–Warshall: what the matrix looks like after 3 passes** (→ $D^{(3)}$ invariant); topological order ⇒ **linear**; polynomial longest-path in general graphs would imply P = NP. → §2.7.

### Text search / automata

**Q24 — Berezovskyj 2023:** text search tasks; approximate search (Hamming, Levenshtein, k substitutions, inserts); dictionary automaton; multiple words at once (→ Aho–Corasick); wanted a **formal definition of a finite automaton** at the end. → §5.1, §5.4–5.5.

**Q30 — 2012:** text search from the FA viewpoint; FA–regular language relationship; Hamming & Levenshtein definitions. → §5.1, §5.4.

### Flows (asked under common part, formally KO)

**Q1 — Berezovskyj 2023; Q11 — Kubr 2014:** max flow definition (he demanded precision — "what *is* a flow?": function $f:E\to\mathbb R$ with capacity bounds + conservation), Ford–Fulkerson, relation to bipartite matching. → KO summary §flows; PAL §2.7 note.

## Observations for preparation

- The committee's default PAL question is **"graphs from scratch"**: definitions → representations → DFS/BFS → MST with a board demo. Drill the §2.1 worked example until you can draw it cold.
- Examiners repeatedly **interrupt for definitions** (graph, tree, spanning tree, cut, flow, automaton). Lead with the formal definition before describing any algorithm.
- Every data-structure answer should end with **complexities** and a **"when would you use it"** sentence — that's the standard follow-up.
- Mařík is the most frequent PAL examiner in the archive (9×); his style: verbatim multi-part questions, wants demonstrations on small examples, friendly.
