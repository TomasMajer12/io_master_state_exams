# BE4M36SMU — Symbolic Machine Learning (SMU) — Study Summary

**Language:** English-primary (slides are English), key Czech terms in parentheses for the oral exam.
**Source materials:** SMU lecture slides in `materials/` — COLT 1–3 (+3 solved tutorials), RL lectures 1–4 (+3 solved tutorials; "heavily inspired by the Stanford RL course of Prof. **Emma Brunskill**"), bandits lectures 5–6, NLP 1–4 (based on **Jurafsky & Martin, *Speech and Language Processing*, 3rd ed. draft** — relevant chapters **3, 6, 7, 9**; 2nd ed. insufficient). COLT background: **Kearns & Vazirani**, *An Introduction to Computational Learning Theory*; surveys by **Angluin** and **Haussler**. All key results verified against these sources (flagged inline).

> **⭐ Exam strategy.** The four official sub-topics are: (1) PAC & online (mistake-bound) learnability + VC dimension, (2) learning conjunctions/disjunctions + reductions, (3) bandits, (4) RL. The spec explicitly asks for **comparisons** ("differences in assumptions — i.i.d.; mutual relations — does one imply the other?") and **algorithms with efficiency analysis** ("if a class is learnable in a model, show an algorithm"). The official spec uses AIMA terminology for RL (direct utility estimation, ADP) while the lectures use Brunskill's (MC evaluation, model-based RL) — **both mappings are given below**, learn to translate.

**Structure.** §1–4 mirror the official sub-topics; §5 NLP (course part, not in the official spec but examinable context); §6 cross-links; §7 references. Worked problems in `examples.md` (from the solved tutorials).

---

## 1. PAC and online (mistake-bound) learnability

> **Official sub-topic:** *PAC and online learnability models: definition, efficient learnability. Comparison: differences in assumptions (i.i.d.), mutual relations. VC dimension. Necessary and sufficient conditions for learnability.*

### 1.1 Concept learning setting (COLT)

- **Instance space** `X` (instances `x`, complexity measure `n` = arity of tuples, typically `X={0,1}ⁿ`); **concept** `C ⊆ X`; **concept class** `𝒞 ⊂ 2^X` (prior knowledge restricts which concepts are possible).
- **Hypothesis** `h` = finite description of a decision function `c: X→{0,1}` (rules, formulas, …); **hypothesis class** `H` = all hypotheses the learner can express. Note `|H|` can be smaller than the number of concepts; symbolic (logical) hypotheses are human-understandable (→ explainable AI).
- COLT asks two questions: **statistical** (how much data?) and **algorithmic** (how much computation?).

### 1.2 Online model = Mistake Bound (MB) model ⭐

Protocol (repeat forever): (1) learner receives `x∈X`; (2) predicts positive/negative; (3) is told the correct answer.
- **No assumptions on example order/choice** — possibly adversarial; **no i.i.d.**
- Algorithm **learns `𝒞` (MB model)** if for every `C∈𝒞` the **total number of mistakes ≤ poly(n)**; then `𝒞` is **learnable in the MB model** (the learner eventually stops making mistakes).
- **Efficiently learnable**: additionally the **time per example ≤ poly(n)**.

### 1.3 PAC model ⭐

Setting: **batch** learning; examples drawn **i.i.d. from an arbitrary, unknown distribution `P` on `X`**; error of hypothesis `err(h) = P(c(x) ≠ h(x))`.
- Algorithm **PAC-learns `𝒞`** if for every `C∈𝒞`, every distribution `P`, and all `0<ε,δ<1`: given `poly(1/ε, 1/δ, n)` i.i.d. examples it outputs, **with probability ≥ 1−δ** ("probably"), a hypothesis with **`err(h) ≤ ε`** ("approximately correct").
- **Efficiently PAC-learnable**: runtime also `poly(1/ε,1/δ,n)`.
- (Origin: Valiant 1984; surveys: Angluin "Computational Learning Theory: Survey and Selected Bibliography", Haussler "Probably approximately correct learning".)

