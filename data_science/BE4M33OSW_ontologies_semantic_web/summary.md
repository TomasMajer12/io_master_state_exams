# BE4M33OSW — Ontologies and Semantic Web (OSW) — Study Summary

**Language:** English-primary, with key Czech terms in parentheses for the oral (Czech) exam.
**Source materials:** lectures 01–11 + supplementary papers in `materials/` (P. Křemen et al., ČVUT FEL, Winter 2025), cross-checked against W3C specs and the *Description Logic Handbook*. See `../../00_subjects_overview.md` for the official topic list.

**Structure.** Sections 1–4 mirror the four official exam sub-topics exactly. Section 5 ("Supplementary") covers the rest of the course (KOS/SKOS, SNOMED-CT, RML, GeoSPARQL, RDF validation, ontology engineering, upper ontologies, persistence/access control, LLMs) per the comprehensive depth requested.

> **Reading order tip.** The exam emphasis is heaviest on **§1 (Description Logics + tableau)** and **§3 (RDF/SPARQL semantics)**. Worked tableau runs are in `examples.md` (see also `materials/s9.pdf`).

---

## 0. Big picture — why ontologies, why a formal language

- **Ontology** (*ontologie*) = a *formal, explicit specification of a shared conceptualization* (Gruber). It stabilizes meaning so that both **people and machines** agree on it. Use-cases: **data integration, Semantic Web, (Linked) Open Data**.
- **Conceptualization** (*konceptualizace*) = the set of objects/relations an observer believes exist in the domain. There is no single "best" conceptualization — only ones that fit a given purpose. An ontology is the explicit specification of one.
- Ontologies **≠ taxonomies**: a taxonomy uses a single relationship (usually *is-a*); an ontology adds many relation types, axioms and rules.
- **Why a formal language?** Informal (UML-like) diagrams cannot express cardinalities, inverses, disjointness, etc., nor support *automated reasoning* / *validation*. We need a logic.
- **Logic trade-off** (*kompromis*): a logical calculus is always a trade-off between **expressiveness** and **tractability of reasoning**. Candidate logics:
  - propositional logic — decidable, **NP-complete** (Cook), too weak;
  - **first-order predicate logic (FOPL)** — expressive but **undecidable** (Gödel);
  - modal logic, …
- **Open World Assumption (OWA)** (*předpoklad otevřeného světa*): whatever is *not* known is *not necessarily false* ("I don't know"). FOPL and DLs adopt OWA ⇒ **monotonic** (adding knowledge never invalidates a conclusion). Contrast **Closed World Assumption (CWA)** of relational DBs / Prolog (unprovable = false). Prolog is *not* an FOPL implementation (CWA, negation-as-failure, weak on disjunction).

---

## 1. Description Logics: ALC, SHOIN(D), SROIQ(D), complexity, tableau, blocking

> **Official sub-topic:** *Characteristics of Description Logics. Basics of ALC, SHOIN(D) and SROIQ(D), computational complexity of key reasoning. Tableau algorithm for ALC. Blocking conditions.*

### 1.1 What Description Logics are

**Description Logics (DLs)** (*deskripční logiky*) are (almost exclusively) **decidable fragments of FOPL** aimed at modeling **terminological, incomplete** knowledge. History: KL-ONE / Classic / KAON (1980s, formal semantics for semantic networks & frames) → **ALC** (1990s) → **SHOIN(D)** = OWL DL (2004) → **SROIQ(D)** = OWL 2 DL (2009).

**Building blocks:**

- **(atomic) concepts** (*koncepty*) — unary predicates / classes, e.g. `Person`, `Man`.
- **(atomic) roles** (*role*) — binary predicates / relations, e.g. `hasChild`.
- **individuals** (*individua*) — ground terms, e.g. `JOHN`.

A **knowledge base / theory** `K = (T, A)` (an **ontology** in OWL):
- **TBox `T`** (*terminologie*) — generally valid axioms, e.g. `Man ⊑ Person`.
- **ABox `A`** (*asertorická část*) — a concrete relational structure (data), e.g. `Man(JOHN)`, `loves(JOHN, MARY)`.

DLs differ in their **expressive power** (which concept/role constructors and axiom types they allow).

### 1.2 ALC — *Attributive Language with Complements*

**Interpretation** `I = (Δ^I, ·^I)`: a non-empty domain `Δ^I` and an interpretation function mapping `A^I ⊆ Δ^I`, `R^I ⊆ Δ^I × Δ^I`, `a^I ∈ Δ^I`.

**Concept constructors (syntax → semantics):**

| Concept | Semantics `·^I` | Name |
|---|---|---|
| `⊤` | `Δ^I` | universal (top) |
| `⊥` | `∅` | unsatisfiable (bottom) |
| `¬C` | `Δ^I \ C^I` | negation |
| `C ⊓ D` | `C^I ∩ D^I` | intersection |
| `C ⊔ D` | `C^I ∪ D^I` | union |
| `∀R.C` | `{a | ∀b: (a,b)∈R^I ⇒ b∈C^I}` | universal restriction |
| `∃R.C` | `{a | ∃b: (a,b)∈R^I ∧ b∈C^I}` | existential restriction |

**Axioms (`I ⊨ axiom` iff):**

| | Axiom | Condition |
|---|---|---|
| TBox | `C₁ ⊑ C₂` (inclusion) | `C₁^I ⊆ C₂^I` |
| TBox | `C₁ ≡ C₂` (equivalence) | `C₁^I = C₂^I` |
| ABox | `C(a)` (concept assertion) | `a^I ∈ C^I` |
| ABox | `R(a₁,a₂)` (role assertion) | `(a₁^I,a₂^I) ∈ R^I` |

**Worked modelling examples (ALC):**
- "Persons whose children (if any) are all men": `Person ⊓ ∀hasChild.Man`.
- `GrandParent ≡ Person ⊓ ∃hasChild.∃hasChild.⊤`.
- FOPL equivalent: `∀x(GrandParent(x) ≡ Person(x) ∧ ∃y(hasChild(x,y) ∧ ∃z hasChild(y,z)))`.
- `Mother ≡ Woman ⊓ ∃hasChild.Person`; `MotherWithoutDaughter ≡ Mother ⊓ ∀hasChild.¬Woman`.

> **Note on assumptions.** Standard ALC/OWL use the **Open World Assumption** and **do NOT** adopt the **Unique Name Assumption (UNA)** — two individuals may denote the same domain element unless stated otherwise (`owl:differentFrom` / `sameAs`). (Some lecture slides assume UNA for the ABox to simplify examples — flag this distinction on the exam.)

**Structural properties of ALC models** (these make reasoning tractable):
- **Tree Model Property (TMP)** (*vlastnost stromového modelu*): every consistent `K = (∅, {C(I)})` has a model shaped as a rooted tree. The *generalized* TMP holds for most DLs and is the key reason their complexity stays manageable.
- **Finite Model Property (FMP)** (*vlastnost konečného modelu*): every consistent `K = (T, A)` has a finite model. (FMP is *lost* once inverse roles + number restrictions interact, e.g. in SHOIN/SROIQ.)

