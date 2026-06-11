# BE4M33SSU — Statistical Machine Learning (SSU) — Study Summary

**Language:** English-primary (slides are English), with key Czech terms in parentheses for the oral (Czech) exam.
**Source materials:** V. Franc's SSU lecture slides (ERM, PAC/VC, SVM, parameter estimation, EM/Bayesian, generative learning, ANNs, deep ANNs/CNNs, SGD, HMMs, ensembling, predictor evaluation) in `materials/`. Cross-checked against standard references (Shalev-Shwartz & Ben-David *Understanding Machine Learning*; Bishop *PRML*; Goodfellow et al. *Deep Learning*) — verified inline.

> **⭐ Exam strategy.** SSU is heavily **theoretical/mathematical** (Franc). The three official topics are tightly defined; be ready to **derive**, not just state: the generalization bound (Hoeffding → union bound → finite-H bound), the SVM primal↔dual, the EM lower bound (Jensen → ELBO → block-coordinate ascent), and backprop via the chain rule. Note SSU is also examined in the Data Science commission (Železný/Kléma chair) — **EM ↔ SAN's GMM clustering** and **SVM/kernels ↔ SAN's kernel PCA** are natural cross-links.

**Structure.** Sections 1–3 mirror the three official sub-topics. Section 4 is supplementary (PAC framing, generative/Bayesian, HMMs, ensembling/bias-variance, predictor evaluation). Worked problems in `examples.md`.

---

## 1. Empirical Risk Minimization (ERM)

> **Official sub-topic:** *Risk and empirical risk of a predictor, generalization bounds and Hoeffding inequality, statistically consistent learning algorithms, VC dimension, SVMs and Kernel SVMs.*

### 1.1 Setup: risk, empirical risk, ERM

- **Predictor / strategy / hypothesis** `h : X → Y`. **Loss** `ℓ : Y×Y → ℝ₊` (e.g. 0/1 loss `ℓ⁰ᐟ¹(y,y')=⟦y≠y'⟧`).
- **(Expected/generalization) risk** `R(h) = E₍ₓ,ᵧ₎∼p[ℓ(y,h(x))]` — the quantity we *want* to minimize.
- **Bayes-optimal predictor** `h* ∈ argmin_{h∈Yˣ} R(h)`; **Bayes risk** `R* = R(h*)` = irreducible error.
- **Hypothesis class** `H ⊆ Yˣ` is fixed *before* learning (prior knowledge). **Learning algorithm** `A : ∪ₘ(X×Y)ᵐ → H` returns `hₘ = A(Tᵐ)` from a training set `Tᵐ ∼ pᵐ` (i.i.d.).
- **Empirical risk** (training error): `R_Tᵐ(h) = (1/m) Σᵢ ℓ(yⁱ, h(xⁱ))`.
- **ERM**: `hₘ ∈ argmin_{h∈H} R_Tᵐ(h)`. Depending on `H`, `ℓ`, and the optimizer, ERM instantiates SVMs, linear/logistic regression, neural nets (backprop), AdaBoost, gradient-boosted trees, …

### 1.2 ERM can fail — overfitting

The empirical risk is **not in general a reliable proxy** for the expected risk. Example: a "lookup table" classifier achieves `R_Tᵐ=0` always (memorizes), yet `R(hₘ)` stays high. **Overfitting** = low training error `R_Tᵐ(hₘ)` but high expected risk `R(hₘ)`. **Fix: constrain `H`.**

### 1.3 From LLN to the Uniform Law of Large Numbers (ULLN)

- For a **single, fixed** `h`, the **Law of Large Numbers** gives `R_Tᵐ(h) → R(h)`. But ERM *chooses* `hₘ` using the data, so we need the bound to hold **simultaneously for all `h∈H`**.
- **ULLN** holds for `H` if there is a **sample complexity** `m_H(ε,δ)` s.t. for all `ε>0`, `δ∈(0,1)`, every distribution `p`, and every `m ≥ m_H(ε,δ)`:
  ```
  P( max_{h∈H} |R(h) − R_Tᵐ(h)| ≥ ε ) ≤ δ.
  ```

### 1.4 ULLN for a finite hypothesis class — the key derivation ⭐

