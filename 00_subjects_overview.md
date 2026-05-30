# CVUT FEL — Open Informatics Master State Exam
## Data Science Specialization — Subjects Overview

**Source:** Open Informatics Master State Exam Topics Specification (2025-02-28)
**Program:** Open Informatics (OI), Master
**Specialization:** Data Science

The state exam consists of **two parts**:
1. **Common part** — 3 subjects (shared across all OI specializations)
2. **Specialization part** — 6 subjects (specific to Data Science)

---

## Part A — Common Part (all specializations)

### A1. BE4M33PAL — Algorithms (PAL)
**Polynomial algorithms for standard graph problems. Combinatorial and number-theoretical algorithms, isomorphism, prime numbers. Search trees and their use. Text search based on finite automata.**

- Notation of asymptotic complexity of algorithms. Basic notation of graph problems — degree, path, circuit, cycle. Graph representations by adjacency, distance, Laplacian and incidence matrices. Adjacency list representation.
- Algorithms for minimum spanning tree (Prim-Jarník, Kruskal, Borůvka), strongly connected components (Kosaraju-Sharir, Tarjan), Euler trail. Union-find problem. Graph isomorphism, tree isomorphism.
- Generation and enumeration of combinatorial objects — subsets, k-element subsets, permutations. Gray codes. Prime numbers, sieve of Eratosthenes. Pseudorandom numbers properties. Linear congruential generator.
- Search trees — data structures, operations, and their complexities. Binary tree, AVL tree, red-black tree (RB-tree), B-tree and B+ tree, splay tree, k-d tree. Nearest neighbor searching in k-d trees. Skip list.
- Finite automata, regular expressions, operations over regular languages. Bit representation of nondeterministic finite automata. Text search algorithms — exact pattern matching, approximate pattern matching (Hamming and Levenshtein distance), dictionary automata.

### A2. BE4M01TAL — Theory of Algorithms (TAL)
**Problem/language complexity classes with respect to the time complexity of their solution and memory complexity including undecidable problems/languages.**

- Asymptotic growth of functions, time and space complexity of algorithms. Correctness of algorithms — variant and invariant.
- Deterministic Turing machines, multitape Turing machines, and Nondeterministic Turing machines.
- Decision problems and languages. Complexity classes P, NP, co-NP. Reduction and polynomial reduction, class NPC. Cook theorem. Heuristics and approximate algorithms for solving NP complete problems.
- Classes based on space complexity: PSPACE and NPSPACE. Savitch Theorem.
- Randomized algorithms. Randomized Turing machines. Classes based on randomization: RP, ZPP, co-RP.
- Decidability and undecidability. Recursive and recursively enumerable languages. Diagonal language. Universal language and Universal Turing machine.

### A3. BE4M35KO — Combinatorial Optimization (KO)
**Combinatorial optimization problems — formulation, complexity analysis, algorithms and example applications.**

- Integer Linear Programming. Shortest paths problem and traveling salesman problem ILP formulations. Branch and Bound algorithm. Problem formulations using ILP. Special ILP problems solvable in polynomial time.
- Shortest paths problem. Dijkstra, Bellman-Ford, and Floyd–Warshall algorithms. Shortest paths in directed acyclic graphs. Problem formulations using shortest paths.
- Network flows. Maximum flow and minimum cut problems. Ford-Fulkerson algorithm. Feasible flow with balances. Minimum cost flow and cycle-canceling algorithm. Problem formulations using network flows. Maximum cardinality matching.
- Knapsack problem. Approximation algorithm, dynamic programming approach, approximation scheme.
- Traveling salesman problem. Double-tree algorithm and Christofides algorithm for the metric problem. Local search k-OPT.
- Scheduling — problem description and notation. One resource — Bratley algorithm, Horn algorithm. Parallel identical resources — list scheduling, dynamic programming. Project scheduling with temporal constraints — relative order and time-indexed ILP formulations.
- Constraint Satisfaction Problem. AC3 algorithm.

---

## Part B — Data Science Specialization (6 subjects)

### B1. BE4M36SAN — Statistical Analysis (SAN)
**Statistical analysis, models and their assessment. Dimensionality reduction. Clustering.**

- Multiple linear regression. Describe the model and its assumptions. Treatment of qualitative independent variables, collinearity and outliers. Decide whether a model is useful. Overfitting, feature selection, model regularization.
- Non-linear regression, polynomial regression, splines, local regression.
- Discriminant analysis. LDA, QDA and logistic regression.
- Robust statistics. Robust estimators of location and scale, M-estimators. Robust regression. Non-parametric tests.
- Dimensionality reduction, task definition, manifold, intrinsic dimension. PCA and kernel PCA. Non-linear dimensionality reduction methods.
- Clustering. The task formalization and its complexity. Clustering methods: k-means, EM GMM clustering, hierarchical clustering, density-based clustering. Spectral clustering.

### B2. BE4M39VIZ — Visualization (VIZ)
**Scientific visualization methods. Information visualization methods.**

- Characteristics of spatial data and spatial fields. Grids used to represent spatial fields and their relation with data enrichment. Visualization of scalar fields with color mapping and contouring. Direct volume rendering.
- Visualization of vector fields with glyphs, stream objects, and line integral convolution (LIC).
- Characteristics of abstract data. Visualization of tabular (or n-dimensional) data. Visualization of relational data (hierarchies, directed acyclic graphs, and undirected graphs).
- Visualization of text. Visualization of a single document. Visualization of document corpus.
- Visualization of time-varying data. Time primitives and relations between them. Time domain, its type, granularity and structure.