### 1.3 Reasoning (inference) problems and their reductions

**TBox tasks** (for concepts `Cᵢ` w.r.t. `T`):
- **(un)satisfiability**: is `T ⊨ C ⊑ ⊥`? (C is unsatisfiable)
- **subsumption**: `T ⊨ C₂ ⊑ C₁`?
- **equivalence**: `T ⊨ C₁ ≡ C₂`?
- **disjointness**: `T ⊨ C₁ ⊓ C₂ ⊑ ⊥`?

**ABox tasks** (w.r.t. `K`):
- **consistency** (is `K` consistent — has a model?),
- **instance checking** `T∪A ⊨ C(a)`,
- **role checking** `T∪A ⊨ R(a₁,a₂)`,
- **instance retrieval** (all `a` with `K ⊨ C(a)`),
- **realization** (most specific concept of `a`).

**Everything reduces to (un)satisfiability / consistency checking.**

- *Subsumption → unsatisfiability:* `T ⊨ C₁ ⊑ C₂` **iff** `C₁ ⊓ ¬C₂` is unsatisfiable w.r.t. `T`.
  Derivation: `C₁^I ⊆ C₂^I ⇔ C₁^I ∩ (Δ^I\C₂^I) = ∅ ⇔ I ⊨ C₁ ⊓ ¬C₂ ⊑ ⊥`.
- *Disjointness → unsatisfiability:* `K ⊨ C ⊑ ¬D` iff `C ⊓ D` is unsatisfiable.
- *Concept unsatisfiability → theory consistency:* with a **fresh individual** `a_f`, `T ⊨ C ⊑ ⊥` **iff** `(T, {C(a_f)})` is **inconsistent**. (For DLs more expressive than ALC the whole ABox must be considered, because of TBox–ABox interaction.)

### 1.4 Tableau algorithm for ALC

State of the art for expressive DLs: **sound, complete, finite** decision procedure. It is a *production system* operating on **completion graphs**.

- **Completion graph** `G = (V, E, L)`: nodes labelled with **sets of concepts** `L(x)`, edges labelled with **sets of roles** `L(x,y)`. (Not to be confused with "complete graphs" of graph theory.)
- **Direct clash**: `{A, ¬A} ⊆ L(x)` or `⊥ ∈ L(x)`.
- **Complete graph**: no rule is applicable.
- **Idea**: `A` is consistent w.r.t. `T` iff a model of `T∪A` can be built.

**Algorithm (empty TBox `T = ∅`):**
0. **Preprocessing** — put every concept into **Negation Normal Form (NNF)** (push `¬` to atomic concepts via de Morgan; `¬∀R.C ≡ ∃R.¬C`, `¬∃R.C ≡ ∀R.¬C`).
1. **Init** `S₀ = {G₀}` from `A`: for each `C(a)` add node `a` with `C∈L(a)`; for each `R(a₁,a₂)` add edge with `R∈L(a₁,a₂)`.
2. If **every** graph in `S` has a direct clash → return **INCONSISTENT**.
3. Pick a clash-free `G`; if it is **complete** → return **CONSISTENT**.
4. Apply an applicable rule, get new state `S'`, go to 2.

**Expansion rules:**

| Rule | If | Then |
|---|---|---|
| **→⊓** | `(C₁⊓C₂) ∈ L(a)`, `{C₁,C₂} ⊄ L(a)` | add `C₁,C₂` to `L(a)` (deterministic) |
| **→⊔** | `(C₁⊔C₂) ∈ L(a)`, `{C₁,C₂}∩L(a)=∅` | **branch**: replace `G` by `G₁` (add `C₁`) and `G₂` (add `C₂`) — *non-deterministic* |
| **→∃** | `(∃R.C) ∈ L(a₁)`, no `a₂` with `R∈L(a₁,a₂) ∧ C∈L(a₂)` | create fresh node `a₂`, edge `R`, `C∈L(a₂)` |
| **→∀** | `(∀R.C) ∈ L(a₁)`, ∃ `a₂` with `R∈L(a₁,a₂)`, `C∉L(a₂)` | add `C` to `L(a₂)` (deterministic) |

Only **→⊔** increases the number of graphs (branching = disjunction).