For `H = {h₁,…,h_K}`, with `B(h) = {Tᵐ : |R_Tᵐ(h)−R(h)| ≥ ε}`:
1. **`max ≥ ε` ⇔ some term `≥ ε`** ⇒ `P(max_h |…| ≥ ε) = P(∪_h B(h))`.
2. **Union bound**: `P(∪_h B(h)) ≤ Σ_h P(B(h))`.
3. **Hoeffding's inequality** (per `h`): `P(|R_Tᵐ(h) − R(h)| ≥ ε) ≤ 2·exp(−2mε²/(ℓ_max−ℓ_min)²)`. *(Verified standard form.)*

⇒ `P(max_{h∈H} |R(h)−R_Tᵐ(h)| ≥ ε) ≤ 2|H|·exp(−2mε²/(ℓ_max−ℓ_min)²)`.

Setting RHS `= δ` gives the **sample complexity** `m_H(ε,δ) = (log 2|H| − log δ)/(2ε²) · (ℓ_max−ℓ_min)²`.

### 1.5 Generalization bound for finite `H`

With probability ≥ `1−δ`, **simultaneously for all `h∈H`**:
```
R(h) ≤ R_Tᵐ(h) + (ℓ_max−ℓ_min)·√[ (log 2|H| + log(1/δ)) / (2m) ]
       └ empirical risk ┘   └──────── complexity term ────────┘
```
- Holds for **any** learning algorithm, not just ERM.
- To shrink the complexity term: **increase `m`** or **decrease `|H|`** (use prior knowledge).
- **Recommendations**: (1) minimize empirical risk, (2) use more data, (3) limit `|H|`.
- **Structural Risk Minimization (SRM)**: design a nested family `H₁⊂H₂⊂…⊂H_K`, run ERM in each, and pick the level minimizing the **bound** (empirical risk + complexity) — balances under/overfitting.

### 1.6 Consistency & error decomposition (PAC framing)

Decompose the risk of the learned `hₘ`:
```
R(hₘ) = [R(hₘ) − R(h_H)]  +  [R(h_H) − R*]  +  R*
          estimation error      approximation error    irreducible
```
- `h_H ∈ argmin_{h∈H} R(h)` (best in class). **Approximation error** depends only on `H` (prior). **Estimation error** depends on `H`, data, and `A`. `R*` is irreducible.
- A **statistically consistent** learner drives the **estimation error → 0** as `m → ∞`.
- **PAC (Probably Approximately Correct)**: `A` is a successful PAC learner for `H` if `∃ m_H^{pac}(ε,δ)` s.t. for `m ≥ m_H^{pac}`, `P(R(hₘ) − R(h_H) ≤ ε) ≥ 1−δ`.
- **Theorem**: if ULLN holds for `H`, then **ERM is a successful PAC learner** with `m_H^{pac}(ε,δ) = m_H^{ull}(ε/2, δ)` (because estimation error `≤ 2·sup_{h∈H}|R(h)−R_Tᵐ(h)|`).

### 1.7 VC dimension (infinite hypothesis classes) ⭐

For finite `H` the bound used `|H|`; for infinite `H ⊆ {−1,+1}ˣ` we need a capacity measure.
- **Shattering**: a set `{x¹,…,xᵐ}` is **shattered** by `H` if for *every* labeling `y∈{−1,+1}ᵐ` there is an `h∈H` realizing it (`h(xⁱ)=yⁱ ∀i`). (i.e. `H` can produce all `2ᵐ` dichotomies.)
- **VC dimension** `VC(H)` = size of the **largest** set that `H` can shatter.
- **Theorem**: the VC dimension of all **two-class linear classifiers** `h(x)=sign(⟨w,φ(x)⟩+b)` in an `n`-dim feature space is **`n+1`**. *(Verified.)*
- **ULLN via VC** (`d = VC(H) < ∞`): `P(sup_{h∈H}|R(h)−R_Tᵐ(h)| ≥ ε) ≤ 4·(2em/d)ᵈ·e^{−mε²/8}` → ⇒ ULLN holds with `m_H^{ull}(ε,δ) ≤ C·(d − log δ)/ε²`. **Finite VC dimension ⇒ ERM is PAC-successful**, even for infinite `H`.

### 1.8 Support Vector Machines (SVM) ⭐

**Goal**: linear classifier `h(x)=sign(f(x;w,b))`, `f(x)=⟨w,φ(x)⟩+b`, minimizing 0/1 risk. But ERM with 0/1-loss is **NP-hard** (no poly algorithm in `m,n`). Two fixes:

