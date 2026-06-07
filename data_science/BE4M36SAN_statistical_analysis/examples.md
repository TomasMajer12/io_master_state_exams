# BE4M36SAN — Worked Examples

Exam-style worked problems, drawn from `materials/san_solved.pdf` and the lecture slides, with derivations. Cross-references to `summary.md` in brackets. These mirror the kind of reasoning Kléma asks for.

---

## A. Reading a regression summary (Boston `medv ~ lstat`) [§1.4–1.7]

```
            Estimate Std.Error t value Pr(>|t|)
(Intercept) 34.554    0.563    61.41   <2e-16
lstat       -0.950    0.039   -24.53   <2e-16
RSE = 6.216 (504 df);  R² = 0.544;  F = 601.6 (1,504)
```
- **Relationship**: each +1% of lower-status population lowers the predicted median home value by ≈ **$950** on average. **Significant**: huge `|t|`=24.5 (and F=601.6) ⇒ reject `H₀: β_lstat=0`.
- **Intercept**: predicted median value at `lstat=0` ≈ $34,554 — only meaningful if we have data near `lstat=0` (extrapolation risk) and the relationship is truly linear.
- **R²=0.544**: knowing `lstat` removes ~54% of the variance vs the mean-only forecast.
- **95% CI for β_lstat**: `−0.950 ± 2·0.039 = [−1.028, −0.872]` (precise factor `t_{0.025,504}=1.965≈2`). Doesn't contain 0 ⇒ significant.
- **Residual plot** curved/skewed ⇒ **linearity violated** → add `lstat²`. **Influential point** = one whose removal noticeably changes `β̂` (an outlier *or* a high-leverage point at extreme `X`); find via Cook's distance.

## B. F-test, model comparison, collinearity [§1.5–1.7]

Full model with 10 params, `R²=0.11`, `F` overall small (most `|t|<1`, only `X7` significant). Conclusion: do **not** trust individual p-values for selection (multiple-comparison risk); compare nested models by the **partial F-test** `F=(RSS₁−RSS₂)/(p₂−p₁) ÷ RSS₂/df₂`. If two predictors (e.g. `yrs.service`, `yrs.since.phd`) correlate 0.91 ⇒ **collinearity**: large VIF, unstable coefficients → drop one or combine.

## C. Spline continuity proof [§2.3]

A cubic spline with one knot `ξ`: show the two cubic pieces `f₁,f₂` join continuously. Continuity `f₁(ξ)=f₂(ξ)`, plus `f₁'(ξ)=f₂'(ξ)` and `f₁''(ξ)=f₂''(ξ)` (continuity up to 2nd derivative) is exactly the cubic-spline constraint; substituting the shared coefficients into both polynomials at `x=ξ` gives equality. A **natural** cubic spline adds `f''=0` beyond the boundary knots (linear extrapolation).

## D. Path-fitting: polynomial vs spline [§2.1–2.3]

A robot must pass through 6 points. **Degree-5 polynomial** (6 params) interpolates all 6 exactly and is smooth, but higher degrees oscillate (ill-posed/overfit). **Natural cubic spline with 4 knots** is the cleaner optimum — quadratic/cubic splines oscillate less than the high-degree polynomial; place knots roughly equally.

## E. Robust vs OLS regression — *which criterion?* [§4.5] ⭐ (Kléma)

| Setting | Criterion minimized | Best when |
|---|---|---|
| OLS (L2) | `Σ(yᵢ−xᵢᵀβ)²` | Gaussian, clean noise (then optimal) |
| LAD (L1) | `Σ|yᵢ−xᵢᵀβ|` | Laplace / heavy tails |
| Huber | quadratic-then-linear loss | small outliers |
| LMS / LTS | `med(rᵢ²)` / trimmed `Σr²(j)` | many/gross outliers (≈50% breakdown) |

On clean data `y=0.1x+N(0,0.1)` all agree. **Add one outlier** → OLS line tilts toward it; L1/Huber/Hampel barely move. **M-estimator link**: OLS = ML under Normal; LAD = ML under Laplace; both are M-estimators `argmin Σρ(rᵢ)` with `ρ=½r²` resp. `½|r|`. Caveat: if data really are Gaussian and clean, robust losses just **lose efficiency** (ARE<1).

## F. LDA vs logistic vs QDA on non-separable data [§3] ⭐

8-point 2-class set, not linearly separable. **LDA / logistic regression** give a **linear** boundary → ≥2 training errors (can't do better with a line). **QDA / polynomial / conic boundary** can separate them (a conic classifies all 8 correctly). Take-away: LDA/logistic = linear (low variance, higher bias); QDA = quadratic (more flexible, needs more data). For `K=2` and a 0/1 target, **linear regression ≡ LDA**.

## G. Kernel PCA on two concentric circles [§5.3] ⭐

Two classes = concentric circles in 2-D — not linearly separable. A radial feature `z=x²+y²` (or RBF kernel `k(x,x')=exp(−‖x−x'‖²/σ²)`) maps to a space where the classes differ in **one** coordinate ⇒ linearly separable; kernel PCA's first component separates them. `σ` matters: `σ=0.5` too small → overfit; `σ=2` captures the pattern → optimal. (Same idea powers **spectral clustering** / kernel k-means.)

## H. LDA discriminant, p=1 [§3.3]

Gaussian classes, common `σ`. `δₖ(x)=x·μₖ/σ² − μₖ²/(2σ²) + log πₖ` (linear in x). For `K=2`, `π₁=π₂`: boundary where `δ₁=δ₂` ⇒ `x=(μ₁+μ₂)/2`. Convert scores to probabilities by softmax `P(k|x)=e^{δₖ}/Σ e^{δₗ}`.

## I. Choosing `k` in clustering [§6.2]

`W(k)` (within-cluster SS) decreases monotonically with `k` → can't minimize it directly. Use the **elbow** of `W(k)`, **Hartigan** `H(k)=[W(k)/W(k+1)−1](m−k−1)` (pick smallest `k` with small `H`), or the **gap statistic** (compare `log W(k)` to a uniform/permuted null), or AIC/BIC with EM-GMM. For **spectral clustering**, use the **eigengap** (first big jump in sorted eigenvalues of `L`).

## J. Why spectral clustering beats k-means on spirals [§6.6]

Two interleaved spirals: k-means (spherical clusters) and complete-linkage fail. Build an RBF / k-NN similarity graph, form `L=D−S`; the **2nd smallest eigenvector** of `L` is an almost-perfect component indicator → k-means on the eigenvector coordinates separates the spirals trivially. Reason: spectral clustering solves a **relaxed balanced min-cut** (RatioCut/Ncut), which depends on connectivity, not on Euclidean compactness.

## K. Power analysis [§7.3]

Four quantities tied together: **α, power=1−β, n, effect size d**. Fix three, solve the fourth.
- *A-priori*: "How many users to detect Cohen's `d=0.5` at α=0.05 with power 0.8?" → solve for `n` (R `pwr`/G*Power).
- A **within-subject** design needs fewer participants than **between-subject** for the same power (lower error variance), at the cost of order effects (counterbalance).
- A statistically significant result with a tiny effect size may have **no practical significance**.

## L. Non-parametric test choice [§4.6]

Comparing two groups when normality is doubtful (e.g. Cauchy-distributed data): the **t-test** loses validity (its assumptions break), while **Mann–Whitney U** stays valid (distribution-free). For paired data: **sign test** (only signs, robust, low power) or **Wilcoxon signed-rank** (uses ranks, higher power, ARE 0.67 vs sign); if differences are Normal, the **paired t-test** is most powerful (ARE 0.95).