**Properties:**
- **Soundness** — each rule preserves satisfiability (e.g. `→∃` "materializes" the domain element that `∃` already guarantees).
- **Completeness** — from a complete clash-free `G` build a **canonical model**: `Δ^I` = nodes of `G`, `A^I = {a | A∈L(a)}`, `R^I = {(a₁,a₂) | R∈L(a₁,a₂)}`.
- **Finiteness** — `K` is finite; `→⊔` applies finitely often; node count ≤ (#individuals in A) + (#existential quantifiers).

### 1.5 General TBoxes (GCIs) and **blocking**

A TBox `{Cᵢ ⊑ Dᵢ}` is **internalized** into one axiom `⊤ ⊑ ⊤_C` where `⊤_C = ⨅ᵢ(¬Cᵢ ⊔ Dᵢ)`. Every domain element must satisfy `⊤_C`, enforced by:

| Rule | If | Then |
|---|---|---|
| **→⊑** | `⊤_C ∉ L(a)` | add `⊤_C` to `L(a)` |

**Problem:** with cyclic axioms like `Man ⊑ ∃hasParent.Man`, the `→⊑`,`→⊔`,`→∃` cycle can **loop forever** (the tableau need not terminate).

**Solution — blocking** (*blokování*): stop expanding when the situation repeats "sufficiently". For **ALC, subset blocking** suffices:

> A node `x` (not from the ABox) is **blocked** by node `y` if there is a directed path `y → … → x` and `L(x) ⊆ L(y)`.

The **→∃** rule is **only applied to non-blocked nodes**. Intuition: a blocked node reuses the witness already created at its blocker, yielding a finite (possibly looping) model.

**Result:** the ALC tableau **with subset blocking** is a **sound, complete and finite** decision procedure. (More expressive DLs need stronger blocking: *equality / pairwise / dynamic blocking* — e.g. inverse roles require **double blocking**.)
*(Try it: `http://kbss.felk.cvut.cz/tools/dl`.)*

### 1.6 Extending ALC — the DL "naming" letters

| Letter | Construct | Example |
|---|---|---|
| `S` | shorthand for `ALC` + **transitive roles** (`trans(R)`) | `isPartOf` transitive |
| `H` | **role hierarchy** `R ⊑ S` | `hasMother ⊑ hasParent` |
| `O` | **nominals** (enumerated individuals) `{a₁,…,aₙ}` | `Continent ≡ {EUROPE, ASIA, …}` |
| `I` | **inverse roles** `R⁻` | `hasChild⁻ = hasParent` |
| `N` | **(unqualified) number restrictions** `(≥n R)`,`(≤n R)`,`(=n R)` | `Car ⊑ (≥4 hasWheel)` |
| `Q` | **qualified number restrictions** `(≥n R.C)` etc. | `(≥3 hasChild.Man)` = ≥3 sons |
| `R` | **complex role axioms** — role chains `R∘S ⊑ P`, disjointness `Dis(R,S)`, `∃R.Self`, reflexivity, etc. | `hasParent∘hasBrother ⊑ hasUncle` |
| `(D)` | **datatypes** (data values/properties) | `Person ⊓ ∃hasAge.23` |

**Syntactic sugar** (definable from the above): functional `⊤⊑(≤1 R)`, inverse-functional `⊤⊑(≤1 R⁻)`, reflexive `⊤⊑∃R.Self`, irreflexive `∃R.Self⊑⊥`, symmetric `R⊑R⁻`, asymmetric `Dis(R,R⁻)`, transitive `R∘R⊑R`.

**The two OWL logics:**
- **SHOIN(D)** = `ALC + S + H + O + I + N + (D)` → backs **OWL DL** (OWL 1).
- **SROIQ(D)** = `ALC + S + R + O + I + Q + (D)` → backs **OWL 2 DL**.

### 1.7 Computational complexity of key reasoning ⭐

| Logic | Concept satisfiability / consistency | Note |
|---|---|---|
| Propositional logic | NP-complete | Cook |
| FOPL | **undecidable** | Gödel |
| **ALC**, no TBox / acyclic TBox | **PSPACE-complete** | what the tableau slide means by "between NP and ExpTime" |
| **ALC** with **general TBox (GCIs)** | **ExpTime-complete** | GCIs raise complexity |
| **SHOIN(D)** = OWL DL | **NExpTime-complete** | |
| **SROIQ(D)** = OWL 2 DL | **N2ExpTime-complete** (2-NExpTime) | data complexity NP-hard |
| **EL++** (OWL 2 EL) | **PTime-complete** | subsumption, no satisfiability needed |
| **DL-Lite_R** (OWL 2 QL) | **NLogSpace**; query answering in **AC0** data complexity | query rewriting → SQL |

> **Exam phrasing:** ALC concept satisfiability *without* a general TBox is **PSPACE-complete**; with general TBoxes it jumps to **ExpTime-complete**. The tableau's space bound (PSPACE) holds for the empty/acyclic-TBox case. *(Verified against the* Description Logic Handbook *and Baader/Lutz.)*

---

## 2. Semantic Web stack, RDF, OWL, SPARQL, SWRL, DL-safe rules

> **Official sub-topic:** *Semantic Web stack, RDF, OWL, SPARQL, SWRL, tractability of rules in description logics (DL-safe rules).*
> (RDF and SPARQL details are in **§3**; here: the stack, OWL, and rules.)

### 2.1 The Semantic Web and its stack

**Semantic Web** (T. Berners-Lee, Hendler, Lassila, 2001): "an extension of the current Web in which information is given **well-defined meaning**, better enabling computers and people to work in cooperation." Goal: richer-than-keyword search via (a) more expressive languages (RDF(S), OWL), (b) inference engines, (c) expressive query languages (SPARQL).

**Resources are identified by URIs/IRIs:**
- **URI** = `<scheme>:<hier-part>[?query][#fragment]` (usually HTTP).
- **URN** — `urn:` scheme (e.g. SWRL atom identification).
- **URL** — a URI resolvable via a protocol (HTTP).
- **IRI** — internationalized URI (non-ASCII); the standard identifier for RDF/OWL.

**The (layered) Semantic Web "cake"** (bottom → top):

```
                 [ User interface & applications ]
   [ Trust ]
   [ Proof ]
   [ Unifying Logic ]
   [ Ontology: OWL ] [ Rules: RIF/SWRL ]   [ Querying: SPARQL ]
   [ Taxonomies: RDFS ]
   [ Data model: RDF ]
   [ Syntax: XML / Turtle / JSON-LD ]
   [ Identifiers: URI/IRI ]   [ Charset: Unicode ]
        (Crypto runs vertically across layers)
```

Key idea: each layer builds on the ones below; **reasoning** is provided by RDFS/OWL/rule layers, **querying** by SPARQL, **trust** by proof + crypto.

### 2.2 OWL — the Web Ontology Language

OWL adds **logically-backed** modelling on top of RDF. Everything (classes, properties, individuals) is an **IRI**.
- **classes** = DL concepts; **individuals** = DL individuals; **object properties** = DL roles; **data properties** = data roles. `owl:` = `http://www.w3.org/2002/07/owl#`.
- **OWL on top of its DL** adds: **syntactic sugar** axioms (`NegativeObjectPropertyAssertion`, `AllDisjoint`, …), **extralogical** constructs (imports, annotations), **datatypes** (XSD).

**Syntaxes (same meaning, different form):**
- *DL syntax:* `FatherOfSons ⊑ ∃hasChild.⊤ ⊓ ∀hasChild.Man`
- *Manchester:* `Class: FatherOfSons SubClassOf: hasChild some owl:Thing and hasChild only Man`
- *Turtle/RDF serialization* (verbose, with `owl:Restriction`, `owl:someValuesFrom`, …).

**Ontology header:** ontology IRI (logical id) + optional version IRI; `Import:` (pulls in other ontologies — strong impact on reasoning); `Annotations:`.

**Modelling constructs** (class frames): `SubClassOf`, `EquivalentTo`, `DisjointWith`, `DisjointUnionOf`, **`HasKey`** (a set of properties uniquely identifying instances). Object-property frames: `Functional`, `InverseFunctional`, `Transitive`, `Reflexive`, `Irreflexive`, `Symmetric`, `Asymmetric`, `Domain`, `Range`, `SubPropertyOf`, `InverseOf`, `SubPropertyChain`. Data properties: only `Functional`. Class expressions: boolean (`and/or/not`), restrictions (`some`, `only`, `value`, `Self`, `min/max/exactly n`), nominals `{a b}`, datatype facets (`xsd:integer[>=5,<10]`).

**No UNA, OWA:** OWL does not assume unique names (use `SameAs`/`DifferentFrom`) and reasons under the open world.

**Punning** (*metamodeling*): the same IRI may be reused as e.g. a class *and* an individual, **but** OWL 2 DL typing constraints must hold:
- every IRI declared as exactly one of {object, data, annotation} property;
- every IRI declared as exactly one of {class, datatype}.

**Global (decidability-preserving) constraints in OWL 2 DL:** only **simple object properties** (no transitive / property-chain sub-properties) may be used in cardinality restrictions, `Self`, `Functional`/`InverseFunctional`/`Irreflexive`/`Asymmetric`, and property disjointness; property chains and datatype definitions must be acyclic.

**Two OWL 2 semantics** (and their relationship):
- **OWL 2 Direct Semantics** (`⊨_{OWL2-DL}`) — defined via **SROIQ(D)**; interprets only logically-backed axioms (ignores annotations/declarations); **decidable**. Requires the RDF graph to satisfy OWL 2 DL restrictions.
- **OWL 2 RDF-Based Semantics** (`⊨_{OWL2-RDF}`, "OWL 2 Full") — interprets **any** RDF graph (extension of D-entailment); **undecidable**, only incomplete entailment rules given.
- **Correspondence Theorem**: the RDF-based semantics can express anything the Direct semantics can — for DL-compliant graphs the two agree.

### 2.3 OWL 2 profiles (tractable sub-languages) ⭐

| Profile | Backing DL | Designed for | Best reasoning | Complexity |
|---|---|---|---|---|
| **OWL 2 EL** | EL++ | rich class **taxonomies** (e.g. **SNOMED-CT**) | subsumption | **PTime-complete** |
| **OWL 2 QL** | DL-Lite_R | **large data** (over DBs) | conjunctive query answering via **query rewriting to SQL** | **NLogSpace**; **AC0** data |
| **OWL 2 RL** | rule-based fragment | **scalable rule reasoning** over RDF | rule materialization | **PTime** |

- **EL**: allows `∃R.C`, `∃R.{I}`, `∃R.Self`, `C⊓D`; **no** inverse / `Dis(R,Q)` / reflexive / functional / symmetric.
- **QL**: subclasses `A`, `∃R.⊤`; superclasses `C⊓D`, `¬C`, `∃R.C`; no sub-properties, no functional/transitive, no individual equality.
- **RL**: subclasses `{I}`, `C⊓D`, `C⊔D`, `∃R.C`; superclasses `C⊓D`, `¬C`, `∃R.C`, `∀R.C`, `(≤1 R.C)`; rule-based (no non-deterministic reasoning, no generation of new individuals).
- **OWL 2 DL** (full DL) = SROIQ(D), **N2ExpTime-complete**; **OWL Full** = undecidable.

### 2.4 Rules: SWRL and **DL-safe rules** (tractability) ⭐

**SWRL** (*Semantic Web Rule Language*) = **OWL DL + a Horn-clause (RuleML/Datalog) rule layer**. A rule is `antecedent → consequent` (conjunctions of atoms `C(x)`, `R(x,y)`, `sameAs`, `differentFrom`, built-ins). Example:
```
hasParent(?x,?y) ∧ hasBrother(?y,?z) → hasUncle(?x,?z)
```
SWRL adds expressivity that OWL lacks (e.g. "compose two properties into a third", relations among multiple individuals beyond tree-shaped axioms).

**The problem:** full SWRL has the power to make reasoning **undecidable** — inference is **not guaranteed to terminate**. (Rules can quantify over *all* individuals, including anonymous ones generated by `∃`.)

**The fix — DL-safe rules** (*DL-bezpečná pravidla*): restrict every rule so that **all variables bind only to explicitly named individuals** in the ABox (each variable must occur in a non-DL/"safety" atom in the body). This:
- restores **decidability**, and
- keeps the **complexity of the underlying OWL DL fragment** (e.g. NExpTime for OWL DL) — i.e. rules become *tractable relative to the base logic* because they only ever fire on the finite set of known individuals, not on the (potentially infinite) tree-model witnesses.

> Trade-off: DL-safe rules cannot draw conclusions about *unnamed* individuals (those introduced by existentials). Related practical tooling: **SQWRL** (query language over SWRL), **SPIN/SHACL rules**, **RIF** (Rule Interchange Format).

---

## 3. RDF graph model, blank-node semantics, SPARQL execution & evaluation semantics

> **Official sub-topic:** *Basics of RDF Graph model, blank node semantics. SPARQL query execution. Possible evaluation semantics of SPARQL and their differences.*

### 3.1 RDF graph model

**RDF** (*Resource Description Framework*, W3C Rec 2004; RDF 1.1 in 2014). An **RDF graph** is a **set of triples** `(subject, predicate, object)` — equivalently a directed labelled graph where nodes are IRIs/literals/blank-nodes and edges are IRIs.

**RDF Term** = IRI | blank node | literal.
- **IRI** — denotes a document or a real thing; uses `#` (hash) or `/` (slash) delimiters. *Two IRIs equal iff their strings equal*; no IRI equals a blank node or literal.
- **Literal** = lexical form (Unicode string) + **datatype IRI** (+ language tag *iff* datatype is `rdf:langString`). E.g. `"dolphin"@en`, `"128"^^xsd:integer`. Two literals equal iff lexical form, datatype, and language tag all match.
- **Datatype** = (lexical space, value space, **L2V** lexical-to-value map). Reused from XSD + `rdf:HTML`, `rdf:XMLLiteral`.
- Positions in standard RDF: subject = IRI/bnode; predicate = IRI; object = IRI/bnode/literal. (*Generalized* RDF graphs allow any term in any position — used when computing entailment closures.)

**RDF 1.1 vs 1.0:** IRIs (not URIs), all literals typed, new datatypes (`rdf:langString`, `xsd:dateTimeStamp`, …), new serializations (JSON-LD, TriG, N-Quads). **RDF 1.2** (draft): **quoted triples** `<<:john :spouse :ann>> :startDate "2020-02-11"` for efficient reification.

### 3.2 Blank-node semantics ⭐

> **Blank nodes (b-nodes)** denote **existential variables**: "there exists *some* resource" without naming it. ("There is a thing such that …")

- **Local scope**: a bnode label is local to its document; it **cannot be referenced** from outside.
- **Ground RDF graph** = no blank nodes.
- **Instance** of graph `G` = `G` with some bnodes replaced by arbitrary RDF terms.
- **Lean graph** = a graph with no instance that is a *proper subgraph* of it (i.e. no redundant bnode triples).
- **Skolemization** = replace bnodes by fresh "Skolem" IRIs (`http://…/.well-known/genid/xxx`). Preserves meaning while giving stronger identification.
- **Typical uses**: statement **reification**, complex/structured values, **n-ary relations** (e.g. a birth event), containers/collections.

**Semantic role (formal, simple interpretation `I = (IR, IP, IEXT, IS, IL)`):** a graph with bnodes is true iff there exists a mapping `A: bnodes → IR` making `A(G)` true. So bnodes are existentially quantified over the domain.

**RDF containers vs collections:**
- **Containers** (`rdf:Bag` multiset, `rdf:Seq` sequence, `rdf:Alt` alternatives) — **open** (others can add members); members via `rdf:_1, rdf:_2, …`.
- **Collections** (`rdf:List` with `rdf:first`/`rdf:rest`/`rdf:nil`) — **closable** LISP-style lists.

**RDF dataset** `DS = {DG, (i₁,G₁),…,(iₙ,Gₙ)}` = one **default (unnamed) graph** + zero or more **named graphs** identified by IRI/bnode. Bnodes may be reused across graphs *within one dataset*; SPARQL 1.1 forbids bnode graph names. **RDF merge** = union after renaming bnodes apart (so no bnode label clashes).

**Syntaxes:** N-Triples (batch), **Turtle** (compact, human-readable), TriG / N-Quads (datasets), RDF/XML (frame-based), **JSON-LD** (JSON + `@context`), RDFa (embed in HTML).

### 3.3 RDFS metamodeling + entailment regimes

**RDFS** (RDF Schema) is a light metamodeling vocabulary: `rdf:type`, `rdfs:Class`, `rdfs:subClassOf`, `rdf:Property`, `rdfs:subPropertyOf`, `rdfs:domain`, `rdfs:range`. (Multiple domains/ranges are read as **conjunction**.)

**Entailment regimes** (= a *semantic extension* fixing what is interpreted) — each is a **monotonic** extension of the previous:
1. **Simple entailment** — structural matching only (+ bnode renaming).
2. **D-entailment** — additionally interprets **datatypes**.
3. **RDF-entailment** — additionally interprets the `rdf:` vocabulary.
4. **RDFS-entailment** — additionally interprets `rdf:`+`rdfs:` vocabularies.

**Simple entailment** (`G₁ ⊨ G₂`): true iff every interpretation that makes `G₁` true makes `G₂` true.
- **Interpolation lemma:** `G₁ ⊨ G₂` **iff** some subgraph of `G₁` is an **instance** of `G₂`. Simple entailment is **NP** in graph size (fewer bnodes ⇒ easier).
- **Procedure for the richer regimes:** `G₁ ⊨_X G₂` iff `Clos_X(G₁)` simply entails `G₂`, where `Clos_X` adds axiomatic triples and applies the **RDF/RDFS entailment rules** (`rdfs2/rdfs3` domain/range, `rdfs5/rdfs11` subProperty/subClass transitivity, `rdfs7` subProperty, `rdfs9` type propagation, …) until exhaustion. This closure is **finite and polynomial**; the overall cost is dominated by the NP simple-entailment check.

### 3.4 SPARQL — query execution

**SPARQL 1.1** (W3C Rec 2013): query language + **update** language + protocol + **federation** (`SERVICE`) + result formats (XML/JSON/CSV/TSV) + entailment regimes. "SPARQL is to RDF what SQL is to RDBMS" (but weaker built-ins/subqueries).

**Query forms:**
- **SELECT** — a binding table;
- **ASK** — boolean (does a pattern exist?);
- **CONSTRUCT** — builds an RDF graph from bindings;
- **DESCRIBE** — RDF describing a resource (semantics implementation-defined).

**Core notions:**
- **solution / mapping** `μ : V → T` assigns RDF terms to (some) query variables.
- **result list** `R = (μ₁,…,μₙ)`.
- **triple pattern (TP)** ∈ `(T∪V) × (TI∪V) × (T∪V)`; **basic graph pattern (BGP)** = a set of TPs (matched conjunctively).

**Graph-pattern algebra** maps to relational operators:
- **conjunction** = sequence of patterns (`join`);
- **disjunction** = `UNION`;
- **conditional conjunction** = `OPTIONAL` (left outer join; left-associative; ordering of OPTIONALs can matter);
- **negation** = `FILTER NOT EXISTS` or `MINUS`;
- **`FILTER`** = selection (boolean condition over a BGP).
- **Property paths**: `^e` (inverse), `e1/e2` (sequence), `e1|e2` (alternative), `e*`/`e+`/`e?` (regular closure), `!(...)`; e.g. RDF-list items via `(rdf:rest*)/rdf:first`.
- **Aggregation**: `COUNT/SUM/AVG/MIN/MAX/GROUP_CONCAT/SAMPLE` with `GROUP BY` / `HAVING`; `BIND(expr AS ?v)`; `VALUES`; `ORDER BY/LIMIT/OFFSET`; `SELECT DISTINCT`.
- **Datasets in the query:** `FROM` / `FROM NAMED` choose active default/named graphs; `GRAPH ?g {…}` matches named graphs.
- **Federation:** `SERVICE [SILENT] <endpoint> { GP }` sends a sub-pattern to a remote endpoint.

**`MINUS` vs `FILTER NOT EXISTS`** (a classic gotcha):
- They usually agree, but differ when variables are **disjoint**: `?s ?p ?o MINUS {?x ?y ?z}` returns **all** rows (no shared vars ⇒ nothing removed), whereas `?s ?p ?o FILTER NOT EXISTS {?x ?y ?z}` returns **nothing** (the inner pattern always matches). Both implement *negation as failure* (CWA-flavored).

**SPARQL Update:** `INSERT [DATA]`, `DELETE [DATA]`, `DELETE…INSERT…WHERE` (replace), `LOAD/CLEAR/CREATE/DROP/COPY/MOVE/ADD`.

### 3.5 Possible **evaluation semantics of SPARQL** and their differences ⭐

This is the part the official topic specifically asks about — "*possible evaluation semantics and their differences*". Two orthogonal axes:

**(A) Bag (multiset) vs. set semantics.**
- Standard SPARQL uses **bag/multiset semantics**: duplicate solutions are kept (matching SQL's default). `DISTINCT` switches to **set semantics**. This affects `COUNT`, `UNION`, etc. — the same data can yield different cardinalities depending on which semantics you assume.

**(B) Entailment regime under which BGPs are matched** — *this is the key "different results" point.*
- By default, SPARQL matches BGPs under **simple entailment** (just subgraph matching, "what is explicitly stated").
- Optional **entailment regimes** ([SPARQL 1.1 Entailment Regimes]) evaluate BGPs under **RDF / RDFS / D / OWL 2 / RIF entailment**, returning also **implied** solutions.
- **Example (same query, different regimes)** — query `SELECT ?x WHERE { ?x :madeFromFruit _:d }` with
  `:BancroftChardonnay a :Chardonnay`, `:Chardonnay ⊑ ∃:madeFromGrape.⊤`, plus a wine made from a fruit:
  - **Simple / RDF entailment:** *no result* (the triple isn't asserted);
  - **RDFS entailment:** *one* result (`:ChateauDYchemSauterne`);
  - **OWL entailment:** *two* results (also `:BancroftChardonnay`, since "made from grape" ⇒ "made from fruit").
  → The **richer the entailment regime, the more (certain) answers** are returned.

**Constraints all SPARQL entailment regimes must satisfy:** (i) be compliant with the corresponding entailment; (ii) return **finite** results; (iii) return only **canonical bnodes** (achieved by skolemizing query + graph); (iv) return only a finite part of the relevant vocabularies.

**(C) Certain-answer semantics under OWA.** Because OWL/RDFS reason under the **Open World Assumption**, query answers under an entailment regime are **certain answers** (true in *every* model). Negation (`NOT EXISTS`/`MINUS`) and counting, however, behave in a **closed-world / negation-as-failure** way over the *queried graph* — a frequent source of confusion: schema-level reasoning is OWA, but the SPARQL algebra operators are CWA.

**Result serializations:** CSV (lossy — loses datatype/lang), TSV (lossless), XML/JSON (lossless, richest).

---

## 4. Linked Data: principles, Hash vs 303 URIs, the 5-star model

> **Official sub-topic:** *Defining principles of linked data. Hash vs. 303 URIs, Five star linked open data model.*

### 4.1 Linked Data principles (T. Berners-Lee, 2009)

**Linked Data** = a method for publishing **structured, interlinked** data on the web, built on **URIs + HTTP + RDF**. From *Web of Documents* (HTML, read by humans) to *Web of Data* (RDF, read by machines) — same plumbing (IRI identifiers, HTTP transfer).

**The four principles:**
1. Use **URIs as names** for things.
2. Use **HTTP URIs** so people can look them up.
3. When someone looks up a URI, **provide useful information** using the standards (**RDF, SPARQL**).
4. **Include links to other URIs** so they can discover more things.

A URI satisfying principle 3 (returns useful RDF when fetched) is **dereferenceable** (*dereferencovatelná*).

### 4.2 Document vs. resource — **Hash URIs vs. 303 URIs** ⭐

A URI scheme must distinguish **a real-world thing** from **the document describing it** (e.g. is `http://example.com/people/Mary` the *person* Mary or a *web page about* Mary?). Two strategies:

| | **303 URIs** | **Hash URIs** |
|---|---|---|
| Form | `http://id.example.org/people/Alice` | `http://example.org/people#Alice` |
| Mechanism | server returns **HTTP 303 See Other** redirect to a *separate* describing document; client re-requests; **content negotiation** (`Accept` header) picks RDF or HTML | a **single document** (the part before `#`) is fetched; the **fragment** (`#Alice`) is resolved **client-side** |
| Extra round-trips | yes (303 + follow-up) | no |
| Best for | **large datasets** (don't ship everything at once — good performance) | **small, stable datasets** (whole doc fetched anyway) |
| Why | server can point each resource to just its slice | the fragment after `#` is evaluated by the **client**, so it must download the whole document first |

Both rely on **HTTP content negotiation** so the same URI serves RDF to machines and HTML to humans.

### 4.3 The 5-star Open Data model ⭐

Tim Berners-Lee's deployment scheme (`5stardata.info`); each star **adds** to the previous:

| Stars | Requirement |
|---|---|
| ★ | Available on the web in **any** format, under an **open licence** (→ "Open Data"). |
| ★★ | Available as **machine-readable structured** data (e.g. Excel, not a scanned image of a table). |
| ★★★ | …plus a **non-proprietary** format (e.g. CSV instead of Excel). |
| ★★★★ | …plus use **open W3C standards (RDF + SPARQL)** to identify things, so people can point at them. |
| ★★★★★ | …plus **link** your data to other people's data to provide context (→ Linked Open Data). |

**Open vs. Linked** (two independent axes):
- **linked, not open** — enterprise / master data;
- **linked + open** — **5-star** Linked Open Data (the **LOD cloud**);
- **not linked, open** — typical open-data portals (CKAN/DataHub, `data.gov.cz` / NKOD);
- **not linked, not open** — irrelevant.

**Adopters:** public sector (`data.gov.cz`, `data.europa.eu`, `data.gov`), private (Google **Knowledge Graph**, Microsoft Satori, Facebook **Open Graph Protocol**, BBC, Ordnance Survey). Publishing tools: LodView, Pubby, D2R, Marmotta, Callimachus.

---

## 5. Supplementary topics (rest of the course)

### 5.1 Knowledge Organization Systems (KOS) and SKOS

**KOS** = any scheme for organizing information, ordered by *degree of control* (natural → controlled language) and *strength of semantic structure* (weak → strong):

| Weak structure → strong | Type | Note |
|---|---|---|
| | **Term lists** | authority files, glossaries, dictionaries, gazetteers |
| | **Classifications/categories** | subject headings (MeSH, LCSH), taxonomies |
| | **Relationship lists** | thesauri (broader/narrower/related), semantic networks, **ontologies** |

- **Controlled vocabulary** = restricted term list; only listed terms used; changes are the editor's responsibility.
- **Taxonomy** = terms in a hierarchy (usually "is-a kind-of"); single-parent (else *poly-hierarchy*).
- **Thesaurus** = controlled terms + relations: hierarchy (broader/narrower), equivalence (synonym), association (related).
- **Semantic network** = graph of concepts with richer relation types.
- **Ontology** = strongest KOS: controlled vocabulary + grammar + **axioms & rules** for machine inference.

**SKOS** (*Simple Knowledge Organization System*, W3C Rec 2009) — an RDF/OWL vocabulary to publish thesauri/taxonomies/subject headings as Linked Data. Core: `skos:Concept`, `skos:ConceptScheme`, `skos:prefLabel`/`altLabel`/`hiddenLabel`, `skos:broader`/`narrower`/`related`, `skos:inScheme`, `skos:notation`, `skos:definition`/`note`. Extensions: **SKOS-XL** (labels as first-class resources), **XKOS** (statistical classifications). Example: **EuroVoc** (EU activities thesaurus).

### 5.2 SNOMED-CT and the EL++ description logic ⭐

**SNOMED-CT** (*Systematized Nomenclature of Medicine – Clinical Terms*): international clinical terminology, **350k+ concepts**, multiple inheritance, curated by SNOMED International (40 member states incl. Czechia). Each concept has an **SCTID**, an **FSN** (Fully Specified Name), **synonyms** (preferred/acceptable), and **relationships**. (COVID certificates reference SNOMED codes, e.g. `840533007` for COVID-19.)

- **In OWL:** `Headache ⊑ ∃FindingSite.HeadStructure`, `Headache ⊑ HeadFinding`, … **Defined** concepts (`≡`) vs **primitive** concepts (`⊑`).
- **Logic:** SNOMED-CT is expressible in **EL++**, which corresponds to **OWL 2 EL**. Reasoning in EL++ is **polynomial** and **does not require satisfiability checking** — it is done by **subsumption/classification** (e.g. the **ELK** reasoner). Classifying SNOMED infers children of defined concepts (e.g. "Pain in left arm" gains 11 direct / 26 total children after classification).
- **EL++ reasoning** works over a **normalized TBox** (`A⊑B`, `A₁⊓A₂⊑B`, `A⊑∃r.B`, `∃r.A⊑B`) via completion rules **CR1–CR5** (reflexivity, ⊤, transitivity of ⊑, convexity, existential propagation). **Reference Sets (RefSets)** = curated subsets of components.
- *Paper "Is SNOMED-CT just for healthcare?"* — argues the modelling patterns generalize beyond medicine.

### 5.3 RDF generation/transformation: R2RML, RML, TARQL

Turning non-RDF data into RDF ("RDF-ization"):
- **TARQL** — CSV → RDF using **SPARQL `CONSTRUCT`** syntax (single-purpose CLI).
- **R2RML** — **W3C Recommendation**; **relational DB (SQL) → RDF**; mapping itself written in RDF.
- **RML** (*RDF Mapping Language*) — W3C-draft **generalization of R2RML** to **CSV/JSON/XML/SQL**; mappings in RDF. Tooling: RMLMapper, RMLStreamer, RML Editor, Ontotext Refine, LinkedPipes ETL, s-pipes. **YARRRML** = human-readable YAML syntax compiled to RML.
- **RML core:** a **logical source** (`rml:source`, `rml:referenceFormulation` = `ql:CSV`/`ql:XPath`/`ql:JSONPath`) + a **TriplesMap** with a **subjectMap** (`rr:template "…/{id}"`, `rr:class`) + **predicateObjectMaps** (`rr:predicate`, `rr:objectMap` with `rml:reference`/`rr:template`/`rr:datatype`).

### 5.4 Semantic GIS — GeoSPARQL

**GIS** = system to capture/store/analyze/present **spatial (geographic) data**. Key concepts: **Coordinate Reference System (CRS)** = reference ellipsoid + geodetic datum + map projection; identified by **EPSG codes** — **WGS-84 (EPSG:4326)** (GPS/Google), **ETRS-89 (4258)**, **S-JTSK (5514)** (Křovák, Czech). Data: **raster** vs **vector**; geometry types point/line/polygon (+ multi-).

**GeoSPARQL** (OGC standard) extends SPARQL for spatial data:
- vocabulary `geo:Feature`, `geo:Geometry`, `geo:hasGeometry`, `geo:asWKT` (literal `geo:wktLiteral`), GML alternative;
- **geometry serialization**: **WKT** (`POINT(50.056 14.434)`, `LINESTRING(...)`, `POLYGON/MULTIPOLYGON`) or **GML**;
- **topological relation families**: **Simple Features** (`geof:sfWithin/sfContains/sfIntersects/sfEquals/sfDisjoint/sfTouches/sfCrosses/sfOverlaps`), **Egenhofer**, **RCC8**; plus `geof:distance`, `buffer`, etc., used inside `FILTER`. Answers queries like "bus stops within 5 min walk", "monuments visible from my room".

### 5.5 RDF validation — SHACL and ShEx (and how they differ from OWL) ⭐

How do we **validate** that data conforms to a structure (vs. *infer* new knowledge)? Two languages: **SHACL** and **ShEx**.

**SHACL** (*Shapes Constraint Language*, W3C Rec 2017):
- **Node shapes** (`sh:NodeShape` + `sh:targetClass`/`sh:targetNode`/`sh:targetSubjectsOf`/`sh:targetObjectsOf`) + **property shapes** (`sh:path`, `sh:datatype`, `sh:minCount`/`maxCount`, `sh:minInclusive`/`maxInclusive`, `sh:minLength`/`maxLength`, `sh:pattern`, `sh:class`, `sh:nodeKind`, `sh:in`, `sh:languageIn`, `sh:uniqueLang`).
- **Property-pair** constraints: `sh:equals`, `sh:disjoint`, `sh:lessThan`, `sh:lessThanOrEquals`.
- **Logical**: `sh:not`, `sh:and`, `sh:or`, `sh:xone` (exactly one); shape-based `sh:node`, `sh:property`, **qualified** `sh:qualifiedValueShape` + `sh:qualifiedMin/MaxCount`; **`sh:closed true`** (closed shapes).
- **SHACL-Core** vs **SHACL-SPARQL** (constraints/targets/functions/rules expressed in SPARQL — `sh:sparql`, `sh:SPARQLConstraint`). **Validation report**: `sh:ValidationReport` with `sh:conforms`, `sh:result`, `sh:resultSeverity` (`sh:Info`/`Warning`/`Violation`), `sh:focusNode`, `sh:resultPath`, `sh:sourceConstraintComponent`.
- Every SHACL-Core constraint corresponds to a SPARQL `ASK`/`SELECT` (e.g. `minCount 1` ⇔ `ASK { $this … FILTER NOT EXISTS { $this :p ?v } }`).

**OWL vs SHACL — the key conceptual difference** ⭐:

| OWL | SHACL |
|---|---|
| **Open-world** | **Closed-world** |
| checks *any possible model* (tableau reasoner) | checks data *conforms to a structure* (via SPARQL) |
| **derives new knowledge** | **validates** existing data |

> Example: data `ex:AlanTuring a schema:Person.` against "Person ⊑ ≥1 givenName".
> - **OWL**: **consistent** — the missing name is "assumed to exist" (generated in some model under OWA).
> - **SHACL**: **invalid** — Alan has no `givenName` in the actual data (CWA).

**ShEx** (Shape Expressions) — alternative shape language with a grammar-style (regex-like) syntax; same goal as SHACL. Some triplestores (GraphDB, Stardog) offer partial SHACL support, validating on repository update.

### 5.6 Ontology engineering & upper (foundational) ontologies

**Ontology engineering** covers the full cycle: conceptualization → design → implementation → deployment, organized by an **ontology life-cycle model** (Waterfall, V, Incremental, Iterative, Evolutionary/Throwaway Prototyping, **Spiral** for high uncertainty).

- **Methodologies:** **METHONTOLOGY** (Madrid, IEEE-based; project mgmt + development + support), **NeOn** (scenario-based, **reuse/re-engineering**, ontology *networks*, 9 scenarios, **ODPs**), **SAMOD** (agile), plus On-To-Knowledge, Grüninger & Fox (TOVE), Uschold & King, KACTUS, SENSUS, DILIGENT.
- **Requirements:** **functional** (query results, inferences, error-checking) vs **non-functional** (coverage, efficiency, docs). Functional types: **Competency Questions (CQ)** (NL questions the ontology must answer), **contextual statements** (axioms in NL), **reasoning requirements**.
- **Guidelines:** taxonomy construction **top-down / bottom-up / middle-out** (combine!); reuse via `owl:imports` (whole), modules, **ODPs**, individual statements, entity references. *Three-layer* guidelines (top/middle/bottom) — middle/bottom matter most for novices.

**Upper / foundational ontologies** (*horní/zakladní ontologie*) — very general, domain-independent categories that "lower" (domain/task/application) ontologies specialize. **Pros:** modelling guidance, ready-made categories, interoperability. **Cons:** abstract, effortful.

**Basic ontological commitments** (the philosophical axes upper ontologies pick):
- Universals vs Particulars; Descriptive vs Realist; Multiplicative vs Reductionist; **Endurantism vs Perdurantism**; Actualism vs Possibilism; Concrete vs Abstract.

**Examples:** **UFO** (descriptive, multiplicative), **BFO** (realist, reductionist, actualist; SNAP endurants + SPAN perdurants; ISO/IEC 21838-2), **DOLCE** (descriptive, cognitive/linguistic bias, particulars, possibilism), SUMO, GFO, YAMATO, Cyc, WordNet, GIST.

**Ontology classification:** *top-level* (UFO/BFO/DOLCE) → *domain* (SNOMED) / *task* → *application*. (Beware **is-a vs part-of vs instance-of** confusion — OWL 2 has no built-in part-of; 5 kinds of part-of: functional, qualification, spatial/temporal, "stuff", material.)

### 5.7 UFO and OntoUML (conceptual-modelling foundations)

**UFO** (*Unified Foundational Ontology*, Guizzardi) underpins **OntoUML** (a UML profile for ontologically well-founded conceptual models). Core distinctions on **endurant types**:
- **Endurant** (changes state over time, e.g. *Person*) vs **Perdurant** (event/process, fixed in time, e.g. *Accident*).
- **Sortal** (all instances share one **principle of identity**, e.g. *Person*) vs **non-sortal** (e.g. *VehicleOwner* = humans or companies).
- **Rigidity**: **Rigid** `R+(T)=□∀x(T(x)→□T(x))` (e.g. *Person*); **Anti-rigid** `R~(T)=□∀x(T(x)→◇¬T(x))` (e.g. *DrivingLicenseHolder*); semi-rigid.
- **Relational dependence** `D+(T,T',R)`.

**UFO modules:** UFO-A (endurants), UFO-B (perdurants/events), UFO-C (social/intentional), UFO-S (services), UFO-L (legal), UFO-MLT (multi-level).
**OntoUML stereotypes:** classes — **Kind, Subkind, Role, Phase, Category, RoleMixin, Mixin**; associations — Formal, Mediation, MaterialDerivation, Characterization, Structuration, Part-Whole.

### 5.8 RDF persistence, indexing, inferencing, access control

**RDF stores (triplestores):** RDF4J, GraphDB, Virtuoso, Fuseki (Jena), Stardog, AllegroGraph, Amazon Neptune, RDFox, BlazeGraph, Fluree.

**Storage / indexing layouts:**
- **Triple table** (s,p,o) — simple but many **self-joins**; **Quad store** adds a **context/graph** column.
- **Property table** (subject + one column per property) — removes self-joins but **nulls** + trouble with multi-valued properties.
- **Vertical partitioning** (one 2-column table per predicate) — removes self-joins.
- **Dictionary encoding** (map IRIs/literals → integer ids) — saves space, removes redundancy.
- **Index permutations** (SPOC, POSC, …): more indexes ⇒ faster queries, slower updates, bigger disk.
- RDF4J backends: Memory store (speed), Native store (B-Trees on disk), Elasticsearch store (read-heavy).

**Inferencing (materialization strategies):**
- **Full materialization (forward-chaining, on write)** — infer + store new triples at load time (GraphDB, AnzoGraph OWL2-RL); fast queries, slow updates, **incomplete** for full OWL.
- **Query-time / runtime reasoning (backward-chaining, on query)** — infer on the fly (Stardog); slower queries, faster updates.
- Datalog/rule reasoners: **RDFox**; rule example: `owl:SymmetricProperty` → add reversed triple.

**Access control** (RDF security is generally hard; most offer only RBAC):
- **RBAC** (role-based), **ABAC** (attribute-based), **FGAC** (fine-grained, triple-level).
- **Stardog** — **named-graph** permissions (unauthorized graphs silently ignored on read; whole UPDATE rolled back on unauthorized write) + **property-based security**: queries are rewritten to **mask** sensitive predicates `INSERT {?s ?p ?masked} DELETE {?s ?p ?o} WHERE { … FILTER(?p in S) BIND(mask(?o) AS ?masked) }` — masking a value also **breaks property chains** through it.
- **GraphDB Enterprise** — scopes/roles for FGAC; **Fluree** — RelBac/SmartFunctions.

**Programmatic access (Java/Kotlin):** **low-level APIs** work with statements — **OWLAPI** (OWL 2 reference impl., pluggable reasoners), **Jena**, **RDF4J**; **high-level (object-mapping) APIs** — **JOPA**, JAOB (work with objects, like an ORM for ontologies).

### 5.9 LLMs and Knowledge Graphs

Complementary strengths:

| LLMs | Knowledge Graphs / ontologies |
|---|---|
| built automatically from un/structured text | built manually or from structured content |
| fluent, broad generative inference | **precise, well-explainable** |
| **weak on truth & explainability** | weak on authoring & generative reasoning |
| heavy compute | heavy manual curation |

- **What LLMs gain from KGs:** disambiguation, provenance/governance, **explainability via schemata** (OWL/RDFS/SHACL).
- **What KGs gain from LLMs:** entity/relation extraction at scale, schema suggestions, mapping/alignment hints, **NL → SPARQL/Cypher** interfaces.
- **Integration patterns:** (1) **Knowledge creation** — LLM proposes candidate triples/classes/SHACL shapes → **SHACL validation** → manual curation → KG; (2) **Explainable query answering / GraphRAG** — augment the prompt with **SPARQL results** to improve answer quality and cut tokens, with KG-grounded justifications.

---

## 6. Cross-subject links
- **Open/Closed World, monotonicity, FOPL undecidability** ↔ TAL (decidability, recursively enumerable languages).
- **Complexity classes (PSPACE, ExpTime, NExpTime, NP)** ↔ TAL (complexity hierarchy) and PAL.
- **RDF graph model, SPARQL** ↔ DS2 (SPARQL, graph data, NoSQL/triple stores, indexing).
- **Tableau / proof procedures** ↔ propositional/predicate-logic tableaux (TAL/AI background).

## 7. References (authoritative sources, verified)
- F. Baader, D. Calvanese, D. McGuinness, D. Nardi, P. Patel-Schneider (eds.), *The Description Logic Handbook*, Cambridge, 2003 (Ch. 2–4) — DL syntax/semantics, tableau, complexity.
- F. Baader, C. Lutz, *Description Logic* (Handbook of Modal Logic, 2006) — ALC PSPACE/ExpTime complexity.
- V. Mařík, O. Štěpánková, J. Lažanský, *Umělá inteligence 6*, Academia, 2013 (Ch. 2–4); P. Křemen, *Ontologie a deskripční logiky*.
- W3C: [RDF 1.1 Primer](https://www.w3.org/TR/rdf11-primer/), [RDF 1.1 Semantics](https://www.w3.org/TR/rdf11-mt/), [SPARQL 1.1 Query](https://www.w3.org/TR/sparql11-query/), [SPARQL 1.1 Entailment Regimes](https://www.w3.org/TR/sparql11-entailment/), [OWL 2 Primer](https://www.w3.org/TR/owl2-primer/), [OWL 2 Profiles](https://www.w3.org/TR/owl2-profiles/), [SKOS Primer](https://www.w3.org/TR/skos-primer/), [SHACL](https://www.w3.org/TR/shacl/).
- I. Horrocks, B. Motik et al. — SWRL & **DL-safe rules** (decidability via binding to named individuals).
- B. Motik, B. Cuenca Grau et al. — **OWL 2 Profiles** complexity (EL PTime, QL NLogSpace/AC0, RL PTime; SROIQ N2ExpTime).
- T. Heath, C. Bizer, *Linked Data: Evolving the Web into a Global Data Space*, 2011; T. Berners-Lee, [Linked Data design issues](https://www.w3.org/DesignIssues/LinkedData.html); [5stardata.info](https://5stardata.info/).
- G. Guizzardi, *Ontological Foundations for Structural Conceptual Models*, 2005 (UFO/OntoUML); SNOMED International; OGC GeoSPARQL.
- Course pages: <https://cw.fel.cvut.cz/wiki/courses/osw>; DL playground: <http://kbss.felk.cvut.cz/tools/dl>.