**(a) Replace 0/1 loss by the convex hinge loss** `ψ(y,f(x)) = max{0, 1 − y·f(x)}`, which **upper-bounds** the 0/1 loss (`⟦y·f(x)≤0⟧ ≤ max{0,1−y·f(x)}`).
**(b) Control `‖w‖`** (`L2` regularization) — limits the hypothesis-space "radius" → controls capacity (PAC sample complexity `O(r²/(ε²δ²))` for the norm-bounded class).

**Primal SVM** (convex quadratic program):
```
min_{w,b,ξ}  ½‖w‖² + (C/m) Σᵢ ξᵢ
s.t.  yⁱ(⟨w,φ(xⁱ)⟩ + b) ≥ 1 − ξᵢ ,   ξᵢ ≥ 0 ,  ∀i
```
- `ξᵢ` = slack (margin violation); **`C`** trades off margin width vs. training error (tuned on validation). Large `C` → fits training data harder (larger `‖w‖`); small `C` → wider margin, more regularization.
- The **geometric margin** is `1/‖w‖`; SVM maximizes the margin subject to (soft) correct classification.

**Dual SVM** (via Lagrangian + strong duality):
```
α* = argmax_α  Σᵢ αᵢ − ½ ΣᵢΣⱼ αᵢαⱼ yⁱyʲ ⟨φ(xⁱ),φ(xʲ)⟩
s.t.  Σᵢ αᵢ yⁱ = 0 ,   0 ≤ αᵢ ≤ C/m ,  ∀i
```
- Recover `w* = Σᵢ yⁱ αᵢ φ(xⁱ) = Σ_{i∈ISV} yⁱ αᵢ φ(xⁱ)`; `b*` from any margin SV (`0<αᵢ*<C/m`).
- **`α*` is sparse**: `w*` is a linear combination of the **support vectors** (`αᵢ*>0`) — the points on/inside the margin.

### 1.9 Kernel SVM ⭐

The dual (and the classifier) use the data **only through dot products** `⟨φ(xⁱ),φ(xʲ)⟩`. Define a **kernel** `k(x,x') = ⟨φ(x),φ(x')⟩`. Then:
```
α* = argmax_α  Σᵢ αᵢ − ½ ΣᵢΣⱼ αᵢαⱼ yⁱyʲ k(xⁱ,xʲ) ,   classifier: h(x)=sign( Σᵢ yⁱαᵢ k(xⁱ,x) + b )
```
- **Kernel trick**: compute `k` cheaply *without* ever forming `φ(x)` → learn in a high-/infinite-dim feature space tractably.
- Examples: **polynomial** `k(x,x')=⟨x,x'⟩²` (degree-2; general `(⟨x,x'⟩+c)ᵈ`), **RBF/Gaussian** `k(x,x')=exp(−γ‖x−x'‖²)` (corresponds to an **infinite-dimensional** feature space).
- Valid kernels = symmetric positive semi-definite (**Mercer** condition).
- **Memory**: linear formulation `O(n)`; kernel formulation `O(m²)` to train, `O(d·|ISV|)` to evaluate — kernel pays off when the implicit feature space is huge.

---

## 2. Maximum likelihood estimation & the EM algorithm

> **Official sub-topic:** *Maximum likelihood estimator, consistency of an estimator, EM algorithm as maximization of a lower bound of the likelihood, E-step and M-step as block-coordinate descent for the lower bound.*

### 2.1 Generative learning & the MLE

- **Generative learning**: restrict to a parametric family `{p_θ(x,y) | θ∈Θ}` (prior knowledge), estimate `θ` from data, then predict via the **plug-in Bayes-optimal** rule on `p_{θₘ}`.
- **Estimator** = a map `θₘ = e(Tᵐ)` from data to parameters. **Desired properties**:
  - **Unbiased**: `E_{Tᵐ∼θ*}[e(Tᵐ)] = θ*`.
  - **Small variance**: `V_{Tᵐ∼θ*}[e(Tᵐ)] → 0` as `m→∞`.
  - **Consistent**: `P_{Tᵐ∼θ*}(|e(Tᵐ) − θ*| > ε) → 0` as `m→∞`.
