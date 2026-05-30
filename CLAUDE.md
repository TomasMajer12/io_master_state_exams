# CLAUDE.md — Project Context

## Project Purpose

Create complete **summary and study materials** for the **CVUT FEL Open Informatics (OI) Master state exam**, specialization **Data Science**.

The user (Tomas) is preparing for the state exam and will incrementally provide source materials (lecture slides, examples, his own study notes, past exam questions). Claude's job is to turn those raw inputs into well-structured, exam-ready study materials per subject.

## Source of Truth for Topics

`state_exam_topic_specification.pdf` (2025-02-28) is the canonical list of exam topics.
The extracted, structured version lives in `00_subjects_overview.md`.

The state exam covers **9 subjects total**:
- **Common part** (3): BE4M33PAL, BE4M01TAL, BE4M35KO
- **Data Science specialization** (6): BE4M36SAN, BE4M39VIZ, BE4M33OSW, BE4M33SSU, BE4M36SMU, BE4M36DS2

## Directory Convention

One directory per subject under either `common/` or `data_science/`. Directory names use the format `<COURSE_CODE>_<short_name>/`, e.g. `BE4M36SAN_statistical_analysis/`. The full layout is documented in `00_subjects_overview.md`.

Inside each subject directory:

| File / folder | Purpose |
|---|---|
| `materials/` | Raw input files provided by the user (PDFs, slides, examples, notes). Read-only — Claude does not modify these. |
| `summary.md` | Main deliverable. Structured theory + examples, organized to match the official topic subpoints from the PDF. |
| `questions.md` | Past exam questions specific to this subject, cross-referenced back to sections in `summary.md`. Populated incrementally from sources in `frequently_asked_questions/`. |
| `examples.md` | Worked-out problems / numerical examples (optional, when there are many). |

Additionally, the project root contains a `frequently_asked_questions/` directory that holds **cross-subject raw archives** of past exam questions (the OI-Wiki committee dump and any future sources). When Tomas drops a new source there, Claude should:
1. Process it into a structured Markdown file inside `frequently_asked_questions/`.
2. Propagate the relevant subject-specific entries into each subject's `questions.md`, with a back-reference to the source file.

## Format Choice

Default to **Markdown (`.md`)** — easier to preview in the editor, renders nicely on GitHub

## Style Guidelines for Study Materials

When generating `summary.md` for a subject:

1. **Mirror the official topic structure.** The exam topics in `00_subjects_overview.md` are the skeleton — each bullet point becomes a section heading. Don't reorganize unless there's a strong pedagogical reason, and call it out if you do.
2. **Theory first, then example.** Every non-trivial concept should be followed by at least one concrete worked example.
3. **Show derivations** for formulas the student would need to reproduce on the exam. Don't just state results.
4. **Cross-reference between subjects** when topics overlap (e.g. PCA appears in SAN and is reused in VIZ; EM appears in SAN and SSU).
5. **Mark exam emphasis.** When the user shares past exam questions, add a `⭐ Frequently asked` annotation next to the relevant sub-topic so it's visible during review.
6. **Language of `summary.md` is decided per subject.** The oral state exam is in Czech, but most DS source materials are in English. At the start of each subject, Claude asks Tomas which language to use — typically:
   - **Czech-primary** when the lecture slides are in Czech (e.g. TAL) or the Czech terminology is what the examiner expects.
   - **English-primary** when the lecture slides and standard textbooks are in English (most DS subjects).
   - In either case, **add the other language in parentheses for key terminology** (e.g. "empirical risk minimization (empirické riziko)" or "rekurzivně spočetný jazyk (recursively enumerable language)") so Tomas can rehearse both versions for the oral exam.
7. **Keep it dense but readable.** This is a study cheat-sheet, not a textbook. Bullet points and tables are fine; long prose paragraphs are not.

## Workflow

Typical interaction:
1. User says "let's work on SAN" (or any subject) and uploads slides / notes into `data_science/BE4M36SAN_statistical_analysis/materials/`.
2. Claude reads everything in `materials/`, cross-references against the topic list in `00_subjects_overview.md`, and produces / updates `summary.md`.
3. User reviews, points out gaps, provides past exam questions → Claude updates `questions.md` and refines `summary.md`.
4. Repeat per subject.

When the user uploads new materials for a subject that already has a `summary.md`, **extend / refine** the existing file rather than rewriting from scratch.

## Things to Always Do

- Before creating a new `summary.md`, **re-read the relevant section of `00_subjects_overview.md`** so the structure stays aligned with the official topics.
- Before extending an existing `summary.md`, **read the current version first** to avoid duplicating sections or contradicting earlier explanations.
- When in doubt about scope or depth, ask the user (AskUserQuestion) rather than guessing.
- **Verify all information on the internet from relevant, authoritative sources.** Use `WebSearch` / `mcp__workspace__web_fetch` to cross-check definitions, formulas, algorithms, and historical attributions before writing them into `summary.md`. Preferred sources, in order:
  1. **Official course pages on CVUT FEL** (linked from `00_subjects_overview.md` — each subject has a "Course web pages" link in the original PDF). These reflect what the lecturers actually expect.
  2. **Standard textbooks** referenced by the course (e.g. Hastie/Tibshirani/Friedman — ESL; Bishop — PRML; Murphy — PML; Russell & Norvig — AIMA; Cormen et al. — CLRS).
  3. **Original research papers** (arXiv, conference proceedings) for advanced topics.
  4. **Reputable encyclopedic sources** (Wikipedia, Scholarpedia) — use only for cross-checking, not as primary source.
  - When sources disagree, **prefer the course-specific definition** and note the discrepancy.
  - **Cite sources** in `summary.md` (footnotes or a "References" section per topic) so Tomas can drill deeper if a topic is unclear.

## Things to Avoid

- Don't replace the user's source materials in `materials/` — that's raw input.
- Don't generate huge unsolicited deliverables — work subject-by-subject as the user provides inputs.
- Don't use emojis unless explicitly asked (the ⭐ for frequently-asked questions is the one exception).

## User Info

- **Name:** Tomas
- **Language:** Czech / English mixed. The oral state exam is in Czech. Summary language is decided per subject (see Style Guidelines #6).
- **Program:** ČVUT FEL, OI Master, Data Science specialization
