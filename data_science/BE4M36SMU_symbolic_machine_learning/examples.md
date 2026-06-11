# BE4M36SMU — Worked Examples

Exam-style problems with solutions, drawn from the solved tutorials (`materials/colt-tutorial-0*.pdf`, `materials/rltutorial*.pdf`) and the lectures. Cross-references to `summary.md` in brackets.

---

## A. Generalization algorithm run (conjunctions, MB) [§2.1] ⭐

Target `c = x₁x̄₃` over n=3 variables. `h₀ = x₁x̄₁x₂x̄₂x₃x̄₃` (always predicts "no").
1. Example `x=(1,0,0)` (positive). Predict "no" → **mistake**. Delete literals false on x (`x̄₁, x₂, x₃`): `h = x₁x̄₂x̄₃`.
2. `x=(1,1,0)` (positive). `h` false (`x̄₂` violated) → **mistake**. Delete `x̄₂`: `h = x₁x̄₃`.
3. Now `h = c`: no more mistakes, ever.
Bound check: first mistake removed n=3 literals, each later ≥1 ⇒ ≤ n+1 = 4 mistakes total. Target literals never deleted ⇒ `c ⊆ h` throughout ⇒ no false positives.

## B. PAC sample complexity for conjunctions (numeric) [§2.3] ⭐

`m ≥ (2n/ε)(ln 2n + ln(1/δ))`. For n=20 variables, ε=0.1, δ=0.05:
`m ≥ (40/0.1)·(ln 40 + ln 20) = 400·(3.689+2.996) ≈ 2674` examples. Polynomial in n, 1/ε, 1/δ — efficient PAC.

## C. WINNOW run + bound [§2.4]

n=4, target = monotone 1-disjunction `x₂`. `h=(1,1,1,1)`, threshold `Σhᵢxᵢ > 2`.
- `x=(0,1,0,0)` positive: `Σ=1 ≤ 2` → predict "no" → **false negative** → double weights where xᵢ=1: `h=(1,2,1,1)`.
- `x=(1,0,1,1)` negative: `Σ=3 > 2` → "yes" → **false positive** → zero those weights: `h=(0,2,0,1)`.
- `x=(0,1,0,0)` positive: `Σ=2 ≤ 2` → false negative → `h=(0,4,0,1)`. Now `x₂` alone clears the threshold; converged.
Bound: `2 + 2k·lg n = 2+2·1·2 = 6` mistakes max — logarithmic in n (vs n+1 for the elimination algorithm) because only k=1 of 4 attributes is relevant.

## D. k-term DNF: improper learning [§2.5] ⭐

`(ab)∨(cd)` (2-term DNF) ≡ `(a∨c)(a∨d)(b∨c)(b∨d)` (2-CNF, by distributivity). Proper 2-term-DNF learning is NP-hard, but learn the 2-CNF instead: create a variable per possible ≤2-literal clause (`O(n²)` of them), run the **conjunction** learner, substitute back. Efficient, **improper** (output class = k-CNF ⊋ k-term DNF). Same trick: k-DT ⊆ k-DNF (paths to 1-leaves) and ⊆ k-CNF (paths to 0-leaves).

## E. VC dimension proofs [§1.6] ⭐

- **Half-planes in ℝ²**: 3 non-collinear points → all 8 labelings separable ⇒ VC≥3. 4 points: if one is inside the triangle of the others, label it − and the rest +; if they form a convex quadrilateral, label diagonally (XOR) — neither is linearly separable ⇒ VC<4 ⇒ **VC=3**.
- **Finite class**: shattering d points requires 2^d concepts ⇒ `VC(𝒞) ≤ lg|𝒞|` [examples: conjunctions on n vars, |𝒞|≤3ⁿ ⇒ VC ≤ n·lg3].
- **Intervals [a,b] on ℝ**: 2 points shatterable; 3 points `x₁<x₂<x₃` with labels (+,−,+) impossible ⇒ VC=2.

## F. MB ⇒ PAC conversion [§1.4] ⭐

Given a lazy MB learner with bound M: run on i.i.d. examples, **halt when a hypothesis survives `(1/ε)ln(M/δ)` consecutive examples**, output it. Total examples ≤ `(M/ε)ln(M/δ)`. Failure probability: each of ≤M tried hypotheses that is bad (err>ε) survives its window w.p. ≤ `(1−ε)^{(1/ε)ln(M/δ)} < e^{−ln(M/δ)} = δ/M`; union ⇒ ≤ δ. All PAC conditions hold ⇒ **MB learnability ⇒ PAC learnability** (the converse fails).