- **Log-likelihood** (normalized by `m`): `L_Tᵐ(θ) = (1/m) Σ_{x∈Tᵐ} log p_θ(x) = E_{x∼Tᵐ}[log p_θ(x)]`.
- **Maximum Likelihood Estimator (MLE)**: `θₘ = e_{ML}(Tᵐ) ∈ argmax_{θ∈Θ} L_Tᵐ(θ)`.
  - For exponential families, MLE matches model expectations of the sufficient statistic `φ(x)` to the empirical mean (**moment matching**); usually no closed form → gradient ascent.

### 2.2 Properties of the MLE ⭐

- **Bias**: the MLE *can be biased* for finite `m` (e.g. the ML variance estimator `(1/m)Σ(x−x̄)²` is biased), but is **asymptotically unbiased**.
- **Consistency**: the MLE is **consistent under (mild) regularity conditions** — chiefly that the log-likelihood **converges uniformly** to the expected log-likelihood (a ULLN-type condition) and identifiability.
- **Asymptotic optimality**: the MLE has the **smallest possible asymptotic variance** — it attains the **Cramér–Rao lower bound** (asymptotically efficient), with `θₘ ≈ N(θ*, I(θ*)⁻¹/m)` (`I` = Fisher information).

### 2.3 Latent variables → the EM algorithm

**Problem**: estimate `θ` of `p_θ(x,y)` from data `Tᵐ = {xʲ}` where the states `y` (or latent `z`) are **never observed**. The marginal log-likelihood
```
L(θ) = E_{x∼Tᵐ}[ log Σ_{y∈Y} p_θ(x,y) ]
```
has a **log-of-sum** → hard to maximize directly when `θ` collects heterogeneous parameters (e.g. GMM weights + means + covariances). Examples: **mixture of Gaussians**, generating MNIST digits with latent style `z`.

**EM idea**: turn the hard unsupervised MLE into a sequence of **easy supervised MLE** problems by introducing soft labels.

### 2.4 EM as lower-bound maximization (the derivation) ⭐

Introduce auxiliary distributions `α_x(y) > 0`, `Σ_y α_x(y)=1` for each `x`. By **Jensen's inequality** (log is concave):
```
L(θ) = E_Tᵐ[ log Σ_y α_x(y)·(p_θ(x,y)/α_x(y)) ]
     ≥ E_Tᵐ[ Σ_y α_x(y)·log(p_θ(x,y)/α_x(y)) ] =: LB(θ,α)     (the ELBO / lower bound)
```
Equivalent form revealing the gap:
```
LB(θ,α) = E_Tᵐ[ log p_θ(x) ] − E_Tᵐ[ D_KL( α_x(y) ‖ p_θ(y|x) ) ]
        =     L(θ)          −  (non-negative KL gap)
```
⇒ the bound is **tight** (`LB = L`) **iff `α_x(y) = p_θ(y|x)`**.

### 2.5 E-step and M-step as block-coordinate ascent ⭐

EM does **block-coordinate ascent** on `LB(θ,α)`, alternately maximizing over `α` then `θ`:
- **E-step** (maximize over `α`, fix `θ⁽ᵗ⁾`): set `α_x⁽ᵗ⁾(y) = p_{θ⁽ᵗ⁾}(y|x)` — the posterior over latent states (soft labels). This makes the bound **touch** the true log-likelihood at `θ⁽ᵗ⁾` (`KL=0`).
- **M-step** (maximize over `θ`, fix `α⁽ᵗ⁾`): solve the **supervised, weighted MLE**
  ```
  θ⁽ᵗ⁺¹⁾ = argmax_θ E_Tᵐ[ Σ_y α_x⁽ᵗ⁾(y) log p_θ(x,y) ]
  ```
  (= MLE as if the soft labels `α` were the data annotation).
- **Guarantees**: `L(θ⁽ᵗ⁾)` is **monotonically non-decreasing** (because the M-step raises a bound that touched `L`), `α⁽ᵗ⁾` converges. **No guarantee of a global maximum** → sensitive to **initialization** (run multiple restarts).

### 2.6 Worked instance: GMM via EM

