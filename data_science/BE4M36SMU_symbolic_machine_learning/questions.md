# BE4M36SMU — Symbolic Machine Learning (SMU) — Questions

The OI-Wiki FAQ archive has **no SMU past-paper questions** (newer DS subject). Below: **anticipated questions derived from the official topic structure** and the course's solved tutorials (which the lecturer states are exam-representative), cross-referenced to `summary.md`. ⭐ = explicitly demanded by the official spec wording.

## Topic 1 — PAC and online learnability
- Define the **mistake-bound (online)** model and **(efficient) learnability** in it. → §1.2
- Define the **PAC** model precisely (ε, δ, i.i.d., poly sample, arbitrary distribution); efficient PAC learnability. → §1.3
- ⭐ **Compare the two models**: differences in assumptions (i.i.d. vs adversarial order, batch vs online), success criteria; **mutual relations — does one imply the other?** (MB ⇒ PAC via the lazy-learner conversion — know the construction; PAC ⇏ MB). → §1.4, examples.md F
- Halving algorithm + its `lg|H|` mistake bound; why learnable ≠ efficiently learnable. → §1.5
- ⭐ **Necessary and sufficient conditions for learnability**: `lg|𝒞| ≤ poly(n)` sufficient; **`VC(𝒞) ≤ poly(n)` necessary**; consistency + poly `VC(H)` sufficient for PAC; relation `VC ≤ lg|𝒞|`. → §1.5
- **VC dimension**: definition (shattering), proof technique, half-planes in ℝ² (=3) / ℝⁿ (=n+1). → §1.6, examples.md E
- Inconsistent/agnostic learning: Hoeffding, error bound, ERM guarantee. → §1.7 (cross-link SSU §1)

## Topic 2 — Conjunctions & disjunctions ⭐
- ⭐ "If the class is learnable in the model, **show an algorithm** that learns it": **generalization/elimination algorithm** for conjunctions (MB, ≤ n+1 mistakes, efficient) + its PAC version with the **sample-complexity derivation** `m ≥ (2n/ε)(ln 2n + ln 1/δ)`. → §2.1, §2.3, examples.md A–B
- Disjunctions via De Morgan duality. → §2.2
- **WINNOW** for sparse k-disjunctions: multiplicative updates, mistake bound `2+2k·lg n`, when it beats elimination/perceptron. → §2.4, examples.md C
- ⭐ **Learning other classes by reduction**: k-CNF/k-DNF (clauses→new variables), k-term DNF ⊆ k-CNF (improper learning; proper is NP-hard), k-DT ⊆ k-DNF/k-CNF; **proper vs improper** learning. → §2.5, examples.md D

## Topic 3 — Multi-armed bandits ⭐
- Definition (single-state MDP), action value Q(a), V*, **regret & total regret**; why minimizing regret = maximizing return; compute total regret of a given action sequence. → §3.1, examples.md G
- ⭐ **ε-greedy and its limitations**: greedy lock-in (linear regret), constant-ε linear regret, decaying-ε difficulties. → §3.2
- ⭐ **UCB**: the bound formula (Hoeffding bonus), optimism under uncertainty, **regret guarantee** (sublinear `O(√(mT log T))`; gap-dependent `O(Σ log T/Δᵢ)`, Lai–Robbins optimal). → §3.3
- ⭐ **Thompson sampling**: Bayesian posterior per arm, Beta-Bernoulli conjugate update, probability matching. → §3.4, examples.md H

## Topic 4 — Reinforcement learning ⭐
- MDP definition; **state utility / value function**, action value, return, discount; Bellman expectation & optimality equations; **optimal policy** = greedy w.r.t. V*/Q*. → §4.1
- ⭐ **Value iteration**: algorithm + why it converges (γ-contraction, fixed-point method); policy iteration (and its EM analogy — tutorial question). → §4.1, examples.md I
- ⭐ **Direct utility estimation** (= first/every-visit Monte-Carlo evaluation; bias/consistency of each) vs **adaptive dynamic programming** (= model-based RL: learn P̂,R̂, then solve) vs **temporal difference learning** (TD(0) update, bootstrap, online; learning-rate conditions). → §4.2, examples.md J
- ⭐ **Exploration vs exploitation**; ε-greedy policies; **GLIE** conditions and why they matter. → §4.3
- ⭐ **Q-learning vs SARSA**: both update rules, on-policy vs off-policy, convergence conditions, behavioral difference (cliff walking). → §4.3, examples.md K
- ⭐ **Policy search**: parametrize π_θ directly, gradient/score-function idea, pros/cons vs value-based. → §4.4
- Value function approximation (features, linear FA, MC/TD targets as regression). → §4.4

## NLP part (course material; possible follow-ups)
- N-gram LMs: Markov assumption, MLE counts, **perplexity**, Laplace smoothing. → §5.1, examples.md L
- TF-IDF, PPMI, cosine similarity; **word2vec/SGNS**; SVD embeddings. → §5.2
- Neural LM vs n-gram; RNN/LSTM; self-attention/transformer basics. → §5.3

## Likely cross-exam links (DS commission)
- "Relate PAC/VC here to SSU's ERM/VC" (the spec invites it); "policy iteration vs EM"; "bandits vs ε-greedy exploration in Q-learning".
