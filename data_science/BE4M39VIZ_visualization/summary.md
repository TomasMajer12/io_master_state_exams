# BE4M39VIZ — Visualization (VIZ) — Study Summary

**Language:** English-primary, Czech terms in parentheses for the oral exam.
**Sources:** Lectures 01–13 by L. Čmolík & P. Slavík (in `materials/`), cross-checked against Munzner — *Visualization Analysis and Design* (course textbook), Telea — *Data Visualization: Principles and Practice*, Aigner et al. — *Visualization of Time-Oriented Data*, and original papers (see References).

Official topic list (from `../../00_subjects_overview.md`):

1. Characteristics of spatial data and spatial fields. Grids and their relation with data enrichment. Visualization of scalar fields with color mapping and contouring. Direct volume rendering.
2. Visualization of vector fields with glyphs, stream objects, and line integral convolution (LIC).
3. Characteristics of abstract data. Visualization of tabular (n-dimensional) data. Visualization of relational data (hierarchies, DAGs, undirected graphs).
4. Visualization of text. Single document. Document corpus.
5. Visualization of time-varying data. Time primitives and relations between them. Time domain, its type, granularity and structure.

---

## 0. Foundations (context for all topics)

### Visualization pipeline (vizualizační proces)

```
data → FILTERING → DATA ENRICHMENT → MAPPING → RENDERING → image
```

- **Filtering (filtrování)** — choose the portion of data to analyze (based on attributes).
- **Data enrichment (obohacení dat)** — interpolating/approximating raw data, i.e. reconstructing a continuous model from samples. **Only meaningful for spatial data** (the grid tells us from which samples to interpolate). For abstract data enrichment makes no sense (interpolating between two filled-in questionnaires is nonsense).
- **Mapping (mapování)** — data → visual elements (marks + channels).
- **Rendering (vykreslení)** — creating the image.

### Dataset types (typy datových sad)

| Type | Components | Techniques |
|---|---|---|
| **Spatial fields (prostorová pole)** | grid (positions, connectivity, cells) + attributes | scientific visualization |
| **Geometry (geometrie)** | mesh: vertices, edges, faces; typically *no* attributes | computer graphics; in viz it is often the *result* of an operation (contouring, stream ribbon) |
| **Tabular (tabulková)** | items (rows) × attributes (columns) | information visualization |
| **Relational (relační)** | items (nodes) + links, both with attributes | information visualization |
| **Text** | document → sections → words; corpus of documents | information visualization |
| **Geographical (geografická)** | geometry (points, polylines, areas) + attached abstract data | mix of both |
| **Time-varying (časově proměnná)** | attributes and/or topology change in time | see §9 |

### Attribute types (typy atributů)

- **Nominal/categorical (nominální)** — no inherent order (cities, diseases).
- **Ordinal (ordinální)** — ordered, but intervals not measurable (XS, S, M, L; Mon, Tue…).
- **Quantitative (kvantitativní)** — ordered + arithmetic possible; discrete (ints) or continuous (floats).

Ordering direction: **sequential** (min→max, e.g. mountain height), **diverging (divergující)** (two directions from a meaningful zero, e.g. elevation incl. ocean depth), **cyclic (cyklické)** (wraps around, e.g. hour of day).

### Marks and visual channels (značky a vizuální kanály)

Marks = geometric primitives (points, lines, areas). Channels control their appearance.

**Effectiveness ranking** (Munzner 2014; magnitude channels for ordered attributes, top = most accurate):
position on common scale > position on aligned scale > length > tilt/angle > area > depth (3D position) > color luminance/saturation > curvature/volume.
**Identity channels for categorical attributes:** spatial region > color hue > motion > shape. **Grouping channels:** containment, connection, proximity, similarity.

- **Expressiveness principle:** encode all of, and *only*, the information in the data (ordered data must look ordered, unordered must not imply order).
- **Effectiveness principle:** most important attributes → highest-ranked channels (Mackinlay 1986).
- Rankings come from **accuracy** (Stevens power law S = Iⁿ), **discriminability** (number of distinguishable steps; e.g. line width only 3–4), **separability** (position+hue separable; width+height interfere; R,G,B channels interfere strongly — never encode 3 attributes into RGB), **popout** (pre-attentive processing, <200–250 ms, independent of number of distractors; works for single channels, fails for conjunctions of features).

### Task classification (klasifikace úloh) — actions × targets

Abstracting the task separates it from domain language ("compare values between two groups").

- **Actions:** Analyze {Consume: discover, present, enjoy; Produce: annotate, record, derive}; Search {lookup (target+location known), locate (target known, location unknown), browse (location known/characteristics known, target unknown), explore (both unknown)}; Query {identify (1 target), compare (several), overview/summarize (all)}.
- **Targets:** all data {trends, outliers, features}; attributes {one: distribution, extremes; many: dependency, correlation, similarity}; networks {topology, path}; spatial data {shape, distance/proximity}.

---

## 1. Spatial data, spatial fields, grids, data enrichment

### Characteristics

A spatial field is the sampled representation of an underlying continuous function