Model `p_θ(x) = Σ_y p(y)·N(x; μ_y, σ_y)`. Iterate:
- **E-step** (responsibilities): `α_x⁽ᵗ⁾(y) = p(y)N(x;μ_y,σ_y) / Σ_{ȳ} p(ȳ)N(x;μ_ȳ,σ_ȳ)`.
- **M-step** (closed form, weighted estimates):
  ```
  p⁽ᵗ⁺¹⁾(y) = E_Tᵐ[α_x(y)] / Σ_ȳ E_Tᵐ[α_x(ȳ)]
  μ_y⁽ᵗ⁺¹⁾   = E_Tᵐ[α_x(y)·x] / E_Tᵐ[α_x(y)]
  σ_y²⁽ᵗ⁺¹⁾ = E_Tᵐ[α_x(y)·(x−μ_y⁽ᵗ⁺¹⁾)²] / E_Tᵐ[α_x(y)]
  ```
  → **same as SAN's EM-GMM soft clustering** (k-means is the hard-assignment special case).

### 2.7 Bayesian inference & MAP (regularized MLE)

- For **small `m`**, a single point estimate `θₘ` can be far from `θ*`. **Bayesian** approach: treat `θ` as random with **prior `p(θ)`**; combine with the likelihood to get the **posterior** `p(θ|Tᵐ) ∝ p(Tᵐ|θ)·p(θ)`; predict with a **weighted mixture** over all `θ` (rather than one).
- **MAP estimate** = single posterior mode: `θₘ = argmax_θ [ Σ_{(x,y)∈Tᵐ} log p(x,y|θ) + log p(θ) ]` = **MLE + a regularizer** `log p(θ)`. E.g. a Gaussian weight prior `w∼N(0,σI)` ⇒ **L2 weight decay** in DNN training.

---

## 3. Deep networks & their supervised training

> **Official sub-topic:** *Neurons, network architectures, convolutional networks, backpropagation and layer types, parameter initialisation, stochastic gradient descent.*

### 3.1 Neuron & activation functions

- **McCulloch–Pitts neuron**: output `f(s)` with `s = ⟨w,x⟩ + b` (weights `w`, bias `b`); `f` = **activation function**. The bias is often folded in by appending a constant-1 input.
- **Activations**:
  - **Linear** (identity) → a linear neuron ≡ **linear regression** (MLE under Gaussian noise ⇒ MSE loss).
  - **Logistic sigmoid** `σ(s)=1/(1+e⁻ˢ)` ∈ (0,1) → can represent a **Bernoulli** parameter; an MCP neuron with sigmoid ≡ **logistic regression** (MLE ⇒ **cross-entropy** loss). `tanh(s)=2σ(2s)−1`.
  - **ReLU** `max(0,s)` — non-saturating for `s>0`, cheap, mitigates vanishing gradients (the default in deep nets).
  - **Softmax** `σ_k(s)=e^{s_k}/Σ_j e^{s_j}` → a **categorical** distribution over `K` classes (inputs `s` = "logits"); linear layer + softmax ≡ **multinomial logistic regression** (MLE ⇒ multinomial cross-entropy).

### 3.2 Layers, architectures & loss

- **Linear / dense / fully-connected (affine) layer**: many linear neurons, `z = Wx + b`.
- **Multilayer Perceptron (MLP)**: stacked linear layers + nonlinearities; a **feed-forward** network. **Universal approximation**: an MLP with one sufficiently wide hidden layer can approximate any continuous function — but **depth** is exponentially more parameter-efficient for many functions (motivation for deep nets).
- **Loss by output type** (all from MLE):
  | Problem | Output layer | Loss |
  |---|---|---|
  | Binary classification | sigmoid neuron | (binary) cross-entropy |
  | Multinomial classification | softmax | multinomial cross-entropy |
  | Multi-output regression | linear layer | mean squared error |

### 3.3 Backpropagation ⭐

A method to compute the **gradient `∇L(w)`** of the loss w.r.t. all parameters, used by gradient-based optimizers. It is the **chain rule applied modularly** (divide-and-conquer): represent the network (and even the loss) as a chain of modules `z¹=x → z² → … → zᴸ=L`.
- **Forward pass**: compute and cache each module's output `zˡ`.
- **Backward pass**: propagate the **sensitivity** `δˡ = ∂L/∂zˡ` from output to input:
  ```
  δᵢˡ = ∂L/∂zᵢˡ = Σ_j δⱼˡ⁺¹ · ∂zⱼˡ⁺¹/∂zᵢˡ
  ```
  i.e. each module only needs to know **how to push a gradient message through itself** (`∂out/∂in` and `∂out/∂params`). Local parameter gradients `∂L/∂wˡ` are then read off. Cost ≈ one forward + one backward pass (same order as evaluation).

