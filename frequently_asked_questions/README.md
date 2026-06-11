# Frequently Asked Questions — Archive of Past State-Exam Questions

This directory collects raw question archives gathered from various sources. The processed, per-subject version of each question lives in the corresponding `questions.md` inside the subject directory (e.g. `data_science/BE4M36SAN_statistical_analysis/questions.md`).

## Current sources

| File | Source | Coverage | Notes |
|---|---|---|---|
| `oi_wiki_committee_archive.md` | OI-Wiki state-exam committee wiki (Czech/Slovak), exams 2014–2023 | All 9 subjects (PAL, TAL, KO + 6 DS subjects); only SAN, VIZ, DS2 have actual entries for DS — OSW, SSU, SMU are empty in this source. | Topic-frequency tables + verbatim Q&A grouped by topic; preserves examiner names and the candidate's notes on follow-up questions. |
| `source_oi_wiki_committee_archive.pdf` | Original PDF | Source file for the above. | Read-only — do not edit. |

## How to add a new FAQ source

1. Drop the raw source (PDF, web export, screenshots, etc.) into this directory with a `source_<name>.<ext>` filename.
2. Ask Claude to process it: extract questions, classify them by subject (PAL/TAL/KO/SAN/VIZ/OSW/SSU/SMU/DS2), and produce a structured `<name>.md` next to the source.
3. Claude will also propagate the new questions into each subject's `questions.md`, with cross-references back to the source file.

## Naming convention

- Raw source: `source_<short_name>.<ext>` (e.g. `source_2024_forum_export.html`).
- Processed Markdown: `<short_name>.md` (e.g. `2024_forum_export.md`).

Both files live in this directory; subject-specific Q&A lives in the subject folders.
