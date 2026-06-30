# OpenUSD Development Certification — Additional Practice Questions

A supplementary, **domain-by-domain** question bank to extend the 13 official samples in [`PRACTICE_QUESTIONS.md`](./PRACTICE_QUESTIONS.md). These **35 questions** are organized by the 8 exam domains in [`EXAM_PREP.md`](./EXAM_PREP.md) and weighted roughly to the exam (more questions where more points live).

> ⚠️ **Not official.** Unlike `PRACTICE_QUESTIONS.md` (which reproduces the study guide's own samples), these questions are **authored for this repo**. Every one is grounded in the [Learn OpenUSD](https://docs.nvidia.com/learn-openusd/latest/index.html) course content — the answer key cites the specific lesson each question is built from (the local `docs/…` paths are the source of those published pages). Use them to drill; treat the official 13 as the calibration of real exam difficulty/phrasing.

**How to use:** Work one domain at a time right after you study it. Answer without scrolling to the [Answer Key](#answer-key). Watch the **"Select N options"** prompts — several are multi-select.

| Domain | Weight | Questions |
|---|---|---|
| 1 — Composition | 23% | Q1–Q6 |
| 2 — Data Exchange | 15% | Q7–Q11 |
| 3 — Pipeline Development | 14% | Q12–Q15 |
| 4 — Data Modeling | 13% | Q16–Q20 |
| 5 — Debugging & Troubleshooting | 11% | Q21–Q24 |
| 6 — Content Aggregation | 10% | Q25–Q28 |
| 7 — Visualization | 8% | Q29–Q32 |
| 8 — Customizing USD | 6% | Q33–Q35 |

---

## Domain 1 — Composition (23%)

### Question 1 — Composition

In USD's **LIVRPS** strength ordering, which composition arc provides the **weakest** opinions (used as fallbacks)?

- **A.** Local
- **B.** Inherits
- **C.** References
- **D.** Specializes

---

### Question 2 — Composition

You need **two independent copies** of the asset `skyscraperA.usd` standing side by side in one scene. Which approach achieves this, and why?

- **A.** Add `skyscraperA.usd` twice as **sublayers** — each include adds another copy to the stage.
- **B.** Add `skyscraperA.usd` as a **reference** on two different prims — referencing *grafts* the hierarchy onto each prim, creating two instances.
- **C.** Add it once as a sublayer — sublayers automatically duplicate included content per use.
- **D.** Use a **specializes** arc so the second copy falls back to the first.

---

### Question 3 — Composition

Given the layers below, what is the composed value of `radius` on `/Ball` when the stage's root layer is `scene.usda`?

`chassis.usda`
```usda
#usda 1.0
(
    defaultPrim = "Ball"
)
def Sphere "Ball" { double radius = 1 }
```

`paint.usda`
```usda
#usda 1.0
(
    defaultPrim = "Ball"
)
over "Ball" { double radius = 2 }
```

`scene.usda`
```usda
#usda 1.0
def Sphere "Ball" (
    prepend references = [@./chassis.usda@, @./paint.usda@]
) {
    double radius = 5
}
```

- **A.** 1
- **B.** 2
- **C.** 5
- **D.** 8

---

### Question 4 — Composition

You have 50 prims that should **all update automatically** whenever you change a single shared source prim's opinion (e.g. a shared look). Which composition arc is designed for this "broadcast" behavior?

- **A.** **References** — edits to the source propagate to every referencing prim.
- **B.** **Inherits** — opinions authored on the source prim broadcast to all prims that inherit it.
- **C.** **Payloads** — loading the payload updates all copies at once.
- **D.** **Sublayers** — including the source updates all copies.

---

### Question 5 — Composition

Which statements about the `defaultPrim` layer metadata are correct? *(Select two options.)*

- **A.** It names the prim used as the root when the layer is referenced or payloaded **without an explicit target path**.
- **B.** If it is **not set**, referencing the layer without a target prim resolves to an empty layer and logs a warning.
- **C.** It sets the default material applied to geometry in the layer.
- **D.** It must be an `Xform`; a `Scope` cannot be a default prim.

---

### Question 6 — Composition

Which specifier marks a prim as an **abstract blueprint** — meant to be the target of a reference/inherit/specialize arc, and **not visited by default stage traversals**?

- **A.** `def`
- **B.** `over`
- **C.** `class`
- **D.** `payload`

---

## Domain 2 — Data Exchange (15%)

### Question 7 — Data Exchange

From the perspective of a Digital Content Creation (DCC) application, an **exporter** translates data:

- **A.** from the DCC's runtime format **into** USD.
- **B.** from USD **into** the DCC's runtime format.
- **C.** between two non-USD formats without ever touching USD.
- **D.** only between layers already inside a USD stage.

---

### Question 8 — Data Exchange

Learn OpenUSD recommends an ETL-inspired **two-phase** approach (extract, then transform) for data exchange. Which statements are correct? *(Select two options.)*

- **A.** The **extract** phase should translate source data to USD as directly as possible, to preserve fidelity to the source.
- **B.** The **transform** phase tailors the USD data to a specific consumer (export options, restructuring, optimizations like mesh merging).
- **C.** Extraction code is generic and reusable across many source formats, while transform code is source-specific.
- **D.** Transformations should be **maximized** so the data is fully normalized before export.

---

### Question 9 — Data Exchange

During debugging you need a layer you can **read and diff in a text/code review**. Which file encoding fits, and what is the trade-off?

- **A.** `.usdc` (Crate) — it is binary but human-readable.
- **B.** `.usda` — human-readable text, ideal for inspection and diffing, at the cost of larger size and slower I/O than Crate.
- **C.** `.usdz` — a compressed archive optimized for in-place editing.
- **D.** `.usd` — guaranteed to be binary.

---

### Question 10 — Data Exchange

A DCC exports an asset and a downstream tool rejects it. Which USD tool validates a stage or USDZ package against interchange and Hydra-renderability **rules** (e.g. flagging a missing `defaultPrim`, `upAxis`, or `metersPerUnit`)?

- **A.** `usdcat`
- **B.** `usdchecker`
- **C.** `usdzip`
- **D.** `usdedit`

---

### Question 11 — Data Exchange

An OBJ file stores **one global vertex list** with per-face indices. When extracting it into a `UsdGeomMesh`, which data must you author?

- **A.** `points`, plus `faceVertexCounts` and a single **flattened** `faceVertexIndices` array covering all faces.
- **B.** Only `points`; USD infers the faces automatically.
- **C.** One `Mesh` prim per face.
- **D.** A relationship from each face to its vertices.

---

## Domain 3 — Pipeline Development (14%)

### Question 12 — Pipeline Development

What characterizes a well-designed **asset interface**? *(Select two options.)*

- **A.** A single, **stable** entry-point prim (commonly the `defaultPrim`) that downstream consumers reference and override.
- **B.** **Encapsulation** — all of the asset's content, *including relationship targets* (e.g. bound materials), descends from the entry-point prim.
- **C.** Internal implementation prims are exposed at the top level so consumers can edit them directly.
- **D.** Entry-point prim paths change frequently to mirror internal refactors.

---

### Question 13 — Pipeline Development

In the **reference/payload pattern**, fields such as variant sets and `extentsHint` are "**lofted**" up onto the interface layer (above the payload). Why?

- **A.** So they remain **discoverable and usable even when the payload is unloaded**.
- **B.** Because USD forbids variant sets from existing inside a payload.
- **C.** To force the payload to always load on stage open.
- **D.** Because lofting compresses the payload contents.

---

### Question 14 — Pipeline Development

You need organizational grouping prims (e.g. `Geometry`, `Materials`) that **carry no transform**. Which prim type should you use, and why?

- **A.** `Xform` — every container in the hierarchy should be transformable.
- **B.** `Scope` — it is a **non-transformable** organizational grouping prim, which avoids ambiguous transform semantics.
- **C.** `Mesh` — containers must themselves be renderable.
- **D.** `Material` — grouping is a shading concern.

---

### Question 15 — Pipeline Development

Which practices best support a **scalable, multi-user** USD pipeline? *(Select two options.)*

- **A.** Split work into separate **sparse layers per workstream** (e.g. modeling vs. surfacing) so artists iterate in parallel without blocking each other.
- **B.** Establish consistent **naming conventions and directory structures** so assets are legible and navigable.
- **C.** Require every workstream to re-author the *entire* asset rather than contribute sparse overrides.
- **D.** Grow the layer stack procedurally without bound, using it as a substitute for version control.

---

## Domain 4 — Data Modeling (13%)

### Question 16 — Data Modeling

Which statements correctly distinguish **attributes** from **relationships**? *(Select two options.)*

- **A.** An attribute has exactly one value type; a relationship has **no** value type.
- **B.** A relationship can target multiple prims, whereas an attribute cannot point to a prim.
- **C.** Relationships can carry **time samples** just like attributes.
- **D.** Attributes cannot have default values.

---

### Question 17 — Data Modeling

A layer sets `timeCodesPerSecond = 24`. A value authored at **time code 48** corresponds to what wall-clock time?

- **A.** 48 seconds
- **B.** 2 seconds
- **C.** 24 seconds
- **D.** 0.5 seconds

---

### Question 18 — Data Modeling

A USD file authors geometry but **does not author** `metersPerUnit`. What linear scale will consumers assume for that file?

- **A.** `1.0` (meters)
- **B.** `0.01` (centimeters) — the fallback value
- **C.** The file is invalid until `metersPerUnit` is authored.
- **D.** It is auto-detected from the geometry's bounding box.

---

### Question 19 — Data Modeling

You author **one color value per face** of a mesh as a `primvars:displayColor` primvar. Which **interpolation** token describes this?

- **A.** `constant`
- **B.** `uniform`
- **C.** `vertex`
- **D.** `faceVarying`

---

### Question 20 — Data Modeling

A stage has animation authored as time samples, but `attr.Get()` (no argument) returns a single static value instead of the animated one. What is happening, and how do you fix it?

- **A.** `Get()` with no time code uses `Usd.TimeCode.Default()` (the non-time-varying default); pass a specific time code, or `Usd.TimeCode.EarliestTime()`, to read animated values.
- **B.** `Get()` always returns the last time sample; there is nothing to fix.
- **C.** Time samples only resolve through a relationship; add one.
- **D.** You must flatten the stage before any animated value resolves.

---

## Domain 5 — Debugging & Troubleshooting (11%)

### Question 21 — Debugging and Troubleshooting

An attribute opinion you authored isn't winning on the composed stage. What is the most direct way to diagnose **which layer's opinion is actually winning**?

- **A.** Inspect the prim's **PrimStack / property stack** (ordered strong → weak) to see the source layers and their precedence.
- **B.** Reload the stage to clear caches.
- **C.** Delete the stronger layer and see if the value changes.
- **D.** Re-encode the layer from `.usda` to `.usdc`.

---

### Question 22 — Debugging and Troubleshooting

Which expression correctly builds a traversal predicate that visits prims that are **both active and loaded**?

- **A.** `Usd.PrimRange(prim, predicate=Usd.PrimIsActive and Usd.PrimIsLoaded)`
- **B.** `Usd.PrimRange(prim, predicate=Usd.PrimIsActive & Usd.PrimIsLoaded)`
- **C.** `Usd.PrimRange(prim, predicate=Usd.PrimIsActive or Usd.PrimIsLoaded)`
- **D.** `stage.Traverse(Usd.PrimIsActive and Usd.PrimIsLoaded)`

---

### Question 23 — Debugging and Troubleshooting

What is the effect of calling `prim.SetActive(False)`?

- **A.** The prim **and all of its descendants' scene description** are excluded from composition and default traversal (a non-destructive prune); a stronger layer can override `active` back to `true`.
- **B.** The prim is permanently deleted from every layer on disk.
- **C.** Only the prim itself is hidden, but its descendants still compose and render.
- **D.** It is identical to setting the prim's `visibility` to `invisible`.

---

### Question 24 — Debugging and Troubleshooting

On one prim, a **stronger** layer authors `customData = {"b": 2}` and a **weaker** layer authors `customData = {"a": 1}`. What does `prim.GetCustomData()` resolve to?

- **A.** `{"b": 2}` — for dictionary metadata the strongest opinion wins outright.
- **B.** `{"a": 1, "b": 2}` — dictionary metadata is merged **per key**.
- **C.** `{"a": 1}` — the weaker, more specific key wins.
- **D.** An error, because the two layers conflict.

---

## Domain 6 — Content Aggregation (10%)

### Question 25 — Content Aggregation

For the **model hierarchy** to be valid, all **ancestors** of a `component` model must have their `kind` set to:

- **A.** `component`
- **B.** `group`, or a subkind of group such as `assembly`
- **C.** `subcomponent`
- **D.** any kind — there is no ancestor rule

---

### Question 26 — Content Aggregation

You set `instanceable = true` on a prim, but no prototypes are created and nothing is deduplicated. What is the most likely cause?

- **A.** The prim has **no composition arc** (e.g. reference or payload) for USD to build a prototype from — instancing is driven by composition.
- **B.** `instanceable` must be set to the string `"on"`, not `true`.
- **C.** Prototypes are only generated for `Mesh` prims.
- **D.** You must call `Stage.Flatten()` before prototypes form.

---

### Question 27 — Content Aggregation

In **scenegraph (native) instancing**, which scene description can you actually edit?

- **A.** The descendants of the instance (the **instance proxy**) — local overrides are applied.
- **B.** The shared **prototype** prims directly (e.g. `/__Prototype_1`).
- **C.** The **instanceable prim's own root** opinions (e.g. its transform or visibility); the instance proxy and prototype are read-only.
- **D.** Nothing — an instanced prim is completely immutable.

---

### Question 28 — Content Aggregation

Which statements about `UsdGeomPointInstancer` are correct? *(Select two options.)*

- **A.** It requires a `prototypes` relationship, a `protoIndices` array, and a `positions` array.
- **B.** To animate a single instance **on/off over time**, author its id into the `invisibleIds` attribute.
- **C.** You can sparsely override just one element of the `positions` array.
- **D.** `protoIndices` stores the world-space position of each instance.

---

## Domain 7 — Visualization (8%)

### Question 29 — Visualization

`UsdGeomImageable` is the base class for renderable prims. Which pair of properties does it contribute to **every** geometry prim?

- **A.** `radius` and `height`
- **B.** `visibility` and `purpose`
- **C.** `points` and `faceVertexIndices`
- **D.** `diffuseColor` and `roughness`

---

### Question 30 — Visualization

Why would you group your materials under a `Scope` rather than an `Xform`?

- **A.** A `Scope` is renderable while an `Xform` is not.
- **B.** A `Scope` is a **non-transformable** organizational prim — it carries no transform semantics, which is appropriate for a pure grouping container.
- **C.** A `Scope` automatically binds the materials it contains.
- **D.** An `Xform` cannot have child prims.

---

### Question 31 — Visualization

How is a material bound to a mesh in USD?

- **A.** Through an attribute named `material:binding` that stores the material's color.
- **B.** Through a **relationship** named `material:binding` (authored via `UsdShade.MaterialBindingAPI`) that targets a `UsdShade.Material`.
- **C.** Through a primvar named `material`.
- **D.** By nesting the mesh prim inside the `Material` prim.

---

### Question 32 — Visualization

You want a `UsdPreviewSurface` material whose **diffuse color is driven by a per-vertex primvar** on the mesh. Which two pieces are required? *(Select two options.)*

- **A.** A `UsdShade.Shader` with id `"UsdPreviewSurface"` whose `diffuseColor` input is **connected** to an upstream source.
- **B.** A primvar-reader shader (e.g. `UsdPrimvarReader_float3`) that reads the named primvar and feeds the surface shader's `diffuseColor`.
- **C.** Hardcoding the color directly on the surface shader so downstream layers cannot override it.
- **D.** Binding the material to the mesh with an **attribute** rather than a relationship.

---

## Domain 8 — Customizing USD (6%)

### Question 33 — Customizing USD

You want to add **optional physics properties** to existing geometry prims **without changing their prim type**. What kind of schema is this, and what does it derive from?

- **A.** A concrete IsA (typed) schema deriving from `UsdTyped`.
- **B.** An **applied API schema** deriving from `UsdAPISchemaBase`, list-edited into the prim's `apiSchemas` metadata.
- **C.** A new prim type assigned via `typeName`.
- **D.** A relationship-only schema with no base class.

---

### Question 34 — Customizing USD

Which statement about applying schemas to a prim is correct?

- **A.** A prim can have multiple IsA (typed) schemas active at once.
- **B.** A prim has **at most one** IsA schema (its `typeName`) but can have **multiple** applied API schemas.
- **C.** API schemas set the prim's `typeName`.
- **D.** IsA schemas are list-edited in the `apiSchemas` metadata.

---

### Question 35 — Customizing USD

After applying `UsdPhysics.RigidBodyAPI` to a prim and calling `CreateVelocityAttr()`, what is the **authored attribute name**?

- **A.** `velocity`
- **B.** `physics:velocity`
- **C.** `rigidBody:velocity`
- **D.** `RigidBodyAPI:velocity`

---

## Answer Key

| Q | Answer | Domain | Why | Grounded in |
|---|---|---|---|---|
| 1 | **D** | Composition | LIVRPS = **L**ocal, **I**nherits, **V**ariants, **R**eferences, **P**ayloads, **S**pecializes, ordered strongest→weakest. Specializes provides the weakest, fallback opinions. | `composition-basics/strength-ordering.md`, `creating-composition-arcs/strength-ordering/what-is-liverps.md` |
| 2 | **B** | Composition | References *graft* a prim hierarchy onto a target prim, so referencing the same asset on two prims yields two instances. Sublayers *include* ("the first sublayer would overwrite the second") and don't duplicate. | `creating-composition-arcs/references-payloads/references-faq.md`, `.../what-are-references.md` |
| 3 | **C** | Composition | **Local** opinions are the strongest in LIVRPS, so the root layer's local `radius = 5` outranks both references. (Without the local opinion, the *first*-listed reference `chassis.usda` would win, giving `1`.) | `composition-basics/strength-ordering.md` |
| 4 | **B** | Composition | An **inherit** arc broadcasts opinions from one source prim to all prims that inherit it, across layer stacks. References create **isolated** instances — edits to one don't propagate. | `creating-composition-arcs/inherits-specializes/what-is-inherits.md` |
| 5 | **A, B** | Composition | `defaultPrim` names the root entry point used when the layer is referenced/payloaded without a target path; if unset, the reference "will resolve as an empty layer and output a warning." (Any prim type, incl. `Scope`, may be the default prim.) | `composition-basics/default-prim.md` |
| 6 | **C** | Composition | `class` "signals that it is a blueprint…intended as the target of a reference, payload, inherit, or specialize." Class/abstract prims compose but are **not** visited by default traversals. | `composition-basics/specifiers.md` |
| 7 | **A** | Data Exchange | An **exporter** translates from the application's runtime format **into** USD; an **importer** goes the other way (USD → app). | `data-exchange/data-exchange/what-is-data-exchange.md` |
| 8 | **A, B** | Data Exchange | Extract = translate to USD "as directly as possible to maintain fidelity"; transform = optional steps to meet client needs. C is reversed (transform code is reusable, extraction is source-specific); D is wrong — minimize transforms to keep round-trips reversible. | `data-exchange/data-exchange/what-is-data-exchange.md`, `.../data-transformation/what-is-data-transformation.md` |
| 9 | **B** | Data Exchange | `.usda` is human-readable text — best for inspection/diffing/archival legibility — while `.usdc` (Crate) is the binary, I/O-efficient encoding. | `asset-structure/asset-structure-principles/why-necessary.md`, `stage-setting/usd-file-formats.md` |
| 10 | **B** | Data Exchange | `usdchecker` validates a stage/USDZ package against rules for interchange and Hydra renderability, catching missing `upAxis`/`metersPerUnit`/`defaultPrim`. | `data-exchange/asset-validation/what-is-asset-validation.md`, `.../exercise-asset-validation-testing.md` |
| 11 | **A** | Data Exchange | OBJ keeps a global vertex list with per-face indices; USD needs `points` plus `faceVertexCounts` and a single flattened `faceVertexIndices`. (Set `subdivisionScheme = none` for polygonal OBJ meshes.) | `data-exchange/data-extraction/exercise-extracting-geometry.md` |
| 12 | **A, B** | Pipeline Development | A good interface exposes a stable entry-point prim for overrides and **encapsulates** all content (incl. relationship targets) under it. Exposing internals (C) and unstable paths (D) break downstream consumers. | `asset-structure/asset-structure-principles/asset-interface-pt1.md`, `.../encapsulating-your-asset.md` |
| 13 | **A** | Pipeline Development | "Lofting" promotes cheap, important fields above the payload so they stay discoverable/usable **without loading the payload**, improving performance. | `asset-structure/reference-payload-pattern/what-is-ref-payload-pattern.md`, `.../lofting-variant-sets.md` |
| 14 | **B** | Pipeline Development | A `Scope` is a non-transformable grouping prim; using it for organizational containers avoids the ambiguous transform semantics of an `Xform`-of-`Xform`s. | `asset-structure/asset-structure-principles/asset-interface-pt3.md`, `scene-description-blueprints/scope.md` |
| 15 | **A, B** | Pipeline Development | Sparse per-workstream layers enable parallel iteration; naming/structure conventions give legibility and navigability. Re-authoring whole assets (C) and unbounded layer stacks as version control (D) are anti-patterns. | `asset-structure/workstreams/modeling-workstreams.md`, `asset-structure/asset-structure-principles/why-necessary.md` |
| 16 | **A, B** | Data Modeling | An attribute has one value type and can't point to a prim; a relationship has no value type and can target multiple prims. Relationships can't be time-sampled (C); attributes *can* have defaults (D). | `stage-setting/properties/attributes.md`, `.../relationships.md` |
| 17 | **B** | Data Modeling | Time = timeCode ÷ timeCodesPerSecond = 48 ÷ 24 = **2 seconds**. | `stage-setting/timecodes-timesamples.md` |
| 18 | **B** | Data Modeling | When `metersPerUnit` is not authored, the fallback is **`0.01`** (centimeters). (`upAxis` falls back to `"Y"`.) | `beyond-basics/units.md` |
| 19 | **B** | Data Modeling | `uniform` = one value per face. (`constant` = whole prim, `vertex` = per point, `faceVarying` = per face-corner.) | `beyond-basics/primvars.md` |
| 20 | **A** | Data Modeling | A bare `Get()` uses `Usd.TimeCode.Default()`, the non-animated default. Use `Get(<time>)` or `Get(Usd.TimeCode.EarliestTime())` to read animated samples. | `beyond-basics/value-resolution.md` |
| 21 | **A** | Debugging & Troubleshooting | The PrimStack/property stack lists the contributing specs strong→weak, directly showing why an opinion does or doesn't win — the core "why doesn't this opinion take effect" tool. | `composition-basics/specifiers.md` (`GetPrimStack`), `beyond-basics/value-resolution.md` |
| 22 | **B** | Debugging & Troubleshooting | Traversal predicates must be combined with **bitwise** operators (`&`, `|`, `~`); Python's `and`/`or`/`not` (A, C, D) do **not** combine them correctly. | `beyond-basics/stage-traversal.md` |
| 23 | **A** | Debugging & Troubleshooting | Deactivation is a non-destructive prune: the prim and its descendants are excluded from composition/traversal, but a stronger layer can set `active = true` to bring it back. | `beyond-basics/active-inactive-prims.md` |
| 24 | **B** | Debugging & Troubleshooting | Dictionary-valued metadata like `customData` resolves **element-by-element**, so both keys survive — unlike most metadata, where the strongest opinion wins outright. | `beyond-basics/value-resolution.md` |
| 25 | **B** | Content Aggregation | Model-hierarchy rule: "All ancestors of a component model must have their kind metadata set to either a group or a subkind of group, such as assembly." A component also cannot contain another component as a descendant. | `asset-structure/model-hierarchy/what-are-model-kinds.md`, `beyond-basics/model-kinds.md` |
| 26 | **A** | Content Aggregation | "Instancing is driven by your composition arcs. There are no composition arcs on the instanceable prims so there's nothing to generate prototypes from." `instanceable = true` alone yields zero prototypes. | `asset-modularity-instancing/authoring-scenegraph-instancing/scenegraph-instancing-intro.md` |
| 27 | **C** | Content Aggregation | Only the instanceable prim's own root is editable. Prototypes are a runtime data model (not editable) and instance proxies are read-only ("local opinions are discarded"). | `asset-modularity-instancing/refining-scenegraph-instances/scenegraph-instance-refinement.md` |
| 28 | **A, B** | Content Aggregation | A point instancer needs `prototypes` (rel), `protoIndices`, and `positions`; `invisibleIds` animates instances on/off. You **cannot** sparsely edit one array element (C) — you re-author the whole array; `protoIndices` are prototype indices, not positions (D). | `asset-modularity-instancing/authoring-point-instancing/point-instancing-intro.md`, `.../refining-point-instances.md` |
| 29 | **B** | Visualization | "All classes in `UsdGeom` inherit from `UsdGeomImageable`," which provides common renderable properties such as `visibility` and `purpose`. | `scene-description-blueprints/xform.md`, `stage-setting/properties/relationships.md` (purpose/proxyPrim) |
| 30 | **B** | Visualization | "A key feature of Scopes is that they cannot be transformed." It is purely an organizational grouping prim, ideal for containers like a `Materials` scope. | `scene-description-blueprints/scope.md` |
| 31 | **B** | Visualization | Material binding "is encoded as a relationship named `material:binding` that targets a `UsdShade.Material`," authored via `UsdShade.MaterialBindingAPI`. | `stage-setting/properties/relationships.md`, `scene-description-blueprints/materials-shaders.md` |
| 32 | **A, B** | Visualization | A `UsdPreviewSurface` shader exposes a `diffuseColor` input you **connect** to an upstream source; a primvar-reader node reads the named primvar and feeds it. Hardcoding (C) defeats override, and binding uses a relationship not an attribute (D). | `scene-description-blueprints/materials-shaders.md`, `stage-setting/properties/relationships.md`, `beyond-basics/primvars.md` |
| 33 | **B** | Customizing USD | Adding optional properties to *existing* typed prims is an **applied API schema** (suffix `API`), list-edited in `apiSchemas` and queried via `HasAPI`; applied API schemas derive from `UsdAPISchemaBase` (not `UsdTyped`). | `scene-description-blueprints/schemas.md` |
| 34 | **B** | Customizing USD | "Each prim can only subscribe to one IsA schema at a time" (its `typeName`), but multiple **API** schemas can be applied via the `apiSchemas` list. | `scene-description-blueprints/schemas.md` |
| 35 | **B** | Customizing USD | API-schema properties are "namespaced with the schema's base name and camelCased" — `UsdPhysics.RigidBodyAPI.CreateVelocityAttr()` authors `physics:velocity`. | `scene-description-blueprints/schemas.md` |

> **Note on sourcing:** These questions were authored for this repository to extend self-study; they are **not** from the official study guide. Each is grounded in the cited [Learn OpenUSD](https://docs.nvidia.com/learn-openusd/latest/index.html) lesson (the `docs/…` source of the corresponding published page). The official, exam-calibrated samples remain in [`PRACTICE_QUESTIONS.md`](./PRACTICE_QUESTIONS.md); the domain weights and study plan are in [`EXAM_PREP.md`](./EXAM_PREP.md).