### 1.4 Comparison of the two models ⭐ (explicitly in the spec)

| | **Mistake Bound (online)** | **PAC (batch)** |
|---|---|---|
| Interaction | predict-then-feedback, one example at a time | training set first, hypothesis after |
| Data assumption | **none** (adversarial order allowed) | **i.i.d.** from arbitrary fixed `P` |
| Success criterion | ≤ poly(n) mistakes **ever** | `err(h)≤ε` with prob ≥ 1−δ |
| Failure allowed | no (hard bound) | yes, prob ≤ δ; error ≤ ε tolerated |

**Mutual relation ("does one imply the other?"):**
- **MB ⇒ PAC**: any mistake-bound learner `L` (bound `M ≤ poly(n)`) converts to a PAC learner: make `L` **lazy** (update only on mistakes), run it over the sample, and **halt when a hypothesis survives `(1/ε)·ln(M/δ)` consecutive examples**; output it. Terminates within `m = (M/ε)·ln(M/δ) ≤ poly` examples; probability that a bad hypothesis (err > ε) survives is `< M·e^{−(ε/ε)ln(M/δ)}·… = δ`. ⇒ **MB learnability is the stronger (more demanding) notion**.
- **PAC ⇏ MB** in general (PAC tolerates ε error forever; MB demands finitely many mistakes against any order).

### 1.5 Generic algorithms & learnability conditions

- **Halving algorithm** (MB, version space): keep all consistent hypotheses, predict by **majority vote**, delete wrong ones. Each mistake halves the version space ⇒ **mistake bound `lg|H|`**. ⇒ any finite class with `lg|𝒞| ≤ poly(n)` is **learnable** (but not necessarily *efficiently* — maintaining exponentially many hypotheses is costly).
- **Consistency + small `H` ⇒ PAC** ⭐: an algorithm is **𝒞-consistent** if it always returns `h∈H` consistent with the sample (requires `H ⊇ 𝒞`). A 𝒞-consistent algorithm PAC-learns 𝒞 if **`ln|H| ≤ poly(n)`**, with sample size `m = (1/ε)·ln(|H|/δ)` (proof: a fixed bad `h` survives `m` i.i.d. examples w.p. `≤(1−ε)^m < e^{−εm}`; union bound over `|H|`).
- **PAC output must be consistent**: a PAC learner's hypothesis is necessarily consistent with the training set (else an adversarial `P`, ε, δ break the guarantee).
- **Necessary & sufficient conditions for learnability** ⭐:
  - **Sufficient (finite class)**: `lg|𝒞| ≤ poly(n)` ⇒ learnable (halving / consistent learner).
  - **Necessary**: **`VC(𝒞) ≤ poly(n)`** — if a large set is shattered, an adversary can force a mistake on every prediction (MB), resp. no sample size suffices (PAC).
  - **Sufficient (infinite class)**: consistency + **`VC(H) ≤ poly(n)`** ⇒ PAC, with sample bound `m ≥ (8/ε)·(VC(H)·ln(16/ε) + ln(2/δ))`.
  - Relation: `VC(𝒞) ≤ lg|𝒞|` (shattering `d` points needs `2^d` distinct concepts) — so poly `lg|𝒞|` ⇒ poly VC, **not conversely** (`VC` can be finite for `|𝒞|=∞`).

### 1.6 VC dimension ⭐

- `𝒞` **shatters** `X'⊆X` if **every** subset `X''⊆X'` is cut out by some concept (`C∩X' = X''`) — all `2^{|X'|}` dichotomies.
- **`VC(𝒞)`** = size of the **largest** shattered set. Proof template: `≥d` — exhibit one shattered `d`-set; `<d+1` — show **no** `(d+1)`-set is shatterable.
- **Half-planes in ℝ²: VC = 3** (3 non-collinear points shatterable; 4 points never — either one inside the triangle of others, or the XOR-labeled convex quadrilateral, both unrealizable by a line). Generally **half-spaces in ℝⁿ: VC = n+1**. *(Matches SSU §1.7.)*

