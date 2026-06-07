# BE4M36SAN — Statistical Analysis (SAN) — Study Summary

**Language:** English-primary (slides + ISLR are English), with key Czech terms in parentheses for the oral (Czech) exam.

**Source materials:** Kléma's SAN lecture slides (regression, GLM, discriminant, dimensionality reduction, clustering), Pevný's robust-statistics & anomaly-detection slides, Míkovec's studies/power lecture, and `san_solved.pdf` — all in `materials/`. Primary textbook: **ISLR** (James, Witten, Hastie, Tibshirani, *An Introduction to Statistical Learning*); secondary **ESL** (Hastie/Tibshirani/Friedman). Cross-checked against authoritative sources (flagged inline).

**Structure.** Sections 1–6 mirror the six official exam sub-topics. Section 0 is statistical fundamentals (the `stat_min_eng.pdf` "minimum"); Section 7 is supplementary (anomaly detection, empirical studies & power). Worked numeric problems are in `examples.md`.

---

## 0. Statistical fundamentals (the "minimum")

From `stat_min_eng.pdf`:

- **Expected value vs. arithmetic mean** (*střední hodnota vs. aritmetický průměr*):
  - **Continuous**: `E[X]=∫x f(x)dx`. **Discrete**: `E[X]=Σₓ x·P(X=x)` — a probability-weighted average over the possible values (each value weighted by its probability mass).
  - `E[X]` is a property of the *distribution* (population) — a fixed deterministic number you could only compute if you knew the true `f`/`P`. The sample mean `x̄=(1/m)Σxᵢ` is a *statistic* (itself random — a different sample gives a different `x̄`) and an *estimate* of `E[X]`. It is **unbiased**: `E[x̄]=E[X]` for any `m`.
  - **Law of Large Numbers (in words)**: as the sample size grows, the sample mean of i.i.d. observations converges to the true expected value — average enough independent draws and the random fluctuations average out, leaving the population mean. Formally `x̄ → E[X]` as `m→∞`.