### B3. BE4M33OSW — Ontologies and Semantic Web (OSW)
**Ontologies. Basic principles of ontological engineering, semantic web technologies, basic principles and technologies of linked data.**

- Characteristics of Description Logics. Basics of ALC, SHOIN(D) and SROIQ(D) description logics, computational complexity of key reasoning in these logics. Tableau algorithm for ALC. Blocking conditions in description logics.
- Semantic Web stack, RDF, OWL, SPARQL, SWRL, tractability of rules in description logics (DL-safe rules).
- Basics of RDF Graph model, blank node semantics. SPARQL query execution. Possible evaluation semantics of SPARQL and their differences.
- Defining principles of linked data. Hash vs. 303 URIs, Five star linked open data model.

### B4. BE4M33SSU — Statistical Machine Learning (SSU)
**Minimizing empirical risk. Maximum likelihood estimation, EM algorithm. Deep networks and their training. Classical and deep neural networks and their learning.**

- Empirical risk minimization: Risk and empirical risk of a predictor, generalization bounds and Hoeffding inequality, statistically consistent learning algorithms, Vapnik-Chervonenkis-dimension of a hypothesis class, SVMs and Kernel SVMs.
- Maximum likelihood estimators and the EM algorithm: maximum likelihood estimator, consistency of an estimator, EM algorithm as maximization of a lower bound of the likelihood, E-step and M-step as block-coordinate descent for the lower bound.
- Deep networks, supervised learning of networks: neurons, network architectures, convolutional networks, backpropagation and layer types, parameter initialisation, stochastic gradient descent.

### B5. BE4M36SMU — Symbolic Machine Learning (SMU)
**Learnability models: PAC and online. Learnability of conjunctions and disjunctions. Multi-armed bandit problem. Reinforcement learning.**

- PAC and online learnability models: definition, efficient learnability. Comparison of the two models: differences in assumptions (such as i.i.d. observations), mutual relations (does one imply the other?). Vapnik-Chervonenkis dimension. Necessary and sufficient conditions for learnability.
- If one or the other concept class is learnable in one or the other learnability models, show an algorithm that learns it in that model. Are they also learnable efficiently? Learning other hypothesis classes by reduction to learning conjunctions or disjunctions.
- Multi-armed bandit problem — definition of the problem, the notion of regret, algorithms to solve bandit problems: epsilon-greedy algorithms and their limitations, UCB algorithm and guarantees on its regret, Thompson sampling algorithm.
- Reinforcement learning: state utility, optimal policy, value iteration, direct utility estimation, adaptive dynamic programming, temporal difference learning, exploration vs. exploitation, Q-learning, and SARSA. Policy search.

### B6. BE4M36DS2 — Big Data / NoSQL Database Systems (DS2)
**Big Data concept, basic principles of distributed data processing, types and properties of NoSQL databases.**

- Big Data (V characteristics, current trends), NoSQL databases (motivation, features). Scaling (vertical, horizontal, network fallacies, cluster). Distribution models (sharding, replication, master-slave and peer-to-peer architectures). CAP theorem (properties, ACID, BASE). Consistency (strong, eventual, read, write, quora). Performance tuning (Amdahl's law, Little's law, message cost model). Polyglot persistence.
- MapReduce (architecture, functions, data flow, execution, use cases). Hadoop (MapReduce, HDFS).
- SPARQL (subgraph matching, graph patterns, datasets, filters, solution modifiers, query forms).
- Redis (data types, operations, TTL, persistence). Cassandra (keyspaces, column families, CRUD operations). MongoDB (CRUD operations, update and query operators, projection, modifiers, aggregations).
- Graph data structures (adjacency matrix, adjacency list, incidence matrix). Data locality (BFS layout, bandwidth minimization problem, Cuthill-McKee algorithm). Graph partitioning (1D partitioning, 2D partitioning). Neo4j (traversal framework, traversal description, traverser). Cypher (graph matching, read, write and general clauses).

---

## Repository Layout

```
io_master_state_exams/
├── 00_subjects_overview.md         (this file)
├── CLAUDE.md                       (project context for the AI assistant)
├── state_exam_topic_specification.pdf
│
├── common/                         (3 subjects shared across all OI specializations)
│   ├── BE4M33PAL_algorithms/
│   ├── BE4M01TAL_theory_of_algorithms/
│   └── BE4M35KO_combinatorial_optimization/
│
├── data_science/                   (6 specialization subjects)
│   ├── BE4M36SAN_statistical_analysis/
│   ├── BE4M39VIZ_visualization/
│   ├── BE4M33OSW_ontologies_semantic_web/
│   ├── BE4M33SSU_statistical_machine_learning/
│   ├── BE4M36SMU_symbolic_machine_learning/
│   └── BE4M36DS2_big_data_nosql/
│
└── frequently_asked_questions/     (raw archives of past exam questions, all subjects)
    ├── README.md
    ├── oi_wiki_committee_archive.md       (processed)
    └── source_oi_wiki_committee_archive.pdf (raw source)
```

Each subject directory contains:
- `materials/` — original source files (presentations, study notes, examples) provided by Tomas. Read-only for Claude.
- `summary.md` — main study summary with theory + worked examples (placeholder until materials arrive).
- `questions.md` — past exam questions specific to this subject, cross-referenced to `summary.md`.
- `examples.md` — worked-out examples / problem solutions (optional, when there are many).

The `frequently_asked_questions/` directory holds the cross-subject raw archives. Per-subject filtered question lists live in each subject's `questions.md`.