### 1.7 Inconsistent (agnostic) learning, Hoeffding & ERM

When consistency is impossible (noise, `H⊉𝒞`, unknown 𝒞): define **training error / empirical risk** `êrr(h)`.
- **Hoeffding**: for fixed `h`, `P(|err(h) − êrr(h)| > ε) ≤ 2e^{−2ε²m}`.
- Union bound over finite `H`: with prob ≥ 1−δ, **`|err−êrr| ≤ √(ln(2|H|/δ)/(2m))` for all h** — sample complexity `m = (1/2ε²)·ln(2|H|/δ)`.
- **ERM** (`h = argmin êrr`): then `err(h) ≤ êrr(h*) + 2ε ≤ err(h*) + 2ε` — within 2ε of the **best hypothesis in H**. Dilemma: large `H` → small `êrr` achievable but loose bound (**bias–complexity trade-off**). *(Same machinery as SSU §1.4–1.5 — say so at the exam.)*

---

## 2. Learnability of conjunctions and disjunctions (+ reductions) ⭐

> **Official sub-topic:** *Algorithms for learning conjunctions/disjunctions in both models, efficiency; learning other classes by reduction to conjunctions or disjunctions.*

### 2.1 The generalization (elimination) algorithm — MB model

Learn conjunctions over `X={0,1}ⁿ`:
1. Start `h = x₁x̄₁x₂x̄₂…xₙx̄ₙ` (all 2n literals — tautologically false). (Monotone version: `h = x₁x₂…xₙ`.)
2. Predict "yes" iff `x ⊨ h`.
3. On a **false negative** (said "no", was positive): **delete all literals of `h` false on `x`**.
4. A false positive ⇒ "concept is not a conjunction."

**Mistake-bound analysis** ⭐: target literals are never deleted (`c ⊆ h` always, so no false positives); the first mistake deletes `n` literals; each further mistake deletes ≥ 1 ⇒ **≤ n+1 mistakes**; time per example `O(n)`. ⇒ **conjunctions are efficiently learnable (MB)**.

### 2.2 Disjunctions — by duality (reduction #1)

If `c` is a disjunction for `C`, then `c̄` is a **conjunction** for the complement `X∖C` (De Morgan). Run the conjunction learner on complemented labels, negate the result. ⇒ **disjunctions efficiently learnable**.

### 2.3 PAC-learning conjunctions

Run the generalization algorithm over `m` i.i.d. examples, output the final `h`.
- Call literal `z` **bad** if `P(z inconsistent with random example) ≥ ε/2n`. If `h` has no bad literal, `err(h) ≤ 2n·(ε/2n) = ε`.
- A bad literal survives `m` examples w.p. `≤ (1−ε/2n)^m`; union over `2n` literals: `2n·e^{−mε/2n} < δ` ⟹
  **`m ≥ (2n/ε)·(ln 2n + ln(1/δ))`** ⇒ poly ⇒ **conjunctions are efficiently PAC-learnable**. ⭐ (Know this derivation.)

### 2.4 WINNOW — sparse disjunctions (MB) ⭐