- **Covariance & correlation matrix** (*kovarianční a korelační matice*):
  - **Covariance** `Cov(X,Y)=E[(X−μ_X)(Y−μ_Y)]` **measures the direction of the linear co-movement** of two variables: when `X` is above its mean, is `Y` typically above (positive) or below (negative) its mean? `>0` move together, `<0` move oppositely, `=0` no *linear* relation (but they may still be dependent non-linearly). Its magnitude is **not standardized** — it depends on the units/scale of `X` and `Y`, so you can't compare covariances across variable pairs.
  - **Correlation** `ρ_XY=Cov(X,Y)/(σ_X σ_Y)` is covariance **standardized to `[−1,1]`** (divide out both standard deviations). Now it is **scale-invariant** and interpretable: `±1` = perfect linear relationship, `0` = no linear relationship. It quantifies the *strength* of the linear association, which raw covariance cannot.
  - **Covariance matrix** `Σ=E[(X−μ)(X−μ)ᵀ]` of a vector `X∈ℝᵖ`: `p×p`, **symmetric**, **positive semi-definite** (because for any `a`, `aᵀΣa=Var(aᵀX)≥0` — a variance can't be negative). Diagonal = variances, off-diagonal = covariances. Used in PCA (diagonalized), LDA (pooled), collinearity diagnostics.
- **CDF, pdf, quantile** (*distribuční funkce, hustota, kvantilová funkce*):
  - **CDF** `F(x)=P(X≤x)` — non-decreasing, right-continuous, `F(−∞)=0`, `F(+∞)=1`. "How much probability mass lies at or below `x`."
  - **pdf** (probability density function) `f(x)=F'(x)` — **the pdf is the derivative of the CDF**. It is *not* a probability itself; it is **probability per unit length**, a density. Properties: `f(x)≥0` and `∫f=1`.
  - **Quantile function** `Q(p)=F⁻¹(p)`, `p∈(0,1)` — inverse of the CDF: `Q(p)` is the smallest `x` with `F(x)≥p`. In words: "what value is the variable below with probability `p`?" Examples: `Q(0.5)`=median, `Q(0.25)`/`Q(0.75)`=lower/upper quartiles.
- **p-value** (*p-hodnota*):
  1. **Via the test statistic** `T`: the probability that, *under the null hypothesis H₀*, the test statistic `T` takes its observed value or a value even more extreme (i.e. more unfavorable to H₀).
  2. **As a threshold**: the **smallest significance level `α` at which H₀ would still be rejected** (*nejmenší hladina významnosti, při které zamítneme H₀*). So you can reject at any `α ≥ p` and cannot at any `α < p`.
  - **It is NOT** the probability that H₀ is true. Significance level `α` = the tolerated false-positive (Type I error) rate, chosen *before* seeing data; `p=0.045 < α=0.05` ⇒ reject H₀.
- **Confidence interval** (*interval spolehlivosti*): a 100(1−α)% CI is a procedure that, over repeated sampling, contains the true parameter in 100(1−α)% of samples. (It is **not** "95% probability the parameter is in this fixed interval".)
- **Type I vs Type II error** (*chyba I./II. druhu*): Type I = reject true H₀ (rate α); Type II = fail to reject false H₀ (rate β); **power** = 1−β.

---

## 1. Multiple linear regression

> **Official sub-topic:** *Multiple linear regression. Model and assumptions. Qualitative independent variables, collinearity and outliers. Decide whether a model is useful. Overfitting, feature selection, model regularization.*

### 1.1 The model

**Simple** (one predictor): `Y = β₀ + β₁X + ε`. **Multiple**: `Y = β₀ + β₁X₁ + … + β_pX_p + ε`.
- `βⱼ` = average change in `Y` for a **one-unit increase in `Xⱼ`, holding all other predictors fixed**.
- Prediction `ŷ = β̂₀ + β̂₁x₁ + … + β̂_px_p`.

### 1.2 Assumptions (*předpoklady*)

A linear model is **linear in the parameters** `β` by construction (you may still include `X²`, `log X`, splines as predictors). On top of that, OLS inference relies on the assumptions below — each with how to detect and how to fix it.

**1. Linearity** (*linearita*). The relationship between predictors and response is linear: the dependent variable changes proportionally with each `Xⱼ`, forming a straight-line trend. Curved or irregular patterns cause underfitting and biased predictions.
- *Detect:* residuals-vs-fitted plot (a curved/U-shaped pattern signals non-linearity).
- *Fix:* transform variables (`log`, `√`), add polynomial/spline terms, or use a non-linear model / GAM (→ Topic 2).

**2. Independence of errors** (*nezávislost chyb*). Residuals must not correlate with each other across observations. Correlated (autocorrelated) errors mean the model missed temporal or patterned structure, which inflates significance and misleads conclusions — common in time-series data.
- *Detect:* Durbin–Watson test (≈2 ⇒ no autocorrelation; ≪2 positive, ≫2 negative), ACF plot of residuals.
- *Fix:* add lagged predictors / omitted structure, GLS, or proper time-series models (ARIMA).

**3. Homoscedasticity** (*homoskedasticita*). The variance of the residuals stays constant across all levels of the predictors — residuals should be evenly scattered. Increasing/decreasing spread (heteroscedasticity) makes coefficient estimates and standard errors unreliable.
- *Detect:* residuals-vs-fitted / scale-location plot (fan or cone shape), Breusch–Pagan test.
- *Fix:* transform `Y` (`log`, `√`), weighted least squares (WLS) or GLS, or heteroscedasticity-robust standard errors.

**4. Normality of errors** (*normalita reziduí*). The residuals follow a normal distribution. This supports valid confidence intervals, hypothesis tests, and p-values; skewed or peaked distributions weaken inference. Needed for the t/F tests and CIs, **not** for the point estimate `β̂` itself (Gauss–Markov).
- *Detect:* Q–Q plot (points on the 45° line ⇒ normal), histogram, Shapiro–Wilk test.
- *Fix:* transform `Y`, rely on the CLT for large `n` (inference stays ≈ valid), bootstrap, or robust regression (→ Topic 4).

**5. No (perfect) multicollinearity** (*multikolinearita*). The predictors are not highly correlated with one another. Strong collinearity inflates coefficient variance and makes it hard to assess each predictor's true contribution (signs can even flip).
- *Detect:* VIF `=1/(1−Rⱼ²)` (>5–10 ⇒ problematic), correlation matrix, condition number of `XᵀX`.
- *Fix:* drop or combine redundant predictors, PCA/PCR, or regularization (ridge/lasso, → §1.9).


### 1.3 Estimation — Ordinary Least Squares (OLS)

Minimize the **residual sum of squares** `RSS = Σ(yᵢ − ŷᵢ)²`. For simple regression:

```
β̂₁ = Σ(xᵢ−x̄)(yᵢ−ȳ) / Σ(xᵢ−x̄)²,   β̂₀ = ȳ − β̂₁x̄
```

Note `β̂₁ = Cov(X,Y)/Var(X)` and the line passes through the centroid `(x̄,ȳ)`.

**Multiple regression (matrix form).** Stack the data: `X` is the `m×(p+1)` **design matrix** (first column all 1s for the intercept), `y` the `m×1` response. Then `RSS = ‖y−Xβ‖²`; setting the gradient `∂RSS/∂β = −2Xᵀ(y−Xβ)` to zero gives the **normal equations** `XᵀXβ = Xᵀy`, hence:

```
β̂ = (XᵀX)⁻¹Xᵀy
```

Fitted values `ŷ = Xβ̂ = Hy` with **hat matrix** `H = X(XᵀX)⁻¹Xᵀ` — the orthogonal projection of `y` onto the column space of `X` (residuals `y−ŷ` are orthogonal to it; `H`'s diagonal = **leverages**, used in Cook's distance, §1.7).

**Connection to collinearity.** The formula needs `XᵀX` to be **invertible**. If two predictors are *perfectly* collinear, `XᵀX` is singular ⇒ no unique solution. If they are *nearly* collinear, `XᵀX` is nearly singular ⇒ its inverse has huge entries ⇒ coefficient variances blow up and signs get unstable — exactly the multicollinearity/VIF problem (§1.2, §1.7). Regularization (ridge adds `λI` to `XᵀX`, making it invertible again) is one remedy.

**Generalized least squares (GLS)** handles heteroscedasticity/correlated errors (`Σ≠σ²I`): minimize `(y−Xβ)ᵀΣ⁻¹(y−Xβ)` ⇒ `β̂=(XᵀΣ⁻¹X)⁻¹XᵀΣ⁻¹y`; WLS is the diagonal-`Σ` special case.

### 1.4 Accuracy of estimates & hypothesis testing

- **Standard error** (*směrodatná chyba*) = the std. dev. of `β̂ⱼ` across repeated samples. Simple regression: `SE(β̂₁)² = σ²/Σ(xᵢ−x̄)²`. General matrix form: `Var(β̂)=σ²(XᵀX)⁻¹`, each `SE(β̂ⱼ)=√` of a diagonal entry.
  - *Intuition:* SE shrinks with **less noise** (`σ²↓`), **more data**, and **more spread-out predictors** (large `Σ(xᵢ−x̄)²` pins the slope down). The `(XᵀX)⁻¹` shows again why collinearity inflates SEs.
- `σ` is estimated by the **residual standard error** `RSE = √(RSS/(m−p−1))`; the denominator `m−p−1` is the **degrees of freedom** (observations minus estimated parameters: `p` slopes + intercept).
- **Confidence interval**: `β̂ⱼ ± t_{1−α/2, m−p−1}·SE(β̂ⱼ)` (≈ `β̂ⱼ ± 2·SE` for 95%). Interpretation per Topic 0: over repeated sampling, 95% of such intervals cover the true `βⱼ`.
- **t-test for a single coefficient** (*t-test koeficientu*): does `Xⱼ` contribute, **given the other predictors**?
  - `H₀: βⱼ=0` (no effect) vs `H₁: βⱼ≠0`. Statistic `t = β̂ⱼ/SE(β̂ⱼ) ~ t_{m−p−1}` — how many SEs `β̂ⱼ` sits from 0.
  - **Logic:** large `|t|` ⇒ small p-value ⇒ reject `H₀` ⇒ predictor is **significant**. A *small p-value is what licenses rejecting H₀*; a large one does **not** prove `βⱼ=0` (absence of evidence ≠ evidence of absence). Set `α` *before* seeing data.
  - **Conditional:** "`βⱼ=0`" means "`Xⱼ` adds nothing *beyond* the other predictors" → a predictor can be significant alone but not in the full model (confounding/collinearity).
  - **Why `t`, not `z`?** We plug in the *estimated* `σ` (RSE), not the true one; the extra uncertainty fattens the tails → `t`-distribution. As `m→∞`, `t→` normal.
  - The t-tests check coefficients **one at a time** → with many predictors, multiple-comparison risk → need the global **F-test** (§1.5).

### 1.5 Is the model useful? (overall)

- **R²** = `(TSS−RSS)/TSS = 1 − RSS/TSS` = fraction of variance explained (`TSS=Σ(yᵢ−ȳ)²`). In simple regression `R²=r²` (squared correlation). **R² always increases with more predictors** — so it cannot select model size.
- **Variance decomposition** (the basis of both `R²` and `F`): `TSS = ESS + RSS`, i.e. `Σ(yᵢ−ȳ)² = Σ(ŷᵢ−ȳ)² + Σ(yᵢ−ŷᵢ)²` (total = explained + residual).
- **F-test** (is *at least one* predictor useful?): `H₀: β₁=…=β_p=0` vs `H₁:` at least one `βⱼ≠0`.
  ```
  F = (TSS−RSS)/p ÷ RSS/(m−p−1)
  ```
  Compared with `F(p, m−p−1)`.
  - **What it is:** a ratio of two variance estimates — **numerator** = explained variance *per predictor* (`(TSS−RSS)/p` = ESS spread over the `p` predictors used), **denominator** = leftover noise variance *per residual df* (`RSS/(m−p−1)`, an estimate of `σ²`).
  - **Why `E[F]≈1` under H₀:** if no predictor matters, the "explained" variance is itself just noise, so numerator ≈ denominator ≈ `σ²` ⇒ their ratio ≈ 1. If some predictor *does* matter, the numerator captures real signal and grows >> denominator ⇒ **large F**.
  - **Reading it:** large `F` (small p-value) ⇒ reject `H₀` ⇒ the model explains more than chance. Unlike `R²`, `F` penalizes extra predictors through the `/p` (a useless predictor adds little to ESS but costs a df), so it doesn't automatically reward bigger models.

- **Why not just use the per-predictor p-values?** For large `p`, multiple-comparisons risk → false discoveries; the global F-test guards against this.

### 1.6 Qualitative (categorical) predictors (*kvalitativní prediktory*)

- **Binary** → one 0/1 dummy. `β₀` = mean of baseline group; `β₀+β₁` = mean of the other; `β₁` = group difference. (A −1/+1 coding gives identical predictions, different β interpretation.)
- **`l` levels** → `l−1` dummies; the omitted level is the **baseline**. Each `βⱼ` = mean difference of level `j` vs baseline.
- **Interactions** (*interakce*): add `Xⱼ·Xₖ` term (e.g. `TV×radio`). Captures synergy; the effect of one predictor then depends on the other. **Hierarchy principle**: if you include an interaction, also keep the main effects.

### 1.7 Diagnostics: outliers, influential points, collinearity

- **Residual plots** (residuals vs fitted) reveal non-linearity, heteroscedasticity, outliers.
- **Outlier** = large residual; **influential point** = large effect on the fit (conceptually different but often overlap). **Cook's distance** `Dᵢ` summarizes the change in all fitted values when obs. `i` is dropped — remove large-`Dᵢ` points.
- **Multicollinearity** (*multikolinearita*) — correlated predictors inflate coefficient variance & make interpretation hazardous. Detect with **VIF** (variance inflation factor): `VIFⱼ = 1/(1−R²ⱼ)`, where `R²ⱼ` is from regressing `Xⱼ` on all other predictors. `VIF>5–10` ⇒ problematic; consider dropping/combining.
- **Anscombe's quartet** — four datasets with identical regression summaries but completely different scatter ⇒ **always plot the data**.

### 1.8 Feature/model selection (overfitting)

- **Training error (RSS, R²) underestimates test error** → can't pick model size directly. Use a criterion balancing fit & size:
  - **Adjusted R²** = `1 − [RSS/(m−p−1)]/[TSS/(m−1)]` (penalizes extra predictors; maximize).
  - **Mallow's Cₚ, AIC, BIC** (minimize). **AIC** `∝ RSS + 2p·σ̂²` — penalty `2p`, aims at best *prediction* (lighter penalty → slightly larger models). **BIC** `∝ RSS + log(m)·p·σ̂²` — penalty `log(m)·p`, so for `m>7` it penalizes complexity *harder* than AIC → prefers **smaller/sparser** models (aims to recover the true model).
  - **Cross-validation** (most general, assumption-free — directly *measures* test error):
    - **hold-out/validation**: single split (e.g. 70:30); simple but noisy, wastes data.
    - **k-fold**: split into `k` folds, train on `k−1`, test on 1, average over folds (`k=5` or `10`); uses all data, low-variance estimate.
    - **LOOCV** (`k=m`): each point its own test fold; nearly unbiased but high-variance/expensive (closed-form shortcut for linear regression via leverages).
- **Subset selection**: best-subset `O(2^p)` (infeasible); **forward stepwise** `O(p²)` (start null, add best); **backward stepwise** `O(p²)` (start full, drop least significant; needs `m>p`).

### 1.9 Regularization / shrinkage (*regularizace, smršťování*)

Fit all `p` predictors but **shrink** coefficients toward 0 (reduces variance, fights overfitting).

- **Ridge regression** (`ℓ₂`): minimize `RSS + λΣβⱼ²`. Shrinks all coefficients smoothly toward 0 but **never exactly to 0** → **keeps all predictors**. Not scale-invariant → **standardize predictors first** (else small-unit predictors get over-penalized). Closed form `β̂=(XᵀX+λI)⁻¹Xᵀy` — adding `λI` makes the matrix always invertible, so ridge **also fixes collinearity / the `p>m` case** where OLS fails. `λ` chosen by CV. Best when **many** predictors each contribute a little.
- **Lasso** (`ℓ₁`): minimize `RSS + λΣ|βⱼ|`. The `ℓ₁` penalty drives some coefficients **exactly to 0** ⇒ **sparse model + automatic feature selection** (shrinkage *and* selection at once). **Geometry:** the solution must lie in the penalty's constraint region; the `ℓ₁` region is a **diamond with corners on the axes**, and the elliptical RSS contours tend to first touch it *at a corner* → some coordinates exactly 0. Ridge's `ℓ₂` region is a **circle/sphere** (no corners) → coefficients shrink but stay nonzero. Best when only a **few** predictors truly matter (sparse truth). No closed form (`ℓ₁` non-differentiable at 0).
- **Elastic net**: combines both penalties `λ(α·Σ|βⱼ| + (1−α)·Σβⱼ²)` — gives lasso-style sparsity while handling correlated groups of predictors better (where lasso arbitrarily picks one).
- **Bias–variance trade-off**: `E[(y₀−f̂(x₀))²] = Var(f̂) + Bias(f̂)² + Var(ε)`. Increasing `λ` ↑bias ↓variance. Choose `λ` minimizing CV error (`λ_min`), or the sparser `λ_1se` (largest `λ` within 1 SE of min).
- **Neither ridge nor lasso dominates**: lasso wins when only few predictors matter; CV decides.
- **Cross-validation**: hold-out (70:30, large data) or **k-fold** (split into k folds, train on k−1, test on 1, average). Training error is a positively-biased estimate of generalization error.

---

## 2. Non-linear regression

> **Official sub-topic:** *Non-linear regression, polynomial regression, splines, local regression.*

"The truth is never linear!" Move beyond linearity, mostly via **basis expansion** (replace `X` by functions `b₁(X),…,b_K(X)` and fit a linear model in them — so all the linear-model machinery still applies).

### 2.1 Polynomial regression

`yᵢ = β₀ + β₁xᵢ + β₂xᵢ² + … + β_d xᵢᵈ + εᵢ`. Create `X,X²,…,Xᵈ` and run multiple regression. We care about the **fitted curve** `f̂(x₀)` (pointwise SE `f̂(x₀)±2·se`) not individual β's. Choose `d` low or by CV.
- **+** can fit any smooth shape with high enough degree. **−** high degrees (≥6) overfit and oscillate wildly, especially at boundaries.

### 2.2 Step functions

Cut `X` into bins via dummy indicators `Cₖ(X)=I(cₖ≤X<cₖ₊₁)` and fit a piecewise-**constant** model. Easy, good for interactions (e.g. `I(Year<2005)·Age`), but the choice of cut-points is arbitrary and the fit is not smooth.

### 2.3 Splines (*spliny*) ⭐

**Piecewise polynomials** joined at **knots** `ξₖ` with smoothness constraints.

- **Regression spline of degree d** = piecewise degree-`d` polynomial, continuous together with derivatives up to order `d−1` at each knot.
- **Linear spline**: basis `b₁(x)=x`, `b_{k+1}(x)=(x−ξₖ)₊` (truncated power, `()₊` = positive part).
- **Cubic spline**: basis `x, x², x³, (x−ξₖ)³₊`. A cubic spline with `K` knots has `K+4` degrees of freedom; visually the smoothest (discontinuities in the 3rd derivative are invisible).
- **Natural cubic spline**: adds the constraint that it is **linear beyond the boundary knots** (2×2 extra constraints) → less wild at the edges; `K` knots ⇒ `K` df. Preferred over high-degree polynomials (compare a degree-14 polynomial vs a natural spline with 15 df — the spline behaves far better at boundaries).
- **Knot placement**: choose `K` and place at quantiles of `X`.
- **Smoothing spline** (*vyhlazovací splajn*): minimize `Σ(yᵢ−g(xᵢ))² + λ∫g''(t)²dt` (RSS + roughness penalty). Solution is a natural cubic spline with a knot at **every** unique `xᵢ`; the single tuning parameter `λ` controls wiggliness (`λ→0` interpolates, `λ→∞` → linear). Specify effective df instead of `λ`.

### 2.4 Local regression (LOESS) (*lokální regrese*)

Fit many **local** weighted-least-squares models along the range of `X`. At a target `x₀`:
1. take the fraction `s=k/m` of training points nearest `x₀`;
2. assign weights `Kᵢ₀=K(xᵢ,x₀)` via a kernel (far points get 0);
3. fit weighted LS `min Σ Kᵢ₀(yᵢ−β₀−β₁xᵢ)²`;
4. predict `f̂(x₀)=β̂₀+β̂₁x₀`.
A **memory-based / lazy** method (needs all training data at prediction time). Span `s` controls smoothness.

### 2.5 Generalized Additive Models (GAMs)

`yᵢ = β₀ + f₁(xᵢ₁) + f₂(xᵢ₂) + … + f_p(xᵢ_p) + εᵢ` — each `fⱼ` a flexible nonlinear function (e.g. a spline) but the model stays **additive**, preserving interpretability (inspect each `fⱼ` separately). Loses the ability to model interactions unless added explicitly. Extends to classification via a logit link (→ §3, GLMs).

### 2.6 When to use which (one-liner)

**Polynomial** = simple global curve (but oscillates at boundaries); **splines** = flexible *and* stable, prefer **natural/smoothing** splines (best boundary behaviour); **LOESS** = smooth local fits, no global formula (lazy); **GAM** = multivariate + interpretable (additive, no interactions unless added).

---

## 3. Discriminant analysis: LDA, QDA, logistic regression

> **Official sub-topic:** *Discriminant analysis. LDA, QDA and logistic regression.* ⭐

Goal: classify `X∈ℝ^p` into a nominal `Y∈{1,…,K}`; often probabilistic `P(Y=k|X)`. Two philosophies: **discriminative** (model `P(Y|X)` directly — logistic regression) vs **generative** (model `P(X|Y)` per class + Bayes — LDA/QDA/naïve Bayes).

### 3.1 Why not linear regression for classification?

Coding `Y∈{0,1}` and regressing works for binary (`E[Y|X]=P(Y=1|X)`, and it equals LDA), but in general can give probabilities `<0` or `>1`, is outlier-sensitive, and **masks classes** with ≥3 categories. → use logistic regression.

### 3.2 Logistic regression (*logistická regrese*) — discriminative

**The problem it solves:** we want `p(X)=P(Y=1|X)` but a linear function is unbounded — it would produce "probabilities" outside `[0,1]`. The fix: don't model the probability itself as linear, model its **log-odds (logit)**:

```
log( p(X)/(1−p(X)) ) = β₀+β₁X₁+…+β_pX_p    ⇒    p(X)=1/(1+e^{−(β₀+…)})
```

**Why the log-odds?** The **odds** `p/(1−p)` map probability `(0,1)` to `(0,∞)` (`p=0.5` → odds 1; `p=0.9` → odds 9); taking the log stretches that to `(−∞,∞)` — *exactly the range a linear function produces*. So the logit is the natural quantity to make linear. Inverting it gives the S-shaped **sigmoid**, and `p(X)∈(0,1)` always.

- **Shape & sensitivity:** the sigmoid is flat near 0 and 1 and **steepest at `p=0.5`** — the model is most sensitive near the decision boundary, where sensitivity belongs (unlike linear regression, which is equally sensitive everywhere → its outlier problem).
- **Interpretation:** a one-unit increase in `Xⱼ` adds `βⱼ` to the *log-odds*, i.e. **multiplies the odds by `e^{βⱼ}`** (the **odds ratio**). `βⱼ>0` ⇒ raises the probability. The effect on the *probability itself* is not constant — it is largest near `p=0.5`, small in the flat tails.
- **Decision boundary:** predict class 1 when `p(X)>0.5` ⇔ `β₀+βᵀx>0`. The boundary `β₀+βᵀx=0` is a hyperplane ⇒ **logistic regression is a linear classifier** — the sigmoid bends the probabilities, not the boundary.
- **Learning — maximum likelihood:** there is no RSS to minimize; instead choose `β` making the observed labels most probable: `ℓ(β)=∏_{yᵢ=1}p(xᵢ)·∏_{yᵢ=0}(1−p(xᵢ))` (each 1 should get high `p`, each 0 low `p`). No closed form ⇒ iterative optimization (e.g. **IRLS** / Newton); the log-likelihood is concave ⇒ unique optimum. It is a **GLM**: logit link + binomial response (§7.1).
- **Confounding** (*matení/confounding*): a coefficient's sign can flip between simple and multiple models (ISLR `student` example: students riskier *overall*, safer *at fixed balance*, because student status correlates with balance) — multiple-model coefficients are conditional on the other predictors (same lesson as §1.4).
- **Multiclass**: symmetric **softmax** form `P(Y=k|X)=e^{βₖᵀx}/Σⱼe^{βⱼᵀx}` (a.k.a. **multinomial/softmax regression**), or asymmetric with a pivot class (`K−1` binary models vs a baseline).

### 3.3 Discriminant analysis via Bayes — generative

Bayes: `P(Y=k|X=x) = πₖ fₖ(x) / Σⱼ πⱼ fⱼ(x)`, where `πₖ=P(Y=k)` is the **prior** and `fₖ(x)=P(X=x|Y=k)` the class-conditional **density**. Model `fₖ` as **Gaussian**:

- **LDA** — Gaussians with a **common covariance `Σ`** for all classes ⇒ **linear** discriminant.
  - `p=1`: discriminant score `δₖ(x) = x·μₖ/σ² − μₖ²/(2σ²) + log πₖ`; assign to the largest `δₖ`. For `K=2`, equal priors, boundary at `x=(μ₁+μ₂)/2`.
  - `p>1`: density `f(x)=(2π)^{−p/2}|Σ|^{−1/2} exp(−½(x−μₖ)ᵀΣ⁻¹(x−μₖ))`; discriminant `δₖ(x)=xᵀΣ⁻¹μₖ − ½μₖᵀΣ⁻¹μₖ + log πₖ` — **linear in x**.
  - **Parameters learned**: `π̂ₖ` (class frequencies), `μ̂ₖ` (class means), `Σ̂` (pooled within-class covariance).
- **QDA** — Gaussians with **class-specific `Σₖ`** ⇒ **quadratic** boundary. More flexible (lower bias) but more parameters (higher variance) — needs more data.
- **Naïve Bayes** — `fₖ(x)=∏ⱼ fⱼₖ(xⱼ)` (conditional independence ⇒ diagonal `Σₖ`). Good when `p` is very large.
- **Scores → probabilities**: `P̂(Y=k|x)=e^{δₖ(x)}/Σₗe^{δₗ(x)}`.

### 3.4 Evaluation of classifiers ⭐

- **Confusion matrix**, accuracy. For **imbalanced classes / unequal misclassification costs**, accuracy misleads (e.g. Credit data: always-"No" gives 3.3% error but 100% of defaulters missed).
- **Change the threshold** (not fixed at 0.5) to trade FNR vs FPR per the loss function.
- **Sensitivity = TPR = TP/P**; **Specificity = TN/N**; **FPR = 1−specificity**.
- **ROC curve** (TPR vs FPR over all thresholds); **AUC/AUROC** ∈ [0,1] (0.5 = random) = probability a random positive is ranked above a random negative. Better than accuracy for imbalanced data.

### 3.5 Where they sit - comparison

The organizing axis is the **bias–variance trade-off**: how many assumptions does each method make, and how much data does it need?

- **Logistic regression** — discriminative, **linear boundary**. The default for binary classification: no assumption on how `X` is distributed, interpretable coefficients (odds ratios), well-calibrated probabilities. *Weak when:* classes are **perfectly/well separated** (MLE coefficients diverge to ±∞) or the sample is **small**; multiclass needs softmax.
- **LDA** — generative, **linear boundary**. Shines exactly where logistic regression struggles: **small `n`**, **well-separated classes**, **`K>2`** (native multiclass; bonus low-dim projections of the data). Closed-form learning (count + average — no iteration). *Needs:* Gaussian-with-shared-`Σ` to be roughly true. For `K=2` equivalent to linear regression on a 0/1 target.
- **QDA** — generative, **quadratic boundary**. Middle ground between LDA and non-parametric: per-class `Σₖ` ⇒ more flexible (lower bias) but `~K·p²/2` covariance parameters ⇒ needs **much more data** (higher variance). Pick when class shapes genuinely differ and the sample is large.
- **Naïve Bayes** — generative, assumes **feature independence** (diagonal `Σₖ`): only `p` 1-D densities per class ⇒ the **very-high-`p`** specialist (text, sparse data) where full covariances are hopeless. Assumption usually false, ranking often still good.
- **kNN** — non-parametric, **no model at all**. Most flexible; wins when the boundary is truly irregular and data plentiful. Loses interpretability, suffers from the curse of dimensionality, no trustworthy probabilities.

**One-sentence summary for the oral:** low variance / higher bias on one end (logistic regression, LDA — linear boundaries, strong or few assumptions), higher variance / lower bias on the other (QDA, kNN); choose by **sample size, class separation, dimensionality (`p` vs `n`), and plausibility of the Gaussian assumption**.

---

## 4. Robust statistics

> **Official sub-topic:** *Robust statistics. Robust estimators of location and scale, M-estimators. Robust regression. Non-parametric tests.* ⭐ (Kléma asked: methods of robust regression, compare with linear regression, *which criterion is optimized*, M-estimators, when to use/not.)

**Goal**: estimators unaffected by **outliers** (even malicious) or **incorrect distributional assumptions**. Motivation: for `0.9·N(0,1)+0.1·N(0,100)` the **mean** is wrecked but the **median** is stable.

### 4.1 How to compare estimators

- **Breakdown point** (*bod selhání*): the largest fraction of observations that can be given arbitrary values without driving the estimator to a useless limit.
  - mean: **0%**; median: **50%**.
- **Influence function** `IF(x;p,η)=limₑ→₀ [η((1−ε)p+εδₓ)−η(p)]/ε` — effect of an infinitesimal contamination at `x`. **Gross error sensitivity** `GES=supₓ|IF(x)|` (bounded ⇒ robust).
- **Asymptotic relative efficiency (ARE)** `ARE(η̂₁,η̂₂)=V₂/V₁` (ratio of variances). E.g. **ARE(median, mean) = 2/π ≈ 0.637** under normality (the price of robustness: the median needs ~1.57× more data to match the mean *when data really are Gaussian*).

### 4.2 Robust estimators of **location** (*polohy*)

| Estimator | Definition | Breakdown | ARE (Normal) |
|---|---|---|---|
| **Mean** | `(1/n)Σxᵢ` | 0% | 1.0 (optimal if Gaussian) |
| **Median** | middle value | 50% | 0.637 |
| **q%-trimmed mean** | mean of the data within `[x_{q%}, x_{1−q%}]` (drop tails) | q% | ~0.94 |
| **q%-Winsorized mean** | replace tail values by the bounds, then mean | q% | (between) |
| **Hodges–Lehmann** | `median{ (xᵢ+xⱼ)/2 }` over all pairs | ~29% | 0.955 |

### 4.3 Robust estimators of **scale** (*měřítka/rozptylu*)

| Estimator | Definition | Breakdown | ARE | Consistency factor (Normal) |
|---|---|---|---|---|
| **Sample std. dev.** | `√(1/(n−1)·Σ(xᵢ−x̄)²)` | 0% | 1.0 (optimal) | — |
| **MAD** (median abs. deviation) | `med{ |xᵢ − med{x}| }` | **50%** | 0.37 | `σ̂ = 1.4826·MAD` |
| **Sₙ** (Rousseeuw–Croux) | `medᵢ medⱼ |xᵢ−xⱼ|` | **50%** | ≈0.58 | `σ̂ ≈ 1.1926·Sₙ` |
| **Qₙ** (Rousseeuw–Croux) | 1st quartile of `{|xᵢ−xⱼ|: i<j}` | **50%** | ≈0.82 | `σ̂ ≈ 2.2219·Qₙ` |

> **⚠ Correction vs slides:** the lecture slide lists Sₙ with breakdown 29% and factor 1.0483 — these are typos. The standard Rousseeuw–Croux values are **breakdown 50%, factor 1.1926, Gaussian efficiency ≈58%** (Qₙ: 50%, 2.2219, ≈82%). *Verified against Rousseeuw & Croux (1993) / robustbase.*

### 4.4 M-estimators (*M-odhady*) — the unifying idea ⭐

**Step 1 — why is the mean so popular?** Because it is the **maximum-likelihood (ML) estimate of location for Normal data**: writing the Gaussian log-likelihood of `μ` and maximizing it turns into *minimizing* `Σ(xᵢ−μ)²` — whose solution is the mean. So **squared loss isn't arbitrary — it's the Gaussian assumption in disguise.** (Likewise, minimizing `Σ|xᵢ−μ|` gives the **median**, which is the ML estimate under Laplace noise.)

**Step 2 — generalize.** Flip the logic: pick a loss shape `ρ` first, equivalent to assuming noise `x ~ (1/Z)·exp(−ρ((x−μ)/σ))` (exponential-type family). The ML estimate of location is then

```
μ̂ = argmin_μ Σ ρ((xᵢ−μ)/σ)      ⇔ (set derivative to 0)      Σ ψ((xᵢ−μ)/σ) = 0,  ψ = ρ'
```

Any estimator of this form is an **M-estimator** ("generalized **M**aximum likelihood"). Two readings to internalize:
- **Choosing `ρ` = choosing the noise distribution you believe in.**
- **`ψ` = each point's *pull* on the estimate** (it has the shape of the influence function). The estimate settles where all pulls balance — so robustness is entirely about *whether the pull of a far-away point is bounded.*

| `ρ(x)` | `ψ=ρ'(x)` | ⇒ estimator | noise model |
|---|---|---|---|
| `½x²` | `x` | **mean** | Normal |
| `½|x|` | `sgn(x)` | **median** | Laplace (→ `Σ sgn(xᵢ−μ)=0`) |
| **Huber**: `½x²` if `|x|<a`, else `a|x|−a²/2` | `x` if `|x|<a`, else `a·sgn(x)` | Winsorizing | Normal centre, heavy tails |
| **Tukey/Hampel** (redescending) | →0 for large `|x|` | full outlier rejection | — |

**Read each row through `ψ` = pull:**
- **Mean** (`ψ=x`): pull grows **without bound** — a point at 10⁶ pulls a million times harder than one at 1. That's the 0% breakdown restated.
- **Median** (`ψ=sgn(x)`): every point pulls ±1 **regardless of distance** — maximally democratic ⇒ robust but inefficient (ignores how far points are even when that's informative).
- **Huber**: best-of-both compromise — **quadratic near 0** (acts like the mean on the well-behaved bulk ⇒ efficient), **linear in the tails** (pull **clamped at `a`** — the same clamping as Winsorizing, §4.2). **Convex** ⇒ unique minimum, easy optimization. Tuning: `a=1.345σ` gives 95% efficiency under normality.
- **Tukey/Hampel (redescending)**: pull *returns to 0* for gross outliers — they are effectively **deleted** (cf. trimming). Strongest rejection, but `ρ` is **non-convex** ⇒ local minima, optimization needs a good start (e.g. from median/Huber).

**Caveats** (the "when not to use"):
1. needs a **scale estimate** `σ` inside `ρ((x−μ)/σ)` — and it must itself be robust (MAD·1.4826, §4.3), else outliers corrupt the scale and through it the location;
2. **efficiency loss** if data are actually clean Gaussian (then the mean was optimal);
3. **tuning constants** must be chosen (Huber's `a`);
4. redescending versions add **non-convexity**.

### 4.5 Robust regression (*robustní regrese*) ⭐

OLS itself is an M-estimator: the generative model `y=xᵀβ+ε, ε~N(0,σ²)` ⇒ `β̂=argmin Σ(xᵢᵀβ−yᵢ)²` (L2). Change the assumed noise / criterion to gain robustness:

- **L1 / Least Absolute Deviations** (Laplace noise): `β̂=argmin Σ|xᵢᵀβ−yᵢ|` (median regression).
- **Huber regression**: Huber loss on residuals (quadratic-then-linear).
- **Least Median of Squares (LMS)**: `β̂=argmin medᵢ (xᵢᵀβ−yᵢ)²` — very high breakdown (~50%).
- **Least Trimmed Squares (LTS)**: `β̂=argmin Σ_{(j)} (residuals²)` over the smallest residuals (trim the largest) — high breakdown, more efficient than LMS.
- **Caveat — leverage points:** L1/Huber protect against outliers in **`y`** (vertical); an outlier in **`x`** (leverage point, §1.7) can still drag them. The high-breakdown methods (LMS/LTS) resist those too.
- **When to use:** with outliers/heavy-tailed noise, robust regression beats OLS; **when noise really is Gaussian and clean, OLS is optimal** (MLE, Gauss–Markov) and robust methods just lose efficiency (the exact point Kléma probes). Practical recipe: fit both — if they agree, report OLS; if they differ, the disagreeing points are your outliers.

### 4.6 Robust correlation & non-parametric tests (*neparametrické testy*)

**Correlation:**
- **Pearson** `ρ=(1/n)Σ[(xᵢ−x̄)(yᵢ−ȳ)/(σ_Xσ_Y)]` — measures *linear* association, **breakdown 0** (outlier-sensitive).
- **Spearman** `r_s` — Pearson on **ranks**; captures monotone (not just linear) association; `r_s√((n−2)/(1−r_s²)) ~ t_{n−2}`.
- **Kendall's τ** — uses only **concordant/discordant** pairs: `τ=(n_c−n_d)/C(n,2)`; `τ ~ N(0, 2(2N+5)/(9N(N−1)))`.

**Non-parametric tests** (distribution-free; use when normality fails):

| Test | Question | Statistic | Null distribution |
|---|---|---|---|
| **Sign test** | are paired differences consistent in sign? | `W=Σ I(yᵢ>xᵢ)` (drop ties) | `Binomial(N, 0.5)` |
| **Wilcoxon signed-rank** | do matched pairs have equal "mean rank"? (symmetric diffs about 0) | `W=Σ sgn(yᵢ−xᵢ)·Rᵢ` (rank of `|diff|`) | mean 0, `σ²_W=N(N+1)(2N+1)/6`; small N tabulated |
| **Mann–Whitney U** | is `P(X>Y)>0.5`? / are the two distributions equal? | `U=min(U₁,U₂)`, `Uₖ=Rₖ−nₖ(nₖ+1)/2` | small n tabulated; large `U ~ N(n₁n₂/2, n₁n₂(n₁+n₂+1)/12)` |
| **Friedman** | generalization of sign test to n-tuples / repeated measures | rank-based | χ² |

- **Sign vs signed-rank**: sign test needs only order (fewer assumptions) but lower power; signed-rank has higher power (ARE 0.67 vs sign), but if differences are Normal the **paired t-test** is best (ARE 0.95).
- **Mann–Whitney U** is the non-parametric counterpart of the two-sample t-test (the `instruction.pdf` lab: compare T-test vs Mann–Whitney when normality is violated, e.g. Cauchy data — the t-test breaks, U holds).

---

## 5. Dimensionality reduction

> **Official sub-topic:** *Dimensionality reduction, task definition, manifold, intrinsic dimension. PCA and kernel PCA. Non-linear dimensionality reduction methods.*

### 5.1 Task definition

- **Input** `X={xᵢ}ᵢ₌₁ᵐ ⊂ ℝ^D`. **Assumption**: the data (approximately) lie on a **manifold** `M` of dimension `d < D`.
- **Output**: a space `T` of dim `L`, a reduction map `F:X→T`, and a reconstruction map `f:T→M⊂X`, with `L` as small as possible (ideally `L=d`) and small **reconstruction error** `Σ d(xᵢ, f(F(xᵢ)))`.
- **Manifold** (*varieta*): a topological space that locally resembles Euclidean space, globally typically non-linear (e.g. Swiss roll, spiral). **Manifold learning** = find its dimension and unfold it.
- **Intrinsic dimension** (*vnitřní dimenze*): the number of variables that satisfactorily describe the data; estimated, it is vague and need not be an integer (fractals: e.g. 1.4).
- **Motivation**: visualization (2–3D), compression, latent-variable discovery, better learning in fewer dims.
- **Curse of dimensionality** (*prokletí dimenzionality*): sample size needed grows exponentially with `D`; high-D space is mostly "empty" (volume of unit hypersphere/hypercube → 0).

### 5.2 PCA (Principal Component Analysis) ⭐

Fits a (hyper)ellipsoid: new orthogonal axes along directions of **maximum variance**; **diagonalizes the covariance matrix** (removes linear redundancy/correlation).

**Derivation** (zero-centred `X`, `Σ xᵢ=0`):
1. Covariance `C_X = (1/m)XᵀX`.
2. Seek transform `P` (`XP=T`) making `C_T` diagonal: `C_T = (1/m)TᵀT = PᵀC_X P`.
3. Any real symmetric `C_X = E D Eᵀ` (eigendecomposition). Choose `P = E` (eigenvectors as columns) ⇒ orthogonal (`P⁻¹=Pᵀ`) ⇒ `C_T = PᵀC_X P = D` (diagonal). **PCA = eigenvectors of the covariance matrix**; eigenvalues = variance along each component.
4. **Reduction** `T_{m×L}=X_{m×D}P_{D×L}` (keep `L` largest-eigenvalue components); **reconstruction** `X̂=T Pᵀ`. Dropping small components = controlled information loss.
- **Big-`D` trick** (`m≪D`, e.g. images): decompose the `m×m` matrix `(1/m)XXᵀ` instead of the `D×D` `XᵀX` (Gram/scalar-product matrix; eigenvectors map via `Xᵀv`).
- Properties: `L≤D`; components ordered by explained variance; used for compression, visualization, de-correlation, and as a regression preprocessing step (PCR).

### 5.3 Kernel PCA ⭐

Linearize a non-linear manifold via an **implicit** feature map `φ:X→U` defined only through a **kernel** `K(xᵢ,xⱼ)=⟨φ(xᵢ),φ(xⱼ)⟩` (the *kernel trick*; never compute `φ` explicitly).
- Eigenvectors of `C_U` lie in the span of `φ(xᵢ)`: `v=Σαᵢφ(xᵢ)`. Substituting reduces the problem to the **kernel matrix** eigenproblem **`Kα = mλα`** (diagonalize `K`).
- Projections: `tᵢₖ = Σⱼ αⱼ K(xᵢ,xⱼ)` — only `K` appears.
- **Remarks**: works on non-linear manifolds; complexity independent of `dim U` (can even use `L>D`); but `K` is `m×m` (grows quadratically with `m` → expensive for large data; subsample); no local minima; **cannot reconstruct** objects (no explicit `f`).

### 5.4 Distance-preserving non-linear methods

- **MDS (Multidimensional Scaling)**: keep points close in `X` close in `T`. Minimize **stress** `√(Σ(f(δᵢⱼ)−dᵢⱼ)²/Σdᵢⱼ²)`, `δᵢⱼ`=input distances, `dᵢⱼ`=output distances, `f`=proximity transform (identity→metric, ranks→ordinal). Solved by gradient descent / eigen-methods.
- **Isomap** [Tenenbaum 2000]: classical MDS on **geodesic** distances (distance *along* the manifold, not Euclidean chord). Algorithm: (1) k-NN / ε-neighborhood graph, (2) edge = Euclidean dist, (3) **shortest paths** (Dijkstra/Floyd, `O(m³)`) ≈ geodesics, (4) classical MDS. Provable convergence with enough data; cubic cost; sensitive to `K` (too small → disconnected graph, too large → "short-circuit" shortcuts).
- **LLE (Locally Linear Embedding)** [Roweis–Saul 2000]: each point ≈ a weighted linear combination of its neighbors; (1) find weights `wᵢⱼ` minimizing `‖xᵢ−Σ wᵢⱼxⱼ‖²` with `Σⱼwᵢⱼ=1`; (2) find low-dim `tᵢ` preserving the **same weights** (`min Σ‖tᵢ−Σwᵢⱼtⱼ‖²`). "Ultimate piecewise-linear" method; one parameter `K`; no local minima; invariant to scaling/rotation/translation; but unstable in sparse regions, collapses points near origin, poor for new data.
- **t-SNE** [van der Maaten–Hinton 2008] ⭐ (visualization): preserve **small** pairwise distances (local structure), allow large distances to grow.
  - Original space: `p_{j|i} ∝ exp(−‖xᵢ−xⱼ‖²/2σᵢ²)` (Gaussian); `σᵢ` set via **perplexity** (effective # neighbors); symmetric `pᵢⱼ=(p_{j|i}+p_{i|j})/(2m)`.
  - Map space: **heavy-tailed Student-t** `qᵢⱼ ∝ (1+‖tᵢ−tⱼ‖²)⁻¹` (resolves the "crowding problem").
  - Minimize **KL divergence** `KL(P‖Q)=ΣΣ pᵢⱼ log(pᵢⱼ/qᵢⱼ)` by gradient descent. KL **asymmetry**: large `p` modeled by small `q` is heavily penalized ⇒ preserves local neighborhoods/clusters. Great for revealing clusters, but distances/cluster sizes in the map are not quantitatively meaningful.

### 5.5 Self-Organizing Maps (SOM / Kohonen) (*samoorganizující se mapy*)

> *(Slide images didn't extract; covered from standard knowledge — and the FAQ shows it was asked in detail by Mrázová.)*

A **vector-quantization** + dimensionality-reduction neural method. A fixed low-D **grid of neurons**, each with a weight (codebook) vector `wⱼ∈ℝ^D`. For each input `x`:
1. find the **Best Matching Unit (BMU)** = nearest neuron `c=argminⱼ‖x−wⱼ‖`;
2. update the BMU **and its grid neighbors** toward `x`: `wⱼ ← wⱼ + η(t)·h_{cj}(t)·(x−wⱼ)`, where `h_{cj}` is a neighborhood function (e.g. Gaussian) on the *grid*, both `η` and neighborhood radius shrinking over time.
- Result: a **topology-preserving** map — nearby grid neurons respond to similar inputs ⇒ 2-D visualization of high-D data.
- **Relation to k-means**: with a **zero-width neighborhood** (only the BMU updates), SOM reduces to online k-means; the neighborhood term is what adds topology preservation.

### 5.6 Summary

PCA remains the workhorse; non-linear methods help on well-sampled smooth manifolds but struggle on real noisy data (curse of dimensionality, unreliable intrinsic-dim estimation, optimization issues). Practical concerns: hyperparameters, implicit vs explicit `F`/`f`, additivity (can you drop a coordinate to go `L→L−1`? — PCA yes, t-SNE no).

---

## 6. Clustering

> **Official sub-topic:** *Clustering. Task formalization and complexity. Clustering methods: k-means, EM GMM clustering, hierarchical clustering, density-based clustering. Spectral clustering.* ⭐ (The single most-asked SAN topic in past papers — distance-based vs density-based, k-means details, DBSCAN, hierarchical, spectral.)

### 6.1 Formalization & complexity

Partition `m` objects into `k>1` **disjoint, non-empty, exhaustive** clusters `Ω={C₁,…,Cₖ}` so that objects are **similar within** and **dissimilar between** clusters. Inputs: data + **distance/dissimilarity function** (+ criterion). Unknown: number of clusters, the partition, (prototypes).
- A Bayesian decision task: minimize expected loss; **search space** = number of partitions = **Stirling number of the 2nd kind** `S(m,k)` (e.g. `S(m,2)=2^{m−1}−1`). Exhaustive search infeasible ⇒ **NP-hard**, heuristics with **local optima**.
- **Distance functions**: **Minkowski** `(Σ|xᵢ−yᵢ|^k)^{1/k}` (k=1 Manhattan/Hamming, k=2 Euclidean, k=∞ Chebyshev), **cosine dissimilarity** `1−cosθ` (documents), **edit/Levenshtein** (strings). *The chosen metric controls cluster shape — e.g. L2→spherical, L1→diamond clusters (a question Kléma/Berka ask).*

### 6.2 k-means ⭐ (distance-based, hard, non-hierarchical)

Minimize global homogeneity `W(k)=Σᵢ Σ_{xⱼ∈Cᵢ} ‖xⱼ−μᵢ‖²`. Algorithm (Lloyd):
1. init `k` centroids (e.g. random objects);
2. **assign** each object to nearest centroid (`argminⱼ‖xᵢ−μⱼ‖²`);
3. **update** each centroid = mean of its objects;
4. repeat 2–3 until centroids stop changing.
- Greedy; **guaranteed to converge**, fast; finds a **local** optimum; **sensitive to initialization** (run multiple times; k-means++); sensitive to outliers; produces **convex/spherical** clusters (under L2).
- Generalizable: other distance (centroid then = the point minimizing total distance, e.g. medoid → k-medoids).
- **Choosing `k`**: domain knowledge; rule of thumb `k≈√(m/2)`; **elbow method** on `W(k)` (W decreases monotonically with k — look for the elbow); **Hartigan** `H(k)=[W(k)/W(k+1)−1](m−k−1)`; **gap statistic** (Tibshirani) compares `log W(k)` to a reference (uniform/permuted) null; **AIC/BIC** with EM.

### 6.3 EM & Gaussian Mixture Models (soft clustering) ⭐

k-means is a special case of the **EM (Expectation–Maximization)** algorithm, which maximizes likelihood `P(X|θ)` via a latent variable `Q`:
- **E-step**: estimate the latent distribution (cluster responsibilities) given current `θ`;
- **M-step**: update `θ` to maximize the (expected) likelihood given `Q`.
- (For k-means: `Q`=hard membership, E=assign, M=recompute means.)

**EM for GMM**: `P(xᵢ|θ)=Σⱼ αⱼ·N(xᵢ; μⱼ, Σⱼ)`, parameters `θ={αⱼ, μⱼ, Σⱼ}` with `Σαⱼ=1`.
- **E-step**: responsibilities `P(Cⱼ|xᵢ)` via Bayes; **M-step**: re-estimate `αⱼ, μⱼ, Σⱼ` (weighted MLE).
- **Soft clustering**: object membership is a probability (`Σⱼ P(Cⱼ|xᵢ)=1`); assign to the max-posterior cluster. More flexible than k-means (elliptical clusters via `Σⱼ`), **more robust but slower**; `k` can be chosen via likelihood/AIC/BIC.
- EM can also fit a **naïve-Bayes** mixture for categorical data.

### 6.4 Hierarchical clustering (*hierarchické shlukování*) ⭐

Builds a **dendrogram** (binary tree) → a taxonomy at all granularities; no prior `k` (cut the dendrogram to get a partition).
- **Agglomerative (AHC, bottom-up)**: start with singletons, repeatedly **merge the two closest clusters**.
- **Divisive (top-down)**: start with one cluster, repeatedly split; needs an internal clustering method; more efficient when the full tree isn't needed.
- **Linkage** = generalized cluster distance `δ(Cᵢ,Cⱼ)`:
  - **single** `min d(x,y)` — chaining, elongated/non-compact clusters;
  - **complete** `max d(x,y)` — compact, outlier-sensitive;
  - **average** `mean d(x,y)`;
  - **centroid** `d(μᵢ,μⱼ)`.
  Different linkages → different dendrograms.
- **Complexity**: AHC single-link `O(m²n)` (vs k-means `O(i·k·m·n)`).

### 6.5 Density-based clustering — DBSCAN ⭐

A cluster = a **high-density region** separated by low-density regions. Parameters: **ε** (neighborhood radius) and **minPts**, + a distance.
- **Core point**: ≥ `minPts` objects within its ε-neighborhood. **Border point**: in a core point's neighborhood but not core. **Noise**: neither.
- Grow clusters from core points by **density-reachability**.
- **+** finds **arbitrary shapes**, **robust to noise/outliers**, **no `k` needed**. **−** struggles with clusters of **very different densities**; sensitive to ε/minPts; (no fixed cluster shape, unlike k-means).

### 6.6 Spectral clustering ⭐

Handles **non-convex / linearly non-separable** clusters (e.g. concentric circles, spirals) where k-means fails. Idea: cluster by the **spectrum (eigenvectors) of a graph Laplacian**.

**Algorithm:**
1. choose a **similarity function** (e.g. RBF `s(xᵢ,xⱼ)=exp(−‖xᵢ−xⱼ‖²/σ²)`);
2. build similarity (affinity) matrix `S`;
3. build a **similarity graph** (ε-neighborhood, or k-NN, or full with local kernel) — sparsify by removing long edges;
4. **graph Laplacian** `L = D − S` (unnormalized) or `L_rw = D⁻¹L = I − D⁻¹S` (normalized), `D`=degree matrix;
5. take the `k` eigenvectors of `L` with smallest eigenvalues as columns → `V_{m×k}`;
6. run **k-means on the rows of `V`**.

- **Why it works**: `fᵀLf = ½ Σᵢⱼ sᵢⱼ(fᵢ−fⱼ)²` measures variation of `f` along the graph; clustering = **balanced minimum cut**. Plain `min cut` gives degenerate cuts → use **RatioCut** `cut(A,B)(1/|A|+1/|B|)` or **Ncut** `cut(A,B)(1/vol A+1/vol B)` (both NP-hard); spectral clustering is the **relaxation**. Ideal case: graph with exactly `k` components ⇒ `λ₁=…=λₖ=0` and the indicator eigenvectors split the clusters; **eigengap** heuristic chooses `k` (look for the first big jump in eigenvalues).
- **+** no assumptions on cluster shape, no local optima, modular (swap kernel/graph), eigengap for `k`. **−** parameter-sensitive (σ, ε/k), expensive on large dense graphs (`O(m³)` eigendecomposition), unclear on irregular (power-law) graphs.
- **Kernel k-means** is the closely-related idea: run k-means in the implicit kernel feature space (only `‖φ(x)−μᵥ‖²` via `k(·,·)` needed) — equivalent in spirit to spectral methods.

### 6.7 Evaluation & categorization

- **Internal** (no labels): homogeneity (intra-cluster distance, = k-means W), separability, stability; for GMM = likelihood; for spectral = balanced cut size.
- **External** (vs gold standard `G`): **purity** (`(1/m)Σᵢ maxⱼ|Cᵢ∩Gⱼ|`; can't compare different `k`), **Normalized Mutual Information** `NMI=2I(Ω,G)/(H(Ω)+H(G))`, **Rand index** `RI=(TP+TN)/C(m,2)`.
- **Categorization**: **non-hierarchical** (global criterion, hard/soft: k-means, EM) vs **hierarchical** (local similarity, agglomerative/divisive: AHC). Clustering is **subjective/unsupervised** — no single ground truth.

---

## 7. Supplementary topics

### 7.1 Generalized Linear Models (GLMs) — bridges §1 and §3

A GLM (Kléma's lecture 6) generalizes linear regression with **three components**:
1. a **linear predictor** `η = Xᵀβ`;
2. a **link function** `g` with `E[Y|X]=μ=g⁻¹(η)`;
3. a response distribution from the **exponential family** `f_θ(y)=exp((yθ−b(θ))/φ + c(y,φ))`.

| Model | Link `g` | Distribution |
|---|---|---|
| Linear regression | identity | Normal |
| **Logistic** regression | logit `log(μ/(1−μ))` | Binomial/Bernoulli |
| **Poisson** regression | log | Poisson (count data; `λ=E=Var`) |

- **Canonical link** `g(μ)=θ=(b')⁻¹(μ)` makes the log-likelihood **strictly concave** ⇒ unique MLE. Learning = maximize `ℓ(β)` (IRLS).
- **Poisson regression**: `log μᵢ = Xᵀβ`, `Y~Poisson(μ)` — for counts; better than linear regression which can predict negatives, assumes homoscedasticity/symmetry that count data violate.
- **Model comparison**: nested GLMs compared by F-test / likelihood-ratio / deviance; AIC/BIC for non-nested.

### 7.2 Anomaly / outlier detection (Pevný, lecture 9)

- **Definition** (Hawkins): an outlier deviates so much from other observations as to arouse suspicion it was generated by a different mechanism. Often **unsupervised** (few/no labeled anomalies) — differs from supervised classification.
- **1-D**: fit a density (parametric or **kernel density / Parzen** estimate); flag `x` as anomaly when `p(x) ≤ β`, with `β` set so the flagged region has probability `α`. Principled when a pdf is available.
- **Higher-D**:
  - **(pseudo-)likelihood / density** methods: Gaussian, **Gaussian mixtures**, KDE → threshold on density.
  - **Distance-based** (Ramaswamy): outliers are far from their neighbors — rank by distance to the **k-th nearest neighbor**; return the top fraction `p` as outliers. **LOF** (local outlier factor) compares a point's local density to its neighbors'.
  - **Classification-based**: **one-class SVM** (learn a boundary enclosing normal data); "density detection as classification".
- Robust statistics (§4) underpins outlier-resistant estimation here.

### 7.3 Empirical studies design & power analysis (Míkovec, lecture 11)

For HCI/empirical experiments (and the `san_practice` lab):
- **Design**: identify **independent**, **dependent**, and **confounding** variables (a confounder co-varies with the IV and biases the result; control via randomization/blocking). **Within-subject** (each participant sees all conditions — more power, but order/learning effects → counterbalance) vs **between-subject** (separate groups — no carry-over, needs more participants).
- **Power analysis**: relates four quantities — **significance level α** (Type I), **power = 1−β** (1−Type II), **sample size n**, **effect size d** (Cohen's d; min. practically-relevant violation of H₀). Fix any three → solve the fourth.
  - **a-priori**: compute required `n` for target power (typically 0.8) at given α, d → plan the study.
  - **post-hoc**: compute achieved power.
- Within-subject designs reach a target power with smaller `n` (lower error variance). Tools: R `pwr`, G*Power.

---

## 8. Cross-subject links
- **PCA / kernel PCA / EM** ↔ also appear in **SSU** (EM, kernels, SVM) and **VIZ** (PCA for projection); **k-means/EM** ↔ SSU.
- **Bias–variance, cross-validation, regularization, ROC/AUC** ↔ **SSU** (empirical risk, VC dimension, SVM).
- **Kernel trick** (kernel PCA, kernel k-means, spectral) ↔ **SSU** (kernel SVM).
- **Complexity (NP-hardness of clustering), graph Laplacian** ↔ **KO/PAL/DS2** (graphs, optimization).
- **Maximum likelihood / exponential family / EM** ↔ **SSU** (MLE, EM as lower-bound maximization).

## 9. References
- **ISLR** — G. James, D. Witten, T. Hastie, R. Tibshirani, *An Introduction to Statistical Learning with Applications in R*, Springer (the primary source for §1–3, §5–6). Free PDF: <https://www.statlearning.com/>.
- **ESL** — Hastie, Tibshirani, Friedman, *The Elements of Statistical Learning*, Springer.
- Course pages: <http://cw.felk.cvut.cz/wiki/courses/b4m36san/start>.
- Robust statistics: **P. J. Rousseeuw & C. Croux (1993)**, *Alternatives to the Median Absolute Deviation*, JASA (Sₙ, Qₙ constants & breakdown — used to correct the slide). P. Huber, *Robust Statistics*.
- Dimensionality reduction: Shlens, *A Tutorial on PCA* (arXiv:1404.1100); Schölkopf, Smola, Müller, *Kernel PCA* (1997); Tenenbaum et al., *Isomap* (Science 2000); Roweis & Saul, *LLE* (Science 2000); van der Maaten & Hinton, *Visualizing Data Using t-SNE* (JMLR 2008); Carreira-Perpiñán, *A Review of Dimension Reduction Techniques*.
- Clustering: Jain, *Data Clustering: 50 Years Beyond K-Means*; von Luxburg, *A Tutorial on Spectral Clustering*; Manning et al., *Introduction to IR* (purity/NMI/Rand).
- Anomaly detection: Chandola, Banerjee, Kumar, *Anomaly Detection: A Survey* (2009); Hawkins (1980).
- Power analysis: Cohen, *Statistical Power Analysis for the Behavioral Sciences* (1988); G*Power.
