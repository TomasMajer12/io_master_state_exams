# BE4M39VIZ — Visualization (VIZ) — Frequently Asked Questions

Past-exam questions specific to **BE4M39VIZ**, cross-referenced to `summary.md`.
Source: `../../frequently_asked_questions/oi_wiki_committee_archive.md` (OI-Wiki committee dump).

**Topic frequency:**

| Topic | Times asked | Summary section |
|---|---:|---|
| Volumetric data / direct volume rendering | 1 | §3 |
| Scalar field visualization (2D grid, color mapping, contouring) | 1 | §2 |
| Scalar vs. vector visualization, marching cubes (common-part GI question) | 2 | §2, §4 |

## Questions

### Q1 — 26.06.2018 (Oborová), examiner: Sedláček — DVR ⭐

> Vizualizace volumetrických dat. Popište dva přístupy pro vykreslování vol. dat (přímé a nepřímé vykreslování), uveďte příklady metod. Popište detailně dvě metody vykreslování, a to metodu **First Hit** a **Average Intensity Projection** (chtěli i vzorec pro AIP).

→ `summary.md` §3 (indirect vs. direct table; §3.1 ray functions — AIP formula and first hit explicitly covered). Student verdict: knew the principle of rays and 3D→2D, failed on the AIP formula → know I(p) = (1/n)Σs(tᵢ) and the first-hit isosurface test.

### Q2 — 05.09.2017 (Oborová), examiner: Malý — scalar fields ⭐

> Charakterizovat skalární data ve 2D uniformní mřížce. Popsat 2 nejběžnější metody, jak tato data vizualizovat. S jakými problémy se při vizualizaci potýkáme a jak je řešit?

→ `summary.md` §1 (grid characteristics) + §2 (color mapping incl. colormap design and HSV vs. HCL, contouring incl. marching squares & ambiguity). Follow-up at the exam: how many colors can a human distinguish; why HSV is a poor choice (→ §2.1 perceptual uniformity).

### Q3 — 2011/2012 (Společná, GI) — scalar vs. vector, marching cubes

> Rozdíly mezi vizualizací vektorových a skalárních dat; algoritmy používané při vizualizaci skalárních dat (marching cubes apod., mapování na barvy).

→ `summary.md` §2 (marching squares/cubes, color mapping) vs. §4 (why vector fields need glyphs/stream objects/LIC — sparse vs. dense, no interpolation of arrows).

> (Same era, Slavík) Reprezentace 3D objektů, množinové operace, převody mezi reprezentacemi — spíše DPG/grafika, ale souvisí s B-rep vs. voxel reprezentací (→ §3 voxelization, indirect rendering).

## Observations for preparation

- Both specialization questions so far target the **scientific-visualization half** (scalar fields + DVR) — prioritize §2 and §3, including formulas (AIP, compositing, linear interpolation of edge intersections) and the asymptotic decider story.
- Examiners ask follow-ups on **perception** (number of distinguishable colors, HSV vs. HCL, color-blind safety) — be ready to justify colormap design choices.
- The oral exam is in Czech — rehearse the Czech terms in parentheses in `summary.md`.