### 3.4 Convolutional Neural Networks (CNNs) ⭐

Motivated by the visual cortex (nearby cells process nearby regions). Replace dense layers with **convolutional layers** to exploit image structure:
- **Local receptive field**: each neuron connects only to a small `F×F` patch (e.g. 3×3) across the input depth — not the whole image.
- **Weight sharing**: the **same filter** (weights + bias) is slid over all spatial positions → drastically fewer parameters (e.g. 3×3×depth+1 per filter vs. millions for dense) and **translation equivariance**.
- **Multiple filters** `D` → `D` output **feature maps** (the "depth" of the output volume).
- **Hyperparameters**: filter size `F`, **stride `S`** (step; larger `S` → smaller output), **zero-padding `P`** (preserve spatial size). Output size `= (W − F + 2P)/S + 1`.
- A **nonlinearity** (ReLU) follows the convolution; **pooling** (e.g. max-pooling) downsamples for translation invariance and to grow the receptive field. Deep CNNs learn a **feature hierarchy** (edges → parts → objects).

### 3.5 Parameter initialization

- Can't init all weights to 0 (symmetry — all neurons learn the same thing). Random init breaks symmetry.
- Scale matters: too large → saturation/exploding activations; too small → vanishing signal. **Variance-preserving** schemes keep activation/gradient variance roughly constant across layers: **Xavier/Glorot** (`Var(w)=1/n_in` or `2/(n_in+n_out)`, for tanh/sigmoid) and **He/Kaiming** (`Var(w)=2/n_in`, for ReLU).
- A Gaussian weight prior (MAP, §2.7) ⇒ **L2 weight decay** as a regularizer.

### 3.6 (Stochastic) Gradient Descent ⭐

- **Gradient Descent (GD)**: `θ_{k+1} = θ_k − α_k ∇L(θ_k)`; `α_k` = learning rate / step size. Full-batch `∇L` is an exact (zero-variance) gradient but costs `O(m)` per step.
- **Stochastic Gradient Descent (SGD)**: estimate `∇L` from a **single sample or mini-batch** `B_k` → cheap, noisy step. `g(θ_k)` is an **unbiased** estimate of `∇L(θ_k)`. Shuffle data each **epoch** before forming batches.
- **Convergence** (informal theorem): with `L` having a **Lipschitz-continuous gradient** (constant `L`) and bounded gradient-noise variance (constant `M`):
  - **Fixed step size** → converges to a **neighborhood** of the optimum of size `∝ αLM` (noise floor).
  - **Diminishing step size** (`Σα_k=∞, Σα_k²<∞`) → converges to the optimum (for strongly convex `L`).
  - Full-batch (`M=0`) → exact convergence; mini-batches trade variance for speed/parallelism.
- **Improvements**:
  - **Momentum**: `v_{k+1}=μv_k − α∇L`, `θ_{k+1}=θ_k+v_{k+1}` (`μ∈[0,1]`); damps oscillations in high-curvature directions, builds velocity along consistent gradients.
  - **AdaGrad / RMSProp / Adam**: **per-parameter adaptive** learning rates (scale by accumulated/decayed squared gradients); Adam = momentum + RMSProp, the common default.

---

## 4. Supplementary topics

### 4.1 Predictor evaluation (test error)

- Estimate `R(h)` of a *given* `h` on an independent **test set** `Sˡ` (i.i.d., separate from training): **test error** `R_Sˡ(h)=(1/l)Σ ℓ(yⁱ,h(xⁱ))`.
- `R_Sˡ(h)` is a random number, **unbiased** (`E[R_Sˡ(h)]=R(h)`), with deviation bounded by Hoeffding: `P(|R_Sˡ(h)−R(h)|≥ε) ≤ 2e^{−2lε²/(ℓ_max−ℓ_min)²}` ⇒ confidence intervals for the risk. (Training error, by contrast, is **optimistically biased**.)

### 4.2 Generative classifiers (Bayes plug-in)

- Model class-conditionals `p(x|y)` + priors `p(y)`; classify by the **plug-in Bayes rule** `h(x)=argmax_y p(y)p(x|y)`.
- Gaussian class-conditionals with shared covariance → **LDA**; per-class covariance → **QDA**; conditional independence → **naïve Bayes** (cross-link to SAN §3). Parameters estimated by MLE (§2).
- **Generative vs discriminative**: generative models `p(x,y)` (can sample, handle missing data, need distributional assumptions); discriminative models `p(y|x)`/decision boundary directly (often more accurate when assumptions are wrong) — SVM/logistic regression are discriminative.

