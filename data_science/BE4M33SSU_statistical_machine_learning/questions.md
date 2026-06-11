# BE4M33SSU — Statistical Machine Learning (SSU) — Questions

The OI-Wiki FAQ archive (`../../frequently_asked_questions/`) has **no SSU past-paper questions** (newer DS subject, not represented by the wiki contributors). The list below is **anticipated questions derived from the official topic structure** and the seminar assignments, cross-referenced to `summary.md`. SSU exams (Franc) reward **derivations** — ⭐ marks the high-yield ones.

## Topic 1 — Empirical Risk Minimization
- Define risk, empirical risk, Bayes risk; what is ERM? Why/when does it overfit? → §1.1–1.2, examples.md A
- ⭐ **Derive the generalization bound for a finite `H`** (max⇒union bound⇒Hoeffding); state the sample complexity. → §1.4–1.5, examples.md E
- State the **Hoeffding inequality**; why is the *uniform* LLN (not plain LLN) needed for learning? → §1.3–1.4
- Error decomposition (estimation/approximation/irreducible); define a **consistent** learner and **PAC** learning; show ULLN ⇒ ERM is PAC. → §1.6
- ⭐ **VC dimension**: define shattering & VC dim; prove `VC` of linear classifiers `= n+1`; `VC(H)≤log₂|H|`. → §1.7, examples.md C–D
- Structural Risk Minimization. → §1.5
- ⭐ **SVM**: why replace 0/1 by hinge loss + norm constraint? Write the **primal** QP; derive/relate the **dual**; what are support vectors; role of `C`. → §1.8, examples.md F
- ⭐ **Kernel SVM**: kernel trick, polynomial & RBF kernels, Mercer condition, memory cost. → §1.9

## Topic 2 — MLE and the EM algorithm
- Define MLE; properties (bias, **consistency**, asymptotic efficiency / Cramér–Rao). → §2.1–2.2
- ⭐ **Derive EM as lower-bound maximization**: Jensen ⇒ ELBO; show `LB=L − E[KL(α‖p(y|x))]`; tightness condition. → §2.4, examples.md H
- ⭐ **E-step and M-step as block-coordinate ascent**; why does the likelihood increase monotonically? → §2.5
- Apply EM to a **GMM** (write E-step responsibilities + M-step updates); relation to k-means. → §2.6, examples.md G
- MAP vs MLE; Bayesian inference; Gaussian prior ⇒ weight decay. → §2.7

## Topic 3 — Deep networks & their training
- Neuron + activations (sigmoid/tanh/ReLU/softmax); linear neuron ≡ linear regression, sigmoid ≡ logistic regression, softmax ≡ multinomial logistic regression. Match output/loss. → §3.1–3.2
- Universal approximation vs the benefit of depth. → §3.2
- ⭐ **Backpropagation**: derive the backward sensitivity recursion `δˡ`; why is it the chain rule done modularly? cost? → §3.3, examples.md I
- ⭐ **CNNs**: local receptive fields, weight sharing, filters/feature maps, stride/padding, output-size formula; why fewer parameters + translation equivariance. → §3.4
- **Parameter initialization**: why not zeros; Xavier/Glorot vs He. → §3.5
- ⭐ **SGD**: GD vs SGD, unbiased mini-batch gradient, convergence (fixed vs diminishing step), momentum, Adam/AdaGrad/RMSProp. → §3.6

## Supplementary (if asked)
- Predictor evaluation: test error, unbiasedness, Hoeffding CI; train error is optimistic. → §4.1
- Generative vs discriminative; LDA/QDA/naïve Bayes as Bayes plug-in. → §4.2
- HMMs: Markov/HMM definition, Viterbi (most probable sequence), forward–backward (marginals), Baum–Welch (EM). → §4.3
- Bias–variance decomposition; bagging/random forests (cut variance) vs boosting (cut bias). → §4.4, examples.md J–K

## Likely cross-exam links (DS commission)
- "Relate SSU's EM to clustering" → SAN EM-GMM. "Relate kernels" → SAN kernel PCA / spectral. "PAC/VC" → SMU learnability.
