# BE4M33OSW — Worked Examples

Worked problems, mostly the **ALC tableau** (the most exam-critical mechanical skill) plus modelling and reduction exercises. Source: lectures 07–09 and `materials/s9.pdf` (DL exercise sheet). Cross-references to `summary.md` in brackets.

---

## A. ALC modelling [§1.2]

Vocabulary: concepts `Person, Man, Woman, Father, GrandFather, Sister`; roles `hasChild, hasSibling`.
TBox:
```
Man ⊑ Person
Woman ⊑ Person ⊓ ¬Man
Father ≡ Man ⊓ ∃hasChild.Person
GrandFather ≡ ∃hasChild.∃hasChild.⊤        (sloppy — allows female grandfathers!)
Sister ≡ Person ⊓ ¬Man ⊓ ∃hasSibling.Person
```
- **Fix the GrandFather axiom:** `GrandFather ≡ Man ⊓ ∃hasChild.∃hasChild.⊤`.
- **"A father having only sons":** `FatherOfBoys ≡ Father ⊓ ∀hasChild.Man`.
- **"A man with no brother but ≥1 sister who has a child":**
  `HappyUncle ≡ Man ⊓ ∃hasSibling.(Woman ⊓ ∃hasChild.⊤) ⊓ ∀hasSibling.¬Man`.
- **Global domain & range** of `hasChild` (connects Person to Person):
  `∃hasChild.⊤ ⊑ Person` (domain) and `⊤ ⊑ ∀hasChild.Person` (range).
- **Local range** ("a father of sons connects via hasChild only to a Man"):
  `FatherOfSons ⊑ ∀hasChild.Man`.

## B. Reductions to (un)satisfiability / consistency [§1.3]

- **Disjointness → unsatisfiability:** `K ⊨ C ⊑ ¬D` ⇔ `K ⊨ C⊓D ⊑ ⊥` ⇔ `C⊓D` unsatisfiable.
  Chain: `C^I ⊆ Δ^I\D^I ⇔ C^I∩D^I ⊆ (Δ^I\D^I)∩D^I = ∅`.
- **"Is C non-empty in *all* models of K?"** Yes iff `K ∪ {C ⊑ ⊥}` is **inconsistent** (for consistent K).
- **Why inconsistency is fatal:** OWL-DL rests on FOL, so an **inconsistent ontology entails everything** (`K ⊨ α` for all α) — reasoning becomes useless.

---

## C. Tableau run 1 — consistency check, empty TBox [§1.4]

`K₂ = (∅, A₂)`, `A₂ = { (∃hasChild.Man ⊓ ∃hasChild.GrandParent ⊓ ¬∃hasChild.(Man⊓GrandParent))(JAN) }`.

**Step 0 — NNF.** Push `¬` inward (`¬∃R.C ≡ ∀R.¬C`):
```
C = ∃hasChild.Man ⊓ ∃hasChild.GrandParent ⊓ ∀hasChild.(¬Man ⊔ ¬GrandParent)
```
**Step 1 — init `G₀`:** one node `JAN`, `L(JAN) = {C}`.

**Steps 2–4 — deterministic rules:**
1. **→⊓** on `JAN`: add the three conjuncts to `L(JAN)`.
2. **→∃** on `∃hasChild.Man`: new node `n₀`, edge `hasChild`, `L(n₀)={Man}`.
3. **→∃** on `∃hasChild.GrandParent`: new node `n₁`, edge `hasChild`, `L(n₁)={GrandParent}`.
4. **→∀** on `∀hasChild.(¬Man⊔¬GrandParent)`: propagate `(¬Man⊔¬GrandParent)` to **both** `n₀` and `n₁`.

Now only the **→⊔** (branching) rule is applicable. Apply it to, say, `n₁` which has `{GrandParent, ¬Man⊔¬GrandParent}`:
- **Branch G₅:** add `¬GrandParent` → `{GrandParent, ¬GrandParent}` = **direct clash** ✗.
- **Branch G₆:** add `¬Man` → `{GrandParent, ¬Man}` — OK.

Apply **→⊔** to `n₀` (`{Man, ¬Man⊔¬GrandParent}`) in branch G₆:
- **G₇:** add `¬GrandParent` → `{Man, ¬GrandParent}` — OK, **complete & clash-free**. ✔
- **G₈:** add `¬Man` → `{Man, ¬Man}` = clash ✗.

**Result: CONSISTENT.** A canonical model from `G₇`:
```
Δ = {Jan, i₀, i₁},  hasChild = {(Jan,i₀),(Jan,i₁)},
Man = {i₀},  GrandParent = {i₁}.
```
Reading: Jan has a child who is a Man (`i₀`, not a grandparent) and a different child who is a GrandParent (`i₁`, not a man) — so Jan has *no* child who is both. (Note: this relies on **not** assuming UNA collapses `i₀`,`i₁`.)

---

## D. Tableau run 2 — general TBox + **blocking** [§1.5]

`K = (T ∪ {Parent ⊑ ∃hasChild.Parent}, A)` with
`T = {∃hasChild.⊤ ≡ Parent}`, `A = {hasChild(JOHN,MARY), Woman(MARY)}`.
(Shorten `Parent=P`, `hasChild=h`, `Woman=W`.)

