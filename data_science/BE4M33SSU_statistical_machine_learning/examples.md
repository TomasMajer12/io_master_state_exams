# BE4M33SSU — Worked Examples

Exam-style worked problems drawn from the SSU seminars (`materials/seminar_*`), with full derivations. Cross-references to `summary.md` in brackets. SSU exams reward **deriving**, so each solution shows the steps.

---

## A. ERM overfitting — the "lookup table" [§1.2]

`X=[a,b]`, `Y={±1}`, 0/1 loss, `p(x|y=+1)=p(x|y=−1)`=uniform, `p(y=+1)=0.8`. Learner returns `hₘ(x)=yʲ` if `x=xʲ∈Tᵐ`, else `−1`.
- **(a) Training error = 0 w.p. 1**: every training `xʲ` is memorized with its label, and (continuous `x`) the points are distinct w.p. 1, so `hₘ(xʲ)=yʲ` for all `j` ⇒ `R_Tᵐ(hₘ)=0`.
- **(b) Expected risk = 0.8 w.p. 1**: a fresh test `x∉Tᵐ` w.p. 1, so `hₘ(x)=−1` always. Since `p(y=+1)=0.8`, it is wrong 80% of the time ⇒ `R(hₘ)=0.8`. (The Bayes-optimal `h≡+1` has `R*=0.2`.)
- **(c) Another zero-training-error learner**: a 1-NN classifier (predicts the label of the nearest training point) — also memorizes, also generalizes badly here.
- **Moral**: zero training error ⇏ low risk; must constrain `H`.

## B. VC dimension of thresholds [§1.7]

`H={h(x)=sign(x−θ) | θ∈ℝ}`. **VC = 1.** One point is shattered (put `θ` on either side → both labels achievable... careful with the sign convention; the class realizes the two single-point labelings reachable). Two points `x₁<x₂` **cannot** be shattered: the labeling `(+1,−1)` (i.e. `h(x₁)=+1, h(x₂)=−1`) is impossible because `sign(x−θ)` is monotone non-decreasing in `x`. ⇒ `VC=1`.

## C. VC dimension of linear classifiers = `d+1` [§1.7] ⭐