### 4.3 Hidden Markov Models (HMMs) — structured sequences

- A **(first-order) Markov model** on a sequence `s=(s₁,…,s_n)`: `p(s)=p(s₁)∏ₜ p(sₜ|sₜ₋₁)` (the future depends on the past only through the present). **Homogeneous** = time-independent transition matrix `P`; state distribution evolves as `π_t = π_1 Pᵗ⁻¹`. Theory: irreducible + aperiodic ⇒ unique **stationary distribution**.
- **HMM** = a generative model with a hidden state sequence `y` (Markov) emitting observed `x`: parameters = initial `p(y₁)`, **transition** `p(yₜ|yₜ₋₁)`, **emission** `p(xₜ|yₜ)`.
- **Inference algorithms** (dynamic programming over the trellis):
  - **Most probable sequence** `s* = argmax p(s|x)` → **Viterbi** (max-product DP).
  - **Marginals** `p(sₜ|x)` → **forward–backward** (sum-product DP).
  - **Parameter learning** from unannotated sequences → EM using **transition/emission counts** (**Baum–Welch**, an instance of §2's EM).

### 4.4 Ensemble methods & the bias–variance decomposition

- **Bias–variance decomposition** (regression, squared loss): `E[(h_m(x)−y)²] = bias² + variance + noise`, where **bias²** = gap of the *averaged* predictor `ḡ_m` from optimal (≈ approximation error), **variance** = spread of `h_m` around `ḡ_m` (≈ estimation error).
- **Bagging (Bootstrap AGGregating)**: train many **high-variance, low-bias** predictors on bootstrap resamples; average / majority-vote → **reduces variance**. **Random Forests** = bagged decision trees + random feature subsets per split (further decorrelates trees).
- **Boosting**: **sequentially** train **low-variance, high-bias** weak learners, each focusing on the previous ones' errors → **reduces bias**. **AdaBoost** reweights misclassified examples; **Gradient Boosting** fits each new learner to the negative gradient of the loss (gradient-boosted trees).
- **Stacking / mixture of experts**: combine heterogeneous models with a learned meta-combiner.

---

## 5. Cross-subject links
- **EM / GMM, k-means** ↔ **SAN** (EM-GMM soft clustering; k-means = hard EM).
- **Kernel trick, SVM** ↔ **SAN** (kernel PCA, kernel k-means, spectral clustering).
- **LDA/QDA/naïve Bayes, ROC/AUC, cross-validation, regularization** ↔ **SAN** (discriminant analysis, model selection).
- **PAC / VC dimension / online learning** ↔ **SMU** (PAC and online learnability, VC dimension — same theory, complementary emphasis).
- **MLE, exponential families, consistency** ↔ **SAN** (statistical fundamentals).
- **Complexity / NP-hardness of 0/1-loss ERM** ↔ **TAL/KO**.

## 6. References (verified)
- **Understanding Machine Learning** — S. Shalev-Shwartz & S. Ben-David, Cambridge (ERM, PAC, VC, ULLN, SVM). Free PDF online.
- **Pattern Recognition and Machine Learning** — C. Bishop, Springer (MLE, EM, mixture models, kernels, neural nets).
- **Deep Learning** — I. Goodfellow, Y. Bengio, A. Courville, MIT Press (backprop, CNNs, initialization, SGD/Adam). Free online.
- Schlesinger & Hlaváč, *Ten Lectures on Statistical and Structural Pattern Recognition*, Kluwer 2002 (EM, HMMs — the course's own lineage).
- T. Minka, *Expectation-Maximization as lower bound maximization* (1998) — the §2.4 derivation.
- Hoeffding (1963); Vapnik & Chervonenkis (VC dimension). Verified: linear-classifier VC dim `= n+1`; Hoeffding bound `2e^{−2mε²/(b−a)²}`.
- Bottou (2009) on SGD convergence; Duchi/Hazan/Singer (2011, AdaGrad); Kingma & Ba (Adam).
- Course pages: <https://cw.fel.cvut.cz/wiki/courses/be4m33ssu/start>.