**Internalize the TBox** into `⊤ ⊑ ⊤_C`:
```
⊤_C = (¬(∃h.⊤) ⊔ P) ⊓ (¬P ⊔ ∃h.⊤) ⊓ (¬P ⊔ ∃h.P)
```
**NNF** (`¬∃h.⊤ ≡ ∀h.⊥`):
```
⊤_C = (∀h.⊥ ⊔ P) ⊓ (¬P ⊔ ∃h.⊤) ⊓ (¬P ⊔ ∃h.P)
```
**Init `G₀`:** nodes `JOHN, MARY`; edge `h(JOHN,MARY)`; `L(MARY)={W}`.

**Apply `→⊑`** to every node (adds `⊤_C`), then `→⊓`, then the `→⊔` branches. Pick the clash-free disjuncts:
- `JOHN` has an `h`-successor (`MARY`), so the disjunct `∀h.⊥` would clash (it would force `⊥` into `MARY`); choose `P` from the first conjunct ⇒ `P ∈ L(JOHN)`, then `∃h.⊤` and `∃h.P` from the others.
- `∃h.P` on `JOHN`: is it already satisfied? `MARY` is an `h`-successor but `P ∉ L(MARY)` yet — so **→∃** creates a new successor `a₀` with `L(a₀) ⊇ {P}` (or we may push P into MARY; either way P-successors get generated).
- Each new `P`-node again triggers `→⊑` ⇒ `∃h.P` ⇒ another `P`-node … **potential infinite chain**.

**Blocking stops it.** A fresh node `a₁` becomes **blocked** by an ancestor (e.g. `JOHN`/`a₀`) once `L(a₁) ⊆ L(blocker)`. The **→∃ rule is not applied to blocked nodes**, so expansion halts. The resulting graph is **complete and clash-free** ⇒ **K is CONSISTENT**. The corresponding model is *cyclic/looping* (the blocked node reuses the blocker's `h`-successor): `Δ={…, x}`, `h` loops back so every Parent has a Parent-child.

> **Take-away for the exam:** (1) internalize GCIs into `⊤ ⊑ ⊤_C`; (2) NNF; (3) run rules; (4) **subset-block** a node `x` by an ancestor `y` when `L(x) ⊆ L(y)` and then forbid `→∃` on `x`. Without blocking the cyclic axiom `Parent ⊑ ∃hasChild.Parent` would not terminate.

---

## E. Tableau run 3 — concept satisfiability, empty TBox [§1.4]

Is `∃hasChild.(Student ⊓ Employee) ⊓ ¬(∃hasChild.Student ⊓ ∃hasChild.Employee)` satisfiable?

**NNF** (`¬(X⊓Y)=¬X⊔¬Y`, `¬∃R.C=∀R.¬C`):
```
∃h.(Student⊓Employee) ⊓ ( ∀h.¬Student ⊔ ∀h.¬Employee )
```
Init: node `a₀` with this concept. `→⊓`, then **→∃** creates successor `b` with `L(b)={Student,Employee}`. Now **→⊔** on the second conjunct of `a₀`:
- **Branch 1:** `∀h.¬Student` on `a₀` ⇒ push `¬Student` to `b` ⇒ `{Student,Employee,¬Student}` = **clash**.
- **Branch 2:** `∀h.¬Employee` on `a₀` ⇒ push `¬Employee` to `b` ⇒ `{Student,Employee,¬Employee}` = **clash**.

Both branches clash ⇒ the concept is **UNSATISFIABLE**. (Intuition: if you have a child who is *both* a student and an employee, then you trivially have "a child who is a student" *and* "a child who is an employee" — so the negation is contradictory.)

---

## F. EL++ classification (SNOMED-style) [§5.2]

TBox `{ A⊑∃r.A, ∃r.B⊑B1, ⊤⊑B, A⊑B2, B1⊓B2⊑C }`. Apply completion rules CR1–CR5:
```
A ⊑ ⊤            (CR2)
A ⊑ B            (from ⊤⊑B)
A ⊑ B1           (CR5: A⊑∃r.A, A⊑B ⇒ via ∃r.B⊑B1)
A ⊑ C            (CR4 convexity: A⊑B1, A⊑B2, B1⊓B2⊑C)
```
⇒ `A ⊑ C` is entailed. Note: **polynomial**, purely by subsumption — **no satisfiability/branching** needed (why EL++/OWL 2 EL scales to SNOMED's 350k concepts).

---

## G. SPARQL semantics gotcha — `MINUS` vs `FILTER NOT EXISTS` [§3.5]

Data: `{ :s :p :o . }`. Query bodies:
```
# (1) ?s ?p ?o  MINUS { ?x ?y ?z }            → returns ALL rows (no shared vars ⇒ nothing removed)
# (2) ?s ?p ?o  FILTER NOT EXISTS { ?x ?y ?z } → returns NOTHING (inner pattern always matches)
```
Same data, opposite results — the difference is whether shared variables exist between outer and inner patterns.

## H. SPARQL entailment-regime gotcha [§3.5]
`SELECT ?x WHERE { ?x :madeFromFruit _:d }` with `:BancroftChardonnay a :Chardonnay`, `:Chardonnay ⊑ ∃:madeFromGrape.⊤`, and grape ⊑ fruit knowledge:
- **Simple / RDF:** 0 results · **RDFS:** 1 result · **OWL:** 2 results.
Richer entailment regime ⇒ more (certain) answers.