Learns monotone **k-disjunctions** (k relevant out of n variables, `k ≪ n`). Linear-threshold hypothesis `h∈ℝⁿ`, init all weights 1; predict "yes" iff `Σ hᵢxᵢ > n/2`. **Multiplicative updates** (vs perceptron's additive):
- false **negative** → **double** all `hᵢ` with `xᵢ=1`;
- false **positive** → **zero** all `hᵢ` with `xᵢ=1`.
**Analysis**: weights never negative, never exceed `n`; each false negative adds ≤ n/2 total weight, each false positive removes > n/2 ⇒ `P ≤ 2+N`; each false negative doubles one of the k target weights, which can't exceed `n` ⇒ `N ≤ k·lg n`. **Mistake bound `2 + 2k·lg n`** — *logarithmic in n* ⇒ excels with many irrelevant attributes.

### 2.5 Reductions to conjunctions/disjunctions ⭐ (the exam's "other classes")

| Class | Reduction | Result |
|---|---|---|
| **k-CNF** | each of the `n' = Σᵢ≤k C(n,i)2^i ≤ poly(n)` possible clauses → a **new variable**; learn a **monotone conjunction** over them | efficiently learnable (MB & PAC) |
| **k-DNF** | dually, terms → variables, learn a disjunction | efficiently learnable |
| **k-term DNF** | *proper* learning is **NP-hard**; but `k-term DNF ⊆ k-CNF` (multiply out: `(abc)∨(de) ≡ (a∨d)(a∨e)…`) | efficiently learnable **improperly** with `H` = k-CNF |
| **k-clause CNF** | dually `⊆ k-DNF` | improper, efficient |
| **k-decision trees (k-DT)** | depth-≤k trees; root-to-leaf-1 paths → terms ⇒ `k-DT ⊆ k-DNF` (and paths to 0-leaves ⇒ `⊆ k-CNF`) | **efficiently improperly** PAC-learnable; **properly but inefficiently** (lg|k-DT| ≤ poly(n) via recursion `c₁=2`, `c_{k+1}= n·c_k²`); proper+efficient = NP-hard |
| **Non-monotone → monotone** | double the variables (`xᵢ, x̄ᵢ` as separate variables) | WINNOW etc. apply |

**Proper vs improper learning** ⭐: *proper* = output hypothesis from the same class as the concept; *improper* = a richer `H` — can turn NP-hard proper problems into efficient ones (k-term DNF the canonical example).

---

## 3. Multi-armed bandits ⭐

> **Official sub-topic:** *Problem definition, regret, ε-greedy and limitations, UCB + regret guarantees, Thompson sampling.*

### 3.1 Problem & regret

**Multi-armed bandit** = a degenerate **single-state MDP**: actions `A = {a₁,…,a_m}` ("arms"), reward distributions `P[Rₜ=r | Aₜ=a]`; each step pick an arm, get a sampled reward; goal: maximize `Σₜ Rₜ`. (Use case: ad selection — arm = ad, reward = click. Efficiency now matters: failed exploration **costs real reward**.)
- **Action value** `Q(a) = E[Rₜ|Aₜ=a]`; **optimal value** `V* = maxₐ Q(a)`, optimal arm `a* = argmaxₐ Q(a)`.
- **Regret** at step t: `Lₜ = V* − Q(Aₜ)` (expected opportunity loss — not directly observable). **Total regret** `L_T = Σₜ₌₁ᵀ (V* − Q(Aₜ))`. **Minimizing total regret ⇔ maximizing expected return**. Goal: regret growing **sublinearly** in T (flat regret = acting optimally).
- Estimation: `Q̂(a)` by incremental averaging `Q̂(aₜ) += (1/N(aₜ))(rₜ − Q̂(aₜ))`.

### 3.2 Greedy and ε-greedy — and their limitations ⭐

- **Pure greedy** (always `argmax Q̂`): can **lock onto a suboptimal arm forever** after unlucky initialization (e.g. true Q = (0.8, 0.5), initial estimates (0, 0.5) → never tries arm 1) ⇒ **linear regret**.
- **ε-greedy**: with prob 1−ε exploit, with prob ε pick uniformly. With **constant ε**, every step has probability `ε−ε/|A|` of a suboptimal pull ⇒ still **linear regret**. Decaying schedules `εₜ` can do better but are tricky to tune (need knowledge about the gaps).

### 3.3 UCB — optimism in the face of uncertainty ⭐

Maintain an **upper confidence bound** per arm; pull the arm with the largest bound:
```
Uₜ(aᵢ) = Q̂(aᵢ) + √( (1/2N(aᵢ)) · log(2t/δ) ),   aₜ = argmaxₐ Uₜ(a)
```
(bonus from **Hoeffding's inequality**: with prob ≥ 1−δ the true Q lies below the bound). Rarely-pulled arms get a big bonus → automatic, *directed* exploration; the bonus shrinks as `N(a)` grows.
- **Regret guarantee** ⭐: UCB achieves **sublinear regret `O(√(mT log T))`** (m arms); in the gap-dependent form, regret grows only **logarithmically in T** — `O(Σᵢ (log T)/Δᵢ)`, matching the **Lai–Robbins lower bound** asymptotically (UCB1, Auer et al. 2002). *(Verified.)*

### 3.4 Thompson sampling (probability matching) ⭐

**Bayesian** approach: keep a **posterior** over each arm's parameter; each round **sample** a parameter from each posterior, pull the arm whose sample is best, then **update the posterior** with the observed reward.
- Bernoulli rewards + **Beta prior** is conjugate: `Beta(α,β)` → after `Pos` successes and `Neg` failures → posterior **`Beta(α+Pos, β+Neg)`** (derivation in the lecture via the Beta function).
- Pulls each arm with the probability that it is optimal (**probability matching**); exploration arises naturally from posterior spread; empirically strong, regret guarantees comparable to UCB.

---

## 4. Reinforcement learning ⭐

> **Official sub-topic:** *State utility, optimal policy, value iteration, direct utility estimation, adaptive dynamic programming, temporal difference learning, exploration vs exploitation, Q-learning, SARSA. Policy search.*
> Lectures follow **Brunskill (Stanford CS234)**; the official spec uses **AIMA (Russell–Norvig)** terms — mapping: **direct utility estimation = Monte-Carlo evaluation**, **ADP = model-based RL**, "utility" = value `V(s)`.

### 4.1 MDPs: utilities, policies, Bellman ⭐

- **Markov process**: states + transition matrix `P(s'|s)`, **Markov property** (future ⫫ past | present). **Markov reward process** adds `R(s)=E[Rₜ|Xₜ=s]` and **discount** `γ∈[0,1]`. **MDP** adds actions: `P(s'|s,a)`, `R(s,a)`.
- **Return** `Gₜ = Rₜ + γRₜ₊₁ + γ²Rₜ₊₂ + …` (discounting keeps it finite for infinite horizon; γ=0 myopic, γ=1 far-sighted).
- **Policy** `π(a|s)`; **state-value (utility)** `V^π(s) = E[Gₜ | Xₜ=s, π]`; **action-value** `Q^π(s,a)`.
- **Bellman expectation equation**: `V^π(s) = R(s,π(s)) + γ Σ_{s'} P(s'|s,π(s)) V^π(s')` (linear system — solvable exactly, or by iterated **Bellman backup**).
- **Optimal policy** `π*`: `V*(s) = max_π V^π(s)`; **Bellman optimality**: `V*(s) = maxₐ [R(s,a) + γΣ_{s'}P(s'|s,a)V*(s')]`; `π*(s) = argmaxₐ Q*(s,a)` (greedy w.r.t. V*).
- **Value iteration (VI)** ⭐: repeat `V ← B[V]` (Bellman optimality backup) until convergence; **converges for γ<1 from any init** because the backup is a **γ-contraction** in ∞-norm (Banach fixed point). Extract π* greedily. Finite-horizon version: backward induction.
- **Policy iteration**: alternate **policy evaluation** (solve V^π) and **policy improvement** (greedy w.r.t. Q^π); converges in finitely many iterations (improvement is monotone).

### 4.2 Learning when the model is unknown

**(a) Model-based RL = Adaptive Dynamic Programming (ADP, AIMA term)**: learn `P̂(s'|s,a)` and `R̂` from experienced transitions (counts/averages), then solve the learned MDP by VI/PI. Sample-efficient but compute-heavy per step.

**(b) Direct Utility Estimation (AIMA) = Monte-Carlo (MC) policy evaluation** ⭐: estimate `V^π(s)` as the **average of observed returns** from `s` over complete episodes (model-free, episodic only):
- **First-visit MC** — average returns from the *first* visit of `s` per episode: **unbiased**, consistent.
- **Every-visit MC** — use every visit: **biased but consistent**, lower variance in practice.
- **Incremental MC**: `V(sₜ) ← V(sₜ) + α(Gₜ − V(sₜ))` (α=1/N(s) recovers averaging).
- Ignores state relations (doesn't exploit Bellman) → high variance, needs full episodes.

**(c) Temporal-Difference (TD) learning** ⭐: update from a **single step** using **bootstrapping** (the current estimate of the next state):
```
V(sₜ) ← V(sₜ) + α( Rₜ + γV(sₜ₊₁) − V(sₜ) )       (TD(0); the bracket = TD error δₜ)
```
- Model-free, **online, works in non-episodic tasks**; biased (bootstrap) but much lower variance than MC; converges for suitable decaying α.
- **MC vs TD vs ADP/DP**: MC = sample full returns (no bootstrap, no model); TD = sample + bootstrap; DP = full-width backup with model.

### 4.3 Model-free control: exploration vs exploitation, SARSA, Q-learning ⭐

Work with `Q(s,a)` (model-free greedy improvement needs Q, not V). **Exploration vs exploitation** (*průzkum vs využití*): must keep trying non-greedy actions to find better ones — **ε-greedy** policies; **GLIE** ("greedy in the limit with infinite exploration": every (s,a) visited infinitely often AND policy → greedy, e.g. `εᵢ = 1/i`) guarantees convergence of MC control to π*.

- **SARSA (on-policy TD control)**: update with the action actually taken next (quintuple `s,a,r,s',a'`):
  ```
  Q(s,a) ← Q(s,a) + α( r + γ·Q(s',a') − Q(s,a) )
  ```
  Learns the value of the **policy being followed** (incl. its exploration) → "safer" behavior during learning (cliff-walking).
- **Q-learning (off-policy TD control)** ⭐: update with the **best** next action regardless of what was taken:
  ```
  Q(s,a) ← Q(s,a) + α( r + γ·max_{a'} Q(s',a') − Q(s,a) )
  ```
  Learns `Q*` **directly** while following any sufficiently exploring behavior policy; converges to Q* under infinite visiting + Robbins-Monro step sizes.
- **On-policy vs off-policy**: evaluate/improve the executed policy (SARSA) vs learn about the greedy target policy from other behavior (Q-learning).

### 4.4 Function approximation & policy search

- **Value function approximation** (lecture 4): represent `V(s;w) = wᵀx(s)` (features `x(s)`; or a neural net — DQN from pixels). Train as supervised regression toward MC return (`Gₜ`) or TD target (`r+γV(s';w)`), minimizing MSE; semi-gradient updates. Needed when state spaces are huge (Atari); linear FA works with good (engineered) features.
- **Policy search / policy gradient** (per official spec; AIMA §"policy search"): parametrize the **policy** `π_θ(a|s)` directly (e.g. softmax over preferences) and **ascend the expected return** `∇_θ J(θ)` — REINFORCE-style score-function estimates, or even gradient-free search (hill-climbing over θ). + works for continuous/stochastic policies; − high variance, local optima. *(Thin in the slides — supplemented from Brunskill/AIMA, flagged.)*

---

## 5. Supplementary: NLP (Jurafsky & Martin, SLP 3rd ed. draft — ch. 3, 6, 7, 9)

> Course part of SMU; not in the official state-exam spec, but provides examinable context. Chapter mapping (3rd ed. 2025 draft, *verified*): **ch3 N-gram LMs · ch6 Vector Semantics & Embeddings · ch7 Neural Networks · ch9 Transformers** (RNNs/LSTMs in ch8 — also covered by lecture NLP4).

### 5.1 Probabilistic language models — n-grams (NLP1; ch3)

- **LM** = `P(W)` or `P(wₙ|w₁…wₙ₋₁)`. **Chain rule** → histories too sparse → **Markov assumption**: `P(wₙ|w₁…wₙ₋₁) ≈ P(wₙ|wₙ₋ₖ…wₙ₋₁)` (unigram/bigram/trigram…).
- **MLE estimates** = relative counts `P(wᵢ|wᵢ₋₁) = c(wᵢ₋₁,wᵢ)/c(wᵢ₋₁)`; work in **log space**.
- **Evaluation**: extrinsic vs intrinsic — **perplexity** `PP(W) = P(w₁…w_N)^{−1/N}` (lower = better; = average branching factor).
- **Zeros → smoothing**: **Laplace (+1)**, add-k; backoff & interpolation (mixing orders).

### 5.2 Vector semantics & embeddings (NLP2–3; ch6)

- Distributional hypothesis ("you shall know a word by the company it keeps") → word vectors.
- **Sparse**: term-document / term-term (co-occurrence) matrices; **TF-IDF** weighting; **PPMI** (positive pointwise mutual information). Similarity = **cosine** `cos(u,v)=u·v/(|u||v|)` ∈ [0,1] for non-negative vectors.
- **Dense embeddings**: **truncated SVD/LSA** over PPMI; **word2vec** — **skip-gram with negative sampling (SGNS)**: train a logistic classifier to distinguish true (word, context) pairs from sampled negatives; the learned weights *are* the embeddings; (CBOW dual). Properties: analogy structure (king−man+woman≈queen), bias caveats.
- Topic-model view (NLP3): unigram mixtures, corpus-generation "stories".

### 5.3 Neural LMs (NLP4; ch7) and sequence models (ch8–9)

- **Neural LM** (Bengio): one-hot → **embedding matrix E** (shared across positions) → hidden layer → softmax over vocabulary; beats n-grams via generalization over similar words; fixed window (or CNN) limits context.
- **RNNs**: recurrent state carries unbounded context; trained by unrolling (BPTT); used as LMs, for classification (incl. **bi-directional RNNs**) and sequence labeling; vanishing gradients → **LSTM** (gates: forget/input/output + cell state).
- **Transformers** (ch9): **(causal/masked) self-attention** — each position attends to previous positions via Q/K/V dot-product attention; **multi-head attention**; positional encodings; feed-forward blocks; basis of GPT/BERT-style models.

---

## 6. Cross-subject links
- **PAC, VC dimension, Hoeffding, ERM** ↔ **SSU §1** (same theory; SSU = statistical/continuous side, SMU = symbolic/discrete + online side). The exam spec explicitly invites this comparison.
- **Halving/version spaces, WINNOW vs perceptron** ↔ SSU linear classifiers.
- **Bandits/RL exploration (ε-greedy, GLIE)** ↔ SSU SGD stochasticity (different role of randomness — say so if asked).
- **MDP value iteration / Bellman contraction** ↔ KO (dynamic programming), TAL (fixed points).
- **n-gram LMs / HMM-style sequence models** ↔ SSU HMMs (Markov chains); **neural LMs/backprop** ↔ SSU §3.

## 7. References (verified)
- M. Kearns, U. Vazirani: *An Introduction to Computational Learning Theory*, MIT Press 1994 (the COLT monograph behind §1–2).
- D. Angluin: *Computational Learning Theory: Survey and Selected Bibliography* (STOC '92); D. Haussler: *Probably Approximately Correct Learning* (AAAI '90) — the two surveys referenced by the course; L. Valiant: *A Theory of the Learnable* (1984); N. Littlestone: *Learning Quickly When Irrelevant Attributes Abound* (WINNOW, 1988).
- E. Brunskill: Stanford **CS234 Reinforcement Learning** (lecture basis); R. Sutton & A. Barto: *Reinforcement Learning: An Introduction* (2nd ed.); Russell & Norvig *AIMA* ch. 17/22 (the spec's DUE/ADP terminology).
- Auer, Cesa-Bianchi, Fischer: *Finite-time Analysis of the Multiarmed Bandit Problem* (UCB1, 2002); Lai & Robbins (1985) lower bound; Thompson (1933).
- D. Jurafsky, J. Martin: *Speech and Language Processing*, **3rd ed. draft** — <https://web.stanford.edu/~jurafsky/slp3/> — **ch. 3, 6, 7, 9** (and 8 for RNNs/LSTMs). 2nd edition insufficient (per lecturer).
- Course pages: <https://cw.fel.cvut.cz/wiki/courses/smu>.