## G. Total regret computation [§3.1] (lecture example)

Two ads: `Q(a₁)=0.8, Q(a₂)=0.5` ⇒ `V*=0.8`, gaps `Δ₁=0, Δ₂=0.3`. Action sequence uses a₂ twice ⇒ total regret `= 2×0.3 = 0.6` (rewards observed don't matter — regret is in **expectations**). ε-greedy with constant ε: per-step expected regret ≥ `(ε−ε/|A|)·Δ₂ > 0` ⇒ **linear** total regret. UCB: `O(√(mT log T))` sublinear / `O(Σᵢ log T/Δᵢ)` gap-dependent.

## H. Thompson sampling update [§3.4]

Bernoulli arm with prior `Beta(1,1)` (uniform). Observe 3 clicks, 1 miss ⇒ posterior `Beta(4,2)` (mean 4/6≈0.67). Each round: sample `θ̂ᵢ ~ posteriorᵢ` for every arm, pull `argmax θ̂ᵢ`, update that arm's Beta. Wide posteriors (rarely-pulled arms) sometimes produce big samples ⇒ built-in exploration.

## I. Value iteration mini-run [§4.1] (rltutorial1)

Two states `s₁,s₂`, `γ=0.5`; `R(s₁)=1, R(s₂)=0`; action a: stay; action b: switch (deterministic). `V₀=(0,0)`.
- `V₁(s₁)=1+0.5·max(V₀(s₁),V₀(s₂))=1`; `V₁(s₂)=0+0.5·max(0,0)=0`.
- `V₂(s₁)=1+0.5·max(1,0)=1.5`; `V₂(s₂)=0+0.5·max(1,0)=0.5`.
- `V₃=(1.75,0.75)` … → `V*=(2,1)`; π*: in s₂ switch to s₁, in s₁ stay. Convergence guaranteed: Bellman backup is a **γ-contraction** (fixed-point/`metoda prosté iterace`). Conceptual links (tutorial): fixed sequence of actions ≠ MDP solution (stochasticity ⇒ closed-loop policy needed; fixed plans = classical planning); **policy iteration ≈ EM** (introduce policy as auxiliary variable, alternate optimize-evaluate — same block-coordinate idea as SSU §2.5).

## J. MC vs TD on one episode [§4.2] ⭐

Episode (γ=1): `s₁,r=0 → s₂,r=0 → s₃,r=1 → end`; all `V=0`, α=0.5.
- **First-visit MC (= direct utility estimation)**: returns `G(s₁)=1, G(s₂)=1, G(s₃)=1` ⇒ all V's move toward 1: `V=0.5` each.
- **TD(0)**: `V(s₁) += 0.5(0+V(s₂)−V(s₁)) = 0`; `V(s₂) += 0.5(0+V(s₃)−V(s₂)) = 0`; `V(s₃) += 0.5(1+0−0) = 0.5`. Only s₃ moves — TD propagates **one step per episode** (information flows backward over repeated episodes) but works online & with lower variance.
- Learning-rate intuitions (rltutorial2): per-state α needed (different visit counts); `α(nₛ)` must satisfy Robbins–Monro (`Σα=∞, Σα²<∞`, e.g. 1/nₛ) for convergence; constant α never converges (tracks); too-fast decay stops learning early.

## K. SARSA vs Q-learning on one transition [§4.3] ⭐

`Q(s,a)=2`; transition `(s,a,r=1,s')` with `Q(s',a')`: Q(s',greedy)=5, exploratory action taken a'' with Q(s',a'')=0; γ=1, α=0.5.
- **SARSA** (uses the action actually taken, a''): `Q(s,a) ← 2+0.5(1+0−2) = 1.5`.
- **Q-learning** (uses max): `Q(s,a) ← 2+0.5(1+5−2) = 4`.
Off-policy Q-learning learns the greedy target regardless of exploration; SARSA bakes exploration into its values (cliff-walking: SARSA learns the safe path, Q-learning the optimal-but-risky one). ε-schedules (rltutorial3): GLIE requires every (s,a) visited infinitely often + ε→0, e.g. `ε(nₛ)=1/nₛ`; per-state ε because exploration needs differ.

## L. Perplexity (NLP) [§5.1]

Test sentence of N=5 words, bigram model assigns `P=1/1024` ⇒ `PP = P^{−1/5} = 1024^{0.2} = 4` — the model is "as confused as" a uniform 4-way choice per word. Laplace smoothing: `P(wᵢ|wᵢ₋₁) = (c+1)/(c(wᵢ₋₁)+|V|)` — prevents zero-probability (infinite-perplexity) events.