`H={sign(⟨w,x⟩+b)}` in `ℝᵈ`. Two-part proof:
1. **`VC ≥ d+1`**: exhibit `d+1` points that are shattered — e.g. the origin + the `d` unit vectors (or any `d+1` affinely independent points). For any labeling, an affine hyperplane separating them exists.
2. **`VC < d+2`**: any `d+2` points in `ℝᵈ` are affinely dependent (Radon's theorem) → they can be split into two subsets whose convex hulls intersect; that dichotomy is **not** linearly separable.
⇒ `VC = d+1`. (Matches `n+1` for an `n`-dim feature map.)

## D. `VC(H) ≤ log₂|H|` for finite `H` [§1.7]

If `H` shatters `m` points it must realize all `2ᵐ` labelings, so `|H| ≥ 2ᵐ` ⇒ `m ≤ log₂|H|`. Taking the largest shatterable `m`: `VC(H) ≤ log₂|H|`. (Consistency check: the finite-H bound's `log|H|` plays the role VC dimension plays for infinite `H`.)

## E. Sample-complexity / generalization bound (numeric) [§1.4–1.5]

Finite `H`, 0/1 loss (`ℓ_max−ℓ_min=1`). Bound: `P(max_h|R−R_Tᵐ| ≥ ε) ≤ 2|H|e^{−2mε²} = δ`.
Solve for `m`: `m = (ln(2|H|) + ln(1/δ)) / (2ε²)`.
E.g. `|H|=10⁶`, `ε=0.05`, `δ=0.05`: `m = (ln(2·10⁶)+ln20)/(2·0.0025) = (14.51+3.00)/0.005 ≈ 3502` examples. → bound is distribution-free; shrinks with more data / smaller `|H|`.

## F. SVM: separable hard-margin ↔ soft-margin [§1.8] ⭐

Separable case: `min ½‖w‖² s.t. yⁱ(⟨w,xⁱ⟩+b) ≥ 1`. This is the **soft-margin** SVM `min ½‖w‖² + C Σξᵢ s.t. yⁱ(⟨w,xⁱ⟩+b) ≥ 1−ξᵢ, ξᵢ≥0` in the limit **`C→∞`** (any `ξᵢ>0` is infinitely penalized ⇒ forces `ξᵢ=0`, recovering hard constraints). Geometric margin `=1/‖w‖`, so minimizing `½‖w‖²` **maximizes the margin**. Finite `C` allows margin violations (needed for non-separable data) and trades margin vs. errors.

## G. EM for a 2-component GMM [§2.5–2.6] ⭐

Data `{xⱼ}`, model `Σ_y p(y)N(x;μ_y,σ_y²)`.
- **E-step** (responsibility of component `y` for point `x`): `αₓ(y)= p(y)N(x;μ_y,σ_y²) / Σ_ȳ p(ȳ)N(x;μ_ȳ,σ_ȳ²)`.
- **M-step** (weighted MLE): `p(y)=⟨αₓ(y)⟩`, `μ_y=⟨αₓ(y)x⟩/⟨αₓ(y)⟩`, `σ_y²=⟨αₓ(y)(x−μ_y)²⟩/⟨αₓ(y)⟩` (⟨·⟩ = average over data).
- **Why it works**: E-step sets `α=p_θ(y|x)` ⇒ KL=0 ⇒ bound touches `L(θ)`; M-step raises the bound ⇒ raises `L` ⇒ **monotone** likelihood increase. Hard-assignment (`αₓ(y)∈{0,1}`, equal fixed `σ`) ⇒ **k-means**.

## H. EM lower bound is tight iff `α=p_θ(y|x)` [§2.4]

`LB(θ,α) = L(θ) − E_Tᵐ[D_KL(αₓ(y) ‖ p_θ(y|x))]`. Since `D_KL ≥ 0` with equality iff the two distributions coincide, `LB=L` ⇔ `αₓ(y)=p_θ(y|x)` for all `x,y`. This is exactly what the E-step sets.

## I. Backprop as message passing [§3.3] ⭐

For a chain `z¹=x → z² → … → zᴸ=L`, define `δˡ=∂L/∂zˡ`. Backward recursion: `δᵢˡ = Σ_j δⱼˡ⁺¹ (∂zⱼˡ⁺¹/∂zᵢˡ)`. Example (softmax + cross-entropy): the gradient of the loss w.r.t. the logits is the clean `δ = ŷ − y` (predicted probs minus one-hot label) — the reason this output/loss pairing is standard. Each module needs only its local Jacobian `∂out/∂in` and `∂out/∂params`.

## J. Bias–variance of k-NN regression [§4.4]

`y=f(x)+ε`, `E[ε]=0`, `Var(ε)=σ²`; `hₘ(x)=(1/k)Σᵢ f(x_{n(x,i)}) + (1/k)Σε`.
- **bias²(x)** = `( f(x) − (1/k)Σᵢ f(x_{n(x,i)}) )²` — grows with `k` (averaging over farther, less-similar neighbors).
- **variance(x)** = `Var((1/k)Σε) = σ²/k` — shrinks with `k`.
- **Total** `= bias²(k) + σ²/k + σ²`. ⇒ classic **U-shaped** test error: small `k` = low bias/high variance (overfit), large `k` = high bias/low variance (underfit). Choose `k` by CV.

## K. Bagging vs boosting (which error do they cut?) [§4.4]

- **Bagging / Random Forest**: average **high-variance, low-bias** learners on bootstrap samples → cuts **variance** (decorrelation), leaves bias ≈ unchanged.
- **Boosting (AdaBoost / gradient boosting)**: sequentially add **high-bias, low-variance** weak learners, each correcting prior errors → cuts **bias** (can overfit if run too long).

## L. PAC approximation error of linear classifiers [§1.6, §4.2]

Gaussian classes, `H`=linear classifiers. **(a)** If `C₊=C₋` (equal covariance), the Bayes-optimal boundary is **linear** ⇒ `h_H` attains it ⇒ **approximation error = 0**. **(b)** If `C₊≠C₋`, the optimal boundary is **quadratic**, which `H` (linear) cannot represent ⇒ approximation error **increases (>0)**. **(c)** Zero approximation error whenever the optimal boundary is linear (e.g. equal covariances, or any linearly separable `p`).