**F(x): x = (x₁,…,xₙ) → F = (F₁,…,Fₘ)**

- x = independent variables (positions where we measured), F = dependent variable.
- Notation **EⁿS** / **EⁿVᵐ**: n = spatial dimension, S = scalar (m=1), V = vector (m>1).
  Examples: rainfall over a plane = E²S; flow in a volume = E³V3; CT scan = E³S.
- **Position is part of the data** → we *cannot* map data on position (unlike abstract data); position is already used to anchor the samples.

### Grid (mřížka)

Spatial field = **grid + attributes**.

- Grid = **positions** (vertices/nodes) + **connectivity** (edges) + **cells** (buňky).
- Attributes (scalars, vectors, tensors, multivariate) can sit in **positions** and/or **cells** (we mostly use attributes in nodes).
- Cell types 0D–3D: point, line, triangle, rectangle / tetrahedron, pyramid, cube (hexahedron), prism.
- Grid taxonomy: **structured** — uniform/regular, rectilinear, curvilinear (implicit connectivity) vs. **unstructured** (explicit connectivity, e.g. triangulation of scattered points, tetrahedral grids).

### Relation with data enrichment (obohacení dat)

The grid tells us **from which samples to interpolate** → enrichment = reconstruction of the continuous field:

- nearest-neighbor, (bi/tri)linear, cubic interpolation.
- **Bilinear**: 3 linear interpolations from 4 samples; **trilinear**: 7 linear interpolations from 8 samples.
- ⚠ The **connectivity matters**: the same scattered points triangulated differently give different interpolated values and different contours (classic exam picture: diagonal of a quad flipped → interpolated value changes, e.g. ~7.8 vs ~5.2).

### Geometry vs. field

Geometry (mesh) describes shape (vertices, edges, faces), carries no attributes, and is the domain of computer graphics. In visualization geometry typically *results* from operations on fields (isosurface from marching cubes, stream ribbon…).

---

## 2. Scalar fields: color mapping and contouring ⭐ Frequently asked

> ⭐ Past exam (2017, Malý): "Characterize scalar data on a 2D uniform grid; describe the 2 most common visualization methods; problems and solutions." — i.e. exactly color mapping + contouring, plus colormap design. See `questions.md` Q2.

2D scalar field can be viewed as a **height field** above the plane. Two basic methods: **color mapping** (mapování na barvy) and **contouring (konturování)**.

### 2.1 Color mapping (mapování na barvu)

Data value → color via a **colormap** (transfer of scalar to color, typically via lookup table).

**Functions of a good colormap:**

1. invertible (with legend we can read the value back) — don't reuse the same hue/lightness twice;
2. perceptually uniform — equal data changes are perceived as equal color changes;
3. implies correct ordering (sequential/diverging/cyclic);
4. works in grayscale (lightness carries the ordering);
5. works for color-blind people (prefer blue↔yellow transitions, rely on lightness);
6. intuitive mapping (data from nature → nature-inspired colors).

**Color spaces (barevné prostory):**

- Colorimetry: color matching experiment → **CIE RGB** color matching functions r(λ), g(λ), b(λ) (r(λ) partially negative).
- **CIE XYZ** — device-independent linear transform of CIE RGB, non-negative; Y = luminance; lights are theoretical. Normalization x+y+z=1 → **chromaticity diagram (CIE xyY)**.
- XYZ is **not perceptually uniform** (Euclidean distance ≠ perceived difference). → **CIE L\*a\*b\*** (perceptually uniform; L = lightness, a = green–red, b = blue–yellow) and its polar form **HCL** (hue, chroma, lightness) — the space in which colormaps should be designed.
- **HSV is not perceptually uniform** → rainbow/jet colormap is bad: non-uniform in hue and lightness, fake boundaries, not color-blind safe, fails in grayscale. Good: **Viridis, Plasma** (perceptually uniform paths in CIELab, long path = high discriminability; Viridis additionally color-blind friendly), gray (precise but low discriminability).
- Match colormap type to ordering direction: sequential data → sequential map; diverging data (meaningful zero, e.g. sea-surface height) → diverging map (hue change at zero); cyclic data (tidal phase) → cyclic map.

### 2.2 Contouring (konturování)

**Contour** for isovalue x: C = {p ∈ D | s(p) = x} — **isoline** (izolinie) in 2D, **isosurface (izoplocha)** in 3D. Contouring is a *filter* (keeps only chosen isovalues) producing *geometry*.

Properties:

- contours are closed curves/surfaces that never intersect (one point can't have two values);
- tangent = direction of zero variation; perpendicular = **gradient** (maximal variation);
- contours at uniform data intervals → spacing indicates gradient magnitude (dense = steep);
- color banding alone = contouring into discrete intervals; combined color mapping + contour lines shows both smooth variation and exact values.

**Marching squares** (2D, structured grid):

1. Classify each node: above/below isovalue (binary).
2. Per cell: 4 nodes → 2⁴ = 16 topological states, reduced to 8 by inversion (symmetry).
3. Intersection points on edges between above/below nodes by **linear interpolation**: q = vᵢ + (v−s(vᵢ))/(s(vⱼ)−s(vᵢ)) · (vⱼ−vᵢ).
4. Connect intersections by line segments according to the state table.

**Ambiguity (nejednoznačnost):** 2 ambiguous states (opposite corners above). Triangulating the quad does NOT help (two possible triangulations → two different answers). The exact contour of the bilinear interpolant is a **hyperbola**; for ambiguous cells the intersection of its asymptotes lies inside the cell → **asymptotic decider** (Nielson & Hamann 1991): evaluate the bilinear interpolant at the intersection [s,t] of the asymptotes; if value > isovalue connect the "above" corners, else the "below" corners.

**Contouring on irregular/scattered data:** triangulate (e.g. Delaunay), then per-triangle — triangles have **no ambiguous states**. But the resulting contour depends on the chosen triangulation (connectivity!).

**Marching cubes** (3D, structured grid; Lorensen & Cline 1987):

- 8 nodes → 2⁸ = 256 cases → **15 topological states** by inversion + rotational symmetry.
- Output: triangles forming the isosurface; must join consistently across cell faces.
- **Ambiguous faces** (same 2D ambiguity on a cube face; X ambiguous faces → 2ˣ divisions): naive table lookup can produce **holes** in the isosurface when adjacent cells resolve a shared ambiguous face differently (e.g. state 6 next to inverted state 3). Fixes:
  - **asymptotic decider** applied per ambiguous face;
  - **marching tetrahedra** (Payne & Toga 1990): decompose each cube into 5 or 6 tetrahedra (with 5, mirror neighbor cubes so edges align); tetrahedron: 2⁴=16 → 8 states, **no ambiguity**; produces more triangles.
- **Shading**: normal = normalized **gradient** of the scalar field; estimate by **central differences** at nodes, ∇f ≈ ((f(x+h)−f(x−h))/2h, …), interpolate gradient to the intersection vertices.

---

## 3. Direct volume rendering (přímé vykreslování objemových dat) ⭐ Frequently asked

> ⭐ Past exam (2018, Sedláček): "Describe the two approaches to rendering volumetric data (direct & indirect), give example methods; describe in detail First Hit and Average Intensity Projection (incl. formula)." See `questions.md` Q1.

**Volumetric data** = 3D scalar field (E³S); sources: CT, MRI, seismic/atmospheric measurement, CFD and physics simulations, voxelization of B-rep. Problem sizes: 1000³ = 10⁹ voxels ≈ 4 GB (ints) → memory + interactivity are core problems. Enrichment: nearest-neighbor / **trilinear** / cubic interpolation.

### Indirect vs. direct (nepřímé vs. přímé vykreslování)

| | Indirect | Direct (DVR) |
|---|---|---|
| Idea | extract intermediate **geometry**, render it | map volume → image directly, **no intermediate geometry** |
| Methods | slicing planes (3D→2D + color mapping), tiny cubes/spheres (filtered, colored, projected), **isosurface via marching cubes/tetrahedra** + surface rendering | ray function, ray integration (ray marching/casting) |
| Pros | easy shading, good shape perception, fast (conversion and rendering separate) | shows interior, semi-transparency, X-ray-like or surface-like, great flexibility |
| Cons | occlusion, no context (everything except the surface is discarded) | high computational cost, large memory |

### Ray marching (pochod paprsku)

For each pixel p of image I: construct ray r ⊥ I through p, intersect with the data block (points p₀, p₁), march in small steps between the intersections (front-to-back or back-to-front), sampling the field by trilinear interpolation. Rays are independent → trivially parallel. Two ways to turn samples into a pixel color: **ray function** or **ray integration**.

### 3.1 Ray functions (funkce paprsku) ⭐

Pixel intensity = simple function of the scalar values s(t) sampled along the ray; the result is then color-mapped:

- **Maximum intensity projection (MIP):** I(p) = max over the ray of s(t). Emphasizes high values (vessels with contrast agent); **no depth cues**.
- **Average intensity projection (AIP):** I(p) = (1/n) Σᵢ s(tᵢ) — discretization of (1/‖p₁−p₀‖)∫s(t)dt. Equivalent to an **X-ray** image. (⭐ formula explicitly demanded at the 2018 exam.)
- **Distance to value σ:** I(p) = distance from p₀ to the first sample with s ≥ σ (depth map of a feature).
- **Isosurface / first hit σ:** does s = σ occur along the ray? Shade the first hit → image like marching cubes but without geometry extraction (software/hardware ray casting).

### 3.2 Ray integration (integrace podél paprsku)

Every sample is mapped to **color AND opacity** by a **transfer function**, then composited.

**Transfer function (přenosová funkce):**

- maps voxel data values → optical properties (color, opacity, possibly specularity): emphasizes/suppresses/classifies features;
- typically piecewise-linear / lookup table; edited interactively by the user (tricky to specify);
- **1D** (density) or **2D** (e.g. density × gradient magnitude — highlights material boundaries; density × angle between view direction and gradient) or higher.
- Note: this color+opacity mapping ≠ plain color mapping of §2 (adds opacity, classification role).

**Optical model (optický model):** light interacts with volume "particles" via **emission**, **absorption** (+ optionally scattering). Each sample emits intensity and absorbs the intensity coming from behind it.

**Volume rendering equation (discretized, emission–absorption):** intensity reaching the camera is the sum of sample emissions attenuated by the opacity of all samples in front:

I = Σᵢ (cᵢ αᵢ) · Πⱼ<ᵢ (1 − αⱼ)   (i indexed from the camera; cᵢαᵢ = pre-multiplied emitted color)

**Compositing schemes (skládání vzorků):** (Levoy 1988; "over" operator)

- **Back-to-front:** iterate from the far end: C ← cᵢαᵢ + (1 − αᵢ)·C. One equation per sample; no early termination.
- **Front-to-back:** accumulate color and opacity: C ← C + (1 − A)·cᵢαᵢ; A ← A + (1 − A)·αᵢ. Two equations per sample, but allows **early ray termination** when A ≈ 1 (opaque) → big speedup.

**Quality factors:** interpolation order (NN vs trilinear), **step size** (too large → banding/slicing artifacts), **shading** (normal = interpolated, normalized gradient; Phong shading per sample improves shape perception of internal structures), **light scattering**: single scattering + shadowing (attenuate external light toward the sample) vs. multiple scattering (full model — expensive).

### 3.3 DVR algorithms

- **2D texture-based:** volume stored as 3 stacks of axis-aligned slices (xy, xz, yz); render slices of the stack most perpendicular to the view direction (chosen by the dominant component of the view vector) as texture-mapped polygons blended back-to-front; needs 3 copies of the data; artifacts when the view is diagonal.
- **3D texture-based:** 3D texture hardware resamples arbitrary view-aligned slicing polygons (polygon–volume clipping) → one copy of data, better quality.
- **GPU ray marching:** render front faces of the data bounding box → ray start coordinates per pixel; back faces → ray ends; fragment shader loops along each ray doing the front-to-back integration. Fully parallel, the standard today.

---

## 4. Vector fields: glyphs, stream objects, LIC

**Vector field (vektorové pole)** = spatial field with vector attributes in nodes (direction + magnitude); describes movement (flow) or a force field at one time point. Obtained e.g. by solving **Navier–Stokes** equations (CFD), meteorology, electromagnetics. Enrichment: per-component (bi/tri)linear interpolation.

### Derived properties (charakteristiky pole)

- **Divergence (divergence)** — scalar: div v = ∇·v = ∂vₓ/∂x + ∂v_y/∂y (+ ∂v_z/∂z).
  div > 0 → **source (zdroj)**, div < 0 → **sink (propad)**: how much the flow expands at the point.
- **Vorticity / curl (vířivost, rotace)** — vector: rot v = ∇×v; in 2D the scalar ∂v_y/∂x − ∂vₓ/∂y (z-component).
  Zero ≈ **laminar** flow, high magnitude ≈ **turbulent** flow. Vorticity vector ⊥ flow vector; cross product orientation distinguishes clockwise/counter-clockwise rotation.
- **Helmholtz decomposition:** field = vorticity-free (divergence) component + divergence-free (vorticity) component.

Typical tasks: identify properties at a point / regions with given properties (speed, turbulence); identify the **path of the flow** from/to a point; properties along a path.

### 4.1 Glyphs (glyfy)

One glyph per sample point; encode direction → orientation, magnitude → length (or color).

- Basic glyphs: **line (hedgehog)**, **arrow**.
- Problems & fixes:
  - line points both ways → ambiguity (use arrows / 3D shaded glyphs — cones, arrows; shading improves 3D orientation perception);
  - **overlap** when length > sample distance → fixed length, magnitude → color;
  - dense grids → **subsample** (every k-th vertex); 3D **occlusion** → subsampling, semi-transparency, glyphs only on an extracted isosurface;
  - regular-grid sampling of a rotated grid → artifacts; random sampling helps;
  - **sparse visualization**: humans cannot visually interpolate arrows (unlike interpolated color in scalar plots) → imprecise for path-tracing tasks.

### 4.2 Stream objects (proudové objekty)

Treat the field as a flow; seed massless particles and trace trajectories:

- **Streamline (proudnice)** — trajectory in a *stationary* field; curve tangent to v everywhere.
- **Pathline (trajektorie částice)** — trajectory of a particle in a *time-varying* field.
- **Streakline** — locus of all particles released continuously from a fixed position (dye in flow), time-varying field.
  (In a stationary field all three coincide.)

**Tracing = numerical integration** of dx/dt = v(x), x(0) = x₀:

- **Euler:** x(t+dt) = x(t) + v(x(t))·dt — fast, 1st-order accurate, drifts on curved flow.
- **2nd-order Runge–Kutta (midpoint):** evaluate v at the midpoint x + v(x)·dt/2, then step — much more accurate.
- v at arbitrary positions by (bi/tri)linear interpolation; need point-in-cell location (easy on structured grids).

**Stream ribbon (proudová stuha):** trace a streamline + a constant-size normal vector with it; the **twist of the ribbon visualizes vorticity** along the path (or color-map vorticity onto the line).

**Seeding strategies:** single seeds give no context → fill space: regular/random seeds with early termination when a new line gets too close to an existing one (discard too-short lines); **farthest point streamline seeding (FPSS)** — iteratively place each new seed at the point farthest from existing streamlines (and the domain border) → uniform, dense coverage.

Stream objects beat glyphs for path-related tasks; properties along the path are color-mapped onto the line.

### 4.3 Texture-based (dense) methods

Dense visualization analog of interpolated scalar plots; for 2D fields or 3D surfaces.

- **Vector color coding:** per-pixel: direction (cyclic attribute) → hue on the color wheel, magnitude (sequential) → value/lightness (saturation = 1). Dense but unintuitive, needs training. (Slides note: should use HCL, not HSV — HSV is not perceptually uniform.)
- **Line integral convolution (LIC)** (Cabral & Leedom 1993): input = white-noise texture T. For every pixel, trace a short streamline forward and backward; output intensity = convolution (average) of T along that streamline segment. → values **correlate along the flow**, do not correlate across it: dense fingerprint-like image of the flow structure. Direction sign and magnitude are lost (can be added by color); used also on 3D surfaces (e.g. airflow over a car body).

---

## 5. Abstract data and tabular (n-dimensional) data

### Characteristics of abstract data (abstraktní data)

- No spatial coordinates of measurement → **no data enrichment** (interpolation is meaningless).
- Therefore **position is free** → we *map data on position* (the most accurate channel) — the defining freedom of information visualization (cf. spatial data where position is taken, §1).
- Tabular: item = row = point in n-dimensional space (n = number of attributes); "n-dimensional / multi-dimensional data". Examples: questionnaires, car properties, sensor measurements.

Tasks: identify value/distribution of one attribute; identify items by value/range; identify relations between many attributes (correlation, dependency, **clusters**, outliers) — harder as n grows.

Axis layouts for position encoding: orthogonal (Cartesian), non-orthogonal (basis vectors not independent — projections), **parallel axes**.

### Methods for continuous attributes

- **Scatterplot** (2 attributes; +3D variant — needs rotation/stereo; +color/shape → ~5 attributes, doesn't scale further).
- **Faceting → scatterplot matrix (SPLOM):** scatterplot for every attribute pair; pairwise clusters/correlations easy; >2-attribute patterns hard; screen space shrinks with n². Interaction: **brushing & linking** — select region in one plot, highlight the same items everywhere; multiple colored brushes.
- **Parallel coordinates (paralelní souřadnice)** (Inselberg): n parallel axes; an n-D point = polyline crossing each axis at its value (point ↔ line duality; a 2D line maps to a set of polylines crossing at one point). Correlation visible **only between adjacent axes**: positive linear → ≈parallel segments, negative linear → X-crossing in between; axis reordering needed for other pairs; mapping one attribute to line color helps non-adjacent pairs. Scaling problems: many attributes → axes don't fit; many items → overplotting → transparency, **hierarchical parallel coordinates** (Fua et al. 1999: cluster the points, draw cluster bands at a chosen level of the cluster hierarchy; loses outliers), outlier-preserving focus+context (Novotný & Hauser 2006). Brushing by intervals on one/more axes.
- **Glyphs per item** (only for few items): **star glyph** (attributes at equal angles around circle, closed polyline; invert "less-is-better" attributes so bigger = better; can be ordered, or placed in a 2D scatterplot by two attributes), **star plot** (circular parallel coordinates, more items in one star), **Chernoff faces** (attributes → facial features; exploits human face perception; map good→happy), **stick figures** (attributes → limb angles/lengths; e.g. census data).
- **Binning:** divide each attribute range into bins → n-D hypercubes; recursively divide screen space axis-by-axis so every n-D bin has a unique 2D area; color = item count. Scales well with n and item count; patterns = clusters/trends but harder to interpret.
- **Dimensional projections:** project n-D points to 2D with dependent basis vectors — **star coordinates** (each dimension = unit vector from origin; point = sum of scaled vectors); deforms space, clusters may overlap or be artifacts. Cf. PCA and t-SNE in §7 (Document collections) and SAN (`../BE4M36SAN_statistical_analysis/summary.md`).

### Methods for nominal/ordinal attributes

Scatterplots/parallel coordinates fail (many items collapse onto one dot/line — can't see counts or patterns). Principle: **aggregation** — count items per category, visualize proportions and relations (e.g. Titanic: class × sex × age × survived).

- **Bargrams / icicle plot:** category proportion → length of a segment per attribute row; no relations between attributes visible.
- **Parallel sets:** bargrams connected by **parallelograms** whose width = item count; subdivided top-to-bottom into a tree → relations visible; reorder categories/axes interactively (Titanic: "women and children first — except 3rd class"). Problem: combinatorial explosion of parallelograms → visual clutter; **bundled layout** divides only by adjacent bargram (low clutter, multi-attribute relations harder to follow).
- **Mosaic plot:** nested division of a square — divide horizontally in proportion to marginal totals of X₁, each rectangle vertically by conditional frequencies of X₂, etc.; area ∝ item count. Harder proportion comparison; only hierarchical relations; faceting → mosaic plot matrix.

---

## 6. Relational data: hierarchies, DAGs, undirected graphs

### Characteristics

Tabular data + **links** between items (in one or more tables). Relation L ⊆ X₁ × … × X_k (subset of cartesian product); mostly **binary relations** (1:M or N:M). Representations: link table (FKs), **adjacency matrix**, **adjacency list**. Both nodes and links can carry attributes (typically few). Abstract data → no enrichment (nodes may have coordinates — cities — but interpolating between flights is meaningless).

Special structures: **hierarchy/tree** (1:M; file system), **DAG** (courses & prerequisites), **directed network** (WWW), **undirected network** (friendships).

### Visual encoding of relations

| Channel | Works for | Notes |
|---|---|---|
| **Containment (ohraničení/vnoření)** | 1:M, hierarchies | treemaps; not scalable for N:M |
| **Connection (spojnice)** | 1:M and N:M | node-link diagrams |
| **Proximity (blízkost)** | 1:M grouping | cannot express hierarchy |
| **Similarity (podobnost, color hue)** | 1:M grouping | not scalable, no hierarchy |

Node attributes → shape, color, size; link attributes → width, color of connections.

Tasks: values of attributes of items/links; items incident to a link & vice versa; degree queries; **paths** between items; **components**; **levels** of hierarchy.

### Hierarchies with containment: treemap

- Space-filling division of a rectangle; **one quantitative attribute → area** (inner node value = sum of its leaves); alternate dividing axis per level (slice-and-dice), recurse depth-first.
- Gives overview of the whole hierarchy; great for "which directories are large"; other attributes → color.
- Problem: many thin rectangles → fix by **squarified-like division**: split children into two groups of similar total value, divide rectangle in two, recurse (more square-ish cells). Examples: disk usage, S&P 500 market map (finviz).

### Hierarchies & networks as graphs (node-link)

**Layouts:** tree layout, hierarchical (layered) layout, orthogonal layout, force-directed layout; edge styles: straight, polyline (control polygons for curves), orthogonal. Tree/hierarchical layouts communicate hierarchy far better than force-directed.

Tree layout variants: **hierarchical tree** (wide+tall → scroll/zoom), **radial tree** (levels on concentric circles; limited number of levels; re-rootable), **balloon tree** (subtree near its root), **hyperbolic tree** (focus+context in hyperbolic space), **cone tree** (3D balloon; many children).

**Aesthetic criteria (estetická kritéria)** — often contradictory; layout = multi-criteria optimization, mostly NP-hard → heuristics, local minima:
minimize edge **crossings** (ideal: planar drawing); minimize **area**; **aspect ratio** ≈ 1; maximize **angular resolution** (smallest angle at a vertex); **edge length** (total/max/variance); **bends** (total/max/variance; orthogonal drawings); display **symmetry**.

**Sugiyama framework** (layered layout for trees/DAGs):

1. Remove cycles (temporarily reverse one edge per cycle);
2. assign nodes to **layers** by topological order (child below parent);
3. break long edges spanning >1 layer with **virtual nodes**;
4. (optional) aggregate virtual nodes;
5. reduce edge crossings by reordering nodes within layers (optimization);
6. assign coordinates; remove virtual nodes, restore reversed edges.

**Force-directed methods (silové metody):** physical analogy; model (forces between nodes/edges) + algorithm (find equilibrium / minimal energy ⇒ Σ forces = 0; usually local minimum).

- **Eades 1984:** springs (logarithmic) on edges + repulsion between non-adjacent nodes.
- **Fruchterman–Reingold 1991:** attraction f_a ∝ d²/k on edges, repulsion f_r ∝ −k²/d all pairs; **temperature/cooling** limits node displacement per iteration; stop at temperature/force threshold or max iterations.
- **Kamada–Kawai 1989:** springs between *all* pairs, ideal length ∝ graph-theoretic distance; iteratively move the node with max potential energy to equilibrium by **Newton–Raphson** (1st derivative = force = direction, 2nd derivative = step size).
- Problems: ignore crossings, **hairball** for dense graphs, local minima.

**Matrix-based visualization:** show the adjacency matrix itself (nodes = rows/columns, edges = cells). Scales much better than node-link (no crossings/occlusion); **row/column reordering** reveals clusters (e.g. co-authorship blocks; NodeTrix combines matrix sub-blocks with links). Paths are hard to follow.

---

## 7. Text visualization

### Characteristics

Text is hard to visualize: very high dimensionality (10⁴–10⁵ features), compositional, abstract (no inherent ordering), subtle (small differences matter), **not pre-attentive** (reading requires focus). Distinguish: **single document**, **document collection (corpus)**, **streams** (Twitter…); plus source code.

**Text analysis levels:** **lexical** (characters → tokens: words, stems via stemming/lemmatization, n-grams, phrases) → **syntactic** (tagging token function: POS, singular/plural, named-entity recognition) → **semantic** (meaning & relationships, sentiment). Hard: ambiguity, synonymy, context dependence.

### 7.1 Single document

Representation: **bag-of-words / vector space model** — document → numeric vector of (weighted) term frequencies; forgets linguistic structure but enables linear algebra (also used for classification/clustering).

Tasks: **overview** of content; **comparison** of documents; **evolution** of content through the document / over time.

- **Word/tag cloud:** word frequency → font size; limit word count; **Wordle**: randomized greedy placement. Extensions: **FacetClouds** (extra attributes → color/orientation; color most effective), **SparkClouds** (tag cloud + sparklines for trends over time), TempoTaggram.
- Linguistic structure visualizations: **arc diagram** (repeated subsequences joined by semicircular arcs; thickness = subsequence length, height = distance), **word tree** (keyword-in-context as a branching tree of following words; subtree size = frequency), **phrase nets** (graph of words connected by user-specified relations, e.g. "X of Y"), **DocuBurst** (radial/sunburst of WordNet hyponym hierarchy, wedge size = occurrence count).

### 7.2 Document collection (corpus)

Tasks: overview of collection content; **grouping** (which documents talk about X); **comparison/similarity** (similar topics? similar documents to those I have?).

Representation: **document–term matrix** (documents × keywords; binary / counts / weights).

**TF-IDF weighting:**
tfidf(w, d) = tf(w, d) · log(N / df(w)),
tf = term frequency in document d, df = number of documents containing w, N = number of documents. A word is important if frequent in the document and rare in the collection.

**Cosine similarity** between document vectors d₁, d₂:
sim = (d₁·d₂)/(‖d₁‖‖d₂‖) ∈ [0,1] (1 = identical direction); cheap — sum over intersecting words.

- **Document atlas / landscape (Themescapes):** project document vectors to 2D by dimensionality reduction (PCA, t-SNE, MDS); similar documents close together; density → terrain/height metaphor (patent landscapes).
- **Dimension reduction** (cross-ref SAN, SSU):
  - **PCA** — orthogonal directions of maximal variance (eigenvectors of covariance matrix; eigenvalues = variance; pick top 2–3; linear).
  - **t-SNE** (van der Maaten & Hinton 2008) — nonlinear, force-directed-like; preserves *neighborhoods* via probabilistic distances (Gaussian in high-D, **Student-t** in low-D to fight crowding); key parameter **perplexity** ≈ effective number of neighbors (recommended 5–50); caveats: cluster **sizes and inter-cluster distances mean nothing**, results vary with random init/perplexity.
- Word-usage over time (State-of-the-Union speeches), average sentence length, etc.
- Metadata: **KeyVis** — stacked bar charts and **keyword co-occurrence graphs** of paper keywords.
- Streams over time: **ThemeRiver** / TIARA — see §9.

### 7.3 Software visualization

Code = structured text in a collection of files; syntax/semantics machine-checkable; relations extractable (calls, uses) → relational data. Taxonomy: program visualization (structure — UML class/component/package diagrams; behavior — sequence/activity diagrams), data visualization, algorithm visualization, visual programming. **SeeSoft** (Eick): one file = column, one code line = one pixel-line; color = age/author/modification request/execution hot spots; version history and diffs at scale.

---

## 8. Geographical data (supporting topic)

Geometry (points of interest, polylines — streets/rivers, areas — districts) + attached abstract data (names, population). Queries combine geometry + attributes. Raster vs. vector representation; map **projections** deform sizes/shapes (choose by task); **choropleth maps** (color-coded areas — normalize by area/population!), cartograms, dot/symbol maps. (Not in the official topic list, but provides exam-friendly examples of mixing spatial + abstract data.)

---

## 9. Time-oriented data (časově orientovaná data)

**Definition (Aigner et al.):** data where changes over time / temporal aspects play the central role. Time ≠ space: unidirectional, gives ordering to events, no human sense for it.

Design driven by: **(What?)** characteristics of time + data; **(Why?)** tasks; **(How?)** visual representation.

### 9.1 Time primitives (časová primitiva) and relations

- **Instant (časový okamžik)** — anchored point in time ("28. 5. 23:59").
- **Interval (časový interval)** — anchored portion of time with start & end ("lecture 7. 5. 11:00–12:30").
- **Span (časové rozpětí)** — *unanchored* duration ("2 hours").

**Relations between primitives** = their relative locations (topology). For anchored primitives the relations follow from their position in time:

- **instants:** before, equal, after;
- **intervals: Allen's 13 relations** (Allen 1983): before, meets, overlaps, starts, during, finishes, equals + the 6 inverses (after, met-by, overlapped-by, started-by, contains, finished-by).

### 9.2 Time domain (časová doména): type, granularity, structure

**Type (typ):**

- **ordinal** — only relative order ("after lunch");
- **discrete** — time as integers at some granularity (digital clock; nearest-neighbor interpolation);
- **continuous** — time as reals; between any two distinct instants another exists (linear interpolation).

**Granularity (granularita):** different attributes measured at different granularities (stock price intraday vs. mutual fund daily; age in months then years); granularity can change over time. Conversions: to coarser = truncation (15. 5. 2025 11:20 → 15. 5. 2025); to finer = **uncertainty** → represent a day as the interval 0:00:00–23:59:59 (uncertainty interval method). Some quanta don't nest (weeks × years/months).

**Structure (struktura):**

- **linear (lineární)** — time as a line; good for **trends**;
- **cyclic (cyklická)** — time wraps (spiral); good for **periodic patterns** (cycle length often unknown!);
- **branching (větvící se)** — multiple futures; planning/scenarios.

### 9.3 Tasks

Related to time primitives (MacEachren): **existence** (was a measurement made in July?), **temporal location** (when?), **interval/duration** (how long?), **sequence** (what order?), **synchronization** (do they coincide?). Related to attribute values: **identify / compare** {values, trends} × {one item at different times, different items at the same time}.

### 9.4 Visual mapping of time

- **Dynamic — time → time (animation):** natural; suits 2D/3D fields and flow; **bad for analysis** — no direct comparison between time points (memory needed); must be user-controlled (slider).
- **Static — time → space:** map time to visual channels → direct comparison. Channels: **position** (most common & most accurate: time axis), **length** (duration), angle/slope (clock metaphor), connection (before→after arrows), line thickness, color/brightness ("fading away" of the past).
- **Faceting / small multiples / trellis:** same encoding in every view, data partitioned **by time** (compare seasons/years) or **by another attribute** (compare industries over time).

### 9.5 Example techniques

| Technique | Time primitive / data | Idea |
|---|---|---|
| **Time-series plot** | instants, abstract | the classic line chart (most used graphic ever, Tufte) |
| **Spiral graph** | instants, cyclic | time along a spiral, value → color; reveals periodicity |
| **Cycle plot** (Cleveland) | instants, cyclic+trend | per-cycle-position subplots (e.g. one mini-trend per weekday) + mean line: seasonality and trend separated |
| **ThemeRiver** | text stream | themes = colored currents of a river flowing left→right; width = theme strength at that time |
| **TIARA** | text stream | ThemeRiver + keywords inside the layers |
| **Gapminder** | instants, abstract | animated scatterplot (Rosling); animation controlled by user, traces possible |
| **GeoTime** | instants, spatial | 2D map + time as 3rd (vertical) axis |
| **Lexis pencil / helix icons** | instants, spatial | time along a pencil/helix at a location; helix emphasizes cycles |
| **TimeNet / family tree** | intervals, abstract | lives = horizontal lines, marriages = connections |
| **Gantt chart** | intervals, abstract | tasks as bars on a time axis (project planning) |

Note: **intervals and spans are used only for abstract data** (lecture 12 summary).

---

## 10. Cross-references

- **PCA, t-SNE** — derivations in SAN (`../BE4M36SAN_statistical_analysis/summary.md`); here only their role as projection to 2D for document landscapes / tabular data.
- **Color perception, transfer functions** — related to DPG/computer graphics topics (B-rep vs. volumetric representation also appeared in a 2012 common-part exam question, see `questions.md` Q3).
- **Graphs (adjacency matrix/list)** — same structures in PAL and DS2 (graph databases, data locality).
- **Hierarchical clustering** (hierarchical parallel coordinates) — clustering theory in SAN.
- **Bag-of-words, TF-IDF, cosine similarity** — also relevant for SMU/SSU text-classification examples.

## References

1. Lecture slides 01–13, BE4M39VIZ, Čmolík & Slavík, CTU FEL (in `materials/`).
2. T. Munzner: *Visualization Analysis and Design*, CRC Press, 2014 — task abstraction, channels, effectiveness ranking.
3. A. C. Telea: *Data Visualization: Principles and Practice*, 2nd ed., CRC Press, 2014 — scalar/vector/volume visualization.
4. W. E. Lorensen, H. E. Cline: *Marching Cubes: A High Resolution 3D Surface Construction Algorithm*, SIGGRAPH 1987.
5. G. M. Nielson, B. Hamann: *The Asymptotic Decider: Resolving the Ambiguity in Marching Cubes*, IEEE Visualization 1991.
6. M. Levoy: *Display of Surfaces from Volume Data*, IEEE CG&A, 1988 — DVR compositing.
7. B. Cabral, L. C. Leedom: *Imaging Vector Fields Using Line Integral Convolution*, SIGGRAPH 1993.
8. P. Eades (1984); T. Fruchterman & E. Reingold (1991); T. Kamada & S. Kawai (1989) — force-directed layouts.
9. K. Sugiyama et al.: *Methods for Visual Understanding of Hierarchical System Structures*, IEEE SMC, 1981.
10. L. van der Maaten, G. Hinton: *Visualizing Data using t-SNE*, JMLR 9, 2008.
11. J. F. Allen: *Maintaining Knowledge about Temporal Intervals*, CACM 26(11), 1983.
12. W. Aigner, S. Miksch, H. Schumann, C. Tominski: *Visualization of Time-Oriented Data*, Springer, 2011.
13. Course page (Moodle FEL): https://moodle.fel.cvut.cz/local/kos/pages/course/info.php?id=1183 — lecturer L. Čmolík, Dept. of Computer Graphics and Interaction.
