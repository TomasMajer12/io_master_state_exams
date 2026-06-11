# BE4M33OSW — Ontologies and Semantic Web (OSW) — Questions

The cross-subject FAQ archive (`../../frequently_asked_questions/oi_wiki_committee_archive.md`) currently has **no OSW past-paper questions** — OSW is a newer DS subject not represented by the wiki contributors. Add real past-paper questions here as they appear.

Until then, the list below is **anticipated questions derived from the official topic structure** (not from a past-paper archive), with cross-references to `summary.md`. Items marked ⭐ are the ones the official spec emphasizes most.

## Topic 1 — Description Logics
- ⭐ Define ALC: concept constructors and their semantics; give the FOPL reading of a sample axiom. → §1.2
- ⭐ State all key reasoning problems (TBox + ABox) and show they reduce to (un)satisfiability/consistency. Derive subsumption→unsatisfiability. → §1.3
- ⭐ Run the **tableau algorithm** for ALC on a given concept/ABox (NNF, rules →⊓/→⊔/→∃/→∀, clash, model extraction). → §1.4, examples.md C/E
- ⭐ How are **general TBoxes** handled (internalization `⊤⊑⊤_C`)? Why can the tableau loop, and what are **blocking conditions** (subset blocking for ALC)? → §1.5, examples.md D
- Explain the DL naming letters; what is SHOIN(D) vs SROIQ(D)? → §1.6
- ⭐ Give the **complexity** of key reasoning for ALC (no TBox vs general TBox), SHOIN, SROIQ, EL++. → §1.7
- Tree-model / finite-model property — what are they and why do they matter? → §1.2
- OWA vs CWA, UNA — how do they affect DL/OWL reasoning? → §0, §1.2

## Topic 2 — Semantic Web stack, RDF, OWL, SPARQL, SWRL, DL-safe rules
- Draw/describe the **Semantic Web stack** layer by layer. → §2.1
- OWL: punning, global (decidability) constraints, Direct vs RDF-Based semantics, Correspondence Theorem. → §2.2
- ⭐ The three **OWL 2 profiles** (EL/QL/RL): backing DL, intended use, reasoning method, complexity. → §2.3
- ⭐ What is **SWRL**? Why is it undecidable? How do **DL-safe rules** restore decidability and keep tractability relative to the base logic? → §2.4

## Topic 3 — RDF graph model, blank nodes, SPARQL semantics
- Define the RDF graph model: terms, IRIs, literals, datatypes. → §3.1
- ⭐ **Blank-node semantics** (existential variables); ground/instance/lean graphs; skolemization; RDF merge. → §3.2
- RDFS vocabulary + the four **entailment regimes** (simple/D/RDF/RDFS); interpolation lemma; closure procedure. → §3.3
- SPARQL execution: query forms, BGPs, solution mappings, algebra (UNION/OPTIONAL/MINUS/FILTER/paths/aggregation). → §3.4
- ⭐ **Possible evaluation semantics of SPARQL and their differences**: bag vs set; entailment regime (simple vs RDFS vs OWL → different results); certain-answers under OWA vs CWA negation. → §3.5, examples.md G/H

## Topic 4 — Linked Data
- State the four **Linked Data principles**; what makes a URI dereferenceable? → §4.1
- ⭐ **Hash URIs vs 303 URIs**: forms, mechanisms, when to use each, and *why* (client-side fragment vs server redirect). → §4.2
- ⭐ The **5-star** Open Data model (each level); open vs linked data quadrants. → §4.3

## Supplementary (if asked)
- SKOS / KOS types; SNOMED-CT & EL++ classification. → §5.1, §5.2
- R2RML/RML, GeoSPARQL. → §5.3, §5.4
- ⭐ **SHACL vs OWL** (closed vs open world; validation vs inference) — common conceptual question. → §5.5
- Upper ontologies (UFO/BFO/DOLCE), endurant/perdurant, sortal/rigid, OntoUML stereotypes. → §5.6, §5.7
- RDF store indexing (triple/property/vertical-partition/dictionary), materialization vs query-time reasoning, access control. → §5.8
