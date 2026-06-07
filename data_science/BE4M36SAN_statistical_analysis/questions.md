# BE4M36SAN — Statistical Analysis (SAN) — Questions

Past-exam questions for SAN, from `../../frequently_asked_questions/oi_wiki_committee_archive.md` (5 entries), cross-referenced to `summary.md`. **Jiří Kléma is head of commission and examined several of these himself** — note what he drilled into.

## Real past-exam questions (OI-Wiki archive)

### Clustering — asked 3× (the most frequent SAN topic) → §6
- **Q1 (20.6.2023, Berka)** — "Explain distance-based vs density-based clustering with example algorithms." Follow-ups: *what shape do k-means clusters have?* (L2→circular, L1→diamond/square; DBSCAN→arbitrary). *How is `k` chosen in k-means?* Name another distance-based algorithm (→ hierarchical). *How does a taxonomy arise from hierarchical clustering?* (dendrogram). → §6.1–6.5
- **Q2 (8.2.2023, Berka)** — same distance/density theme; drilled **k-means parameters**, **how to choose optimal `k`**, **other metrics than sum-of-squares** (incl. Hamming generalized to numeric vectors = sum of componentwise differences). → §6.2
- **Q3 (20.6.2018, Mrázová)** — broad: k-means, **spectral clustering**, hierarchical, **t-SNE**, **SOM**, PCA, and their drawbacks. k-means: centroid recomputation, initialization problem, distance functions, choosing `k`, convergence-but-not-optimal, outlier problem. Spectral & t-SNE: *why needed*, advantages over plain k-means; **t-SNE → Kullback–Leibler divergence**. Hierarchical: dendrogram, agglomerative vs divisive. **SOM** (heavily): neuron weights, how they store position, topology use, neuron interaction, advantage over other techniques. → §6.2, §6.6, §5.4–5.5

### Robust regression / M-estimators — asked 1× → §4 (examiner: **Kléma**)
- **Q4 (8.2.2023, Kléma)** — "Methods of robust regression, compare with linear regression; with examples show when robust regression is better and when not. M-estimators and their use in robust regression." Kléma wanted: **which criterion is optimized** in OLS (sum of squares, assumes Gaussian + homoscedastic errors) and **how it changes** for robust (other norm: L1/absolute values, or median instead of mean), the **effect of outliers**, and for **M-estimators**: median ↔ which distribution it is the ML estimate of (**Laplace**) and the link to residuals. → §4.4–4.5, examples.md E

### LDA / QDA / logistic regression — asked 1× → §3 (examiner: **Kléma**)
- **Q5 (14.6.2022, Kléma)** — "Linear and quadratic discriminant analysis (LDA, QDA). Define, describe their **parameters**, explain their **decision** and **learning**. Where do these methods sit among other **supervised** methods?" → §3.2–3.5, examples.md F, H

### Dimensionality reduction — covered within Q3 → §5
- PCA, kernel PCA, t-SNE (KL divergence), SOM. → §5.2–5.5

---

## Anticipated questions (from official topic structure)

### §1 Multiple linear regression
- State the model and its **assumptions**; what breaks if each is violated? Why standardize before ridge?
- Derive OLS `β̂`. Explain R² vs adjusted R² vs F-test — *which decides model usefulness / size?*
- Interpret a coefficient with a categorical predictor / an interaction term (hierarchy principle).
- Detect & treat **outliers / influential points** (Cook's distance) and **collinearity** (VIF).
- **Ridge vs lasso** — penalties, sparsity, geometry, bias–variance, choosing λ by CV. ⭐

### §2 Non-linear regression
- Polynomial vs step vs splines vs local regression vs GAM — when to use each.
- Define a cubic / natural cubic / smoothing spline; degrees of freedom; knot placement.

### §3 Discriminant analysis ⭐
- Logistic regression: logit, ML learning, odds ratio, decision boundary, multiclass softmax.
- LDA vs QDA vs naïve Bayes: assumptions, parameters, boundary shape (linear/quadratic), bias–variance.
- Evaluation under class imbalance: threshold, ROC/AUC, sensitivity/specificity.

### §4 Robust statistics ⭐
- Breakdown point, influence function, ARE — for mean/median/trimmed/MAD/Sₙ/Qₙ.
- M-estimators: ρ/ψ for mean (Normal), median (Laplace), Huber, Hampel; convexity.
- Robust regression criteria (L1, Huber, LMS, LTS); when OLS is still best.
- Non-parametric tests: sign, Wilcoxon signed-rank, Mann–Whitney U, Spearman/Kendall.

### §5 Dimensionality reduction
- Task definition, manifold, intrinsic dimension, curse of dimensionality.
- **PCA derivation** (eigenvectors of covariance) + big-D Gram trick; kernel PCA (kernel-matrix eigenproblem, no reconstruction).
- MDS / Isomap (geodesic) / LLE / **t-SNE** (KL, perplexity, Student-t) / SOM (BMU, topology, → k-means).

### §6 Clustering ⭐
- Formalization, NP-hardness (Stirling number), distance functions & cluster shapes.
- k-means (criterion, algorithm, convergence, init sensitivity, choosing k); EM-GMM soft clustering (E/M steps).
- Hierarchical (agglomerative/divisive, linkages, dendrogram); DBSCAN (core/border/noise, ε/minPts, pros/cons).
- **Spectral clustering** (similarity graph, Laplacian L=D−S, eigenvectors, balanced cut, eigengap). ⭐
- Evaluation: internal vs external (purity, NMI, Rand).
