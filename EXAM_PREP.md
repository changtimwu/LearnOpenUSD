# OpenUSD Development Certification — Step-by-Step Prep Plan

A practice-driven study plan for the **NVIDIA-Certified Professional: OpenUSD Development (NCP-OpenUSD)** exam, mapping each exam domain to the [Learn OpenUSD](https://docs.nvidia.com/learn-openusd/latest/index.html) chapters and the notebook (or hands-on exercise) you should practice with.

> 📝 Practice questions from the official study guide are in **[`PRACTICE_QUESTIONS.md`](./PRACTICE_QUESTIONS.md)** — use them as a timed mock exam at the end. For extra drilling, **[`PRACTICE_QUESTIONS_EXTRA.md`](./PRACTICE_QUESTIONS_EXTRA.md)** has 35 additional questions organized by the domains below (authored from the Learn OpenUSD lessons, not official).

> Built from the official **NCP: OpenUSD Development Exam Study Guide v1.1.0** (`ncp-open-usd-development-study-guide.pdf`). The exam is scored across **8 weighted competency domains** — this plan is ordered **by exam weight (highest first)** so your study time tracks where the points are. The study guide also contains **sample questions (pp. 10–18) with an answer key (p. 18)** — do those last as a mock exam.

---

## Exam at a glance

| Order | Domain | Weight | Primary Learn OpenUSD module(s) |
|---|---|---|---|
| 1 | **Composition** | **23%** | Composition Basics → Creating Composition Arcs |
| 2 | **Data Exchange** | **15%** | Data Exchange |
| 3 | **Pipeline Development** | **14%** | All modules (architectural) + Asset Structure |
| 4 | **Data Modeling** | **13%** | Setting the Stage, Scene Description Blueprints, Beyond Basics |
| 5 | **Debugging & Troubleshooting** | **11%** | Creating Composition Arcs (composition debugging) |
| 6 | **Content Aggregation** | **10%** | Model Kinds, Asset Structure, Asset Modularity & Instancing |
| 7 | **Visualization** | **8%** | Scene Description Blueprints, Beyond Basics |
| 8 | **Customizing USD** | **6%** | Model Kinds + plugin/C++ reading (lightest Learn OpenUSD coverage) |

**Job focus (from the guide):** building, maintaining, and optimizing OpenUSD-based 3D content pipelines; expertise in the OpenUSD framework and **Python/C++**, asset interchange, and performance optimization.

---

## How to use this plan

1. **Read** the chapter in the learning doc.
2. **Practice** the paired resource:
   - 📓 **notebook** — run every cell in JupyterLab, then change values and re-run to confirm you understand *why* output changes.
   - 🛠️ **exercise** — downloadable project (do it in your own Python env / `usdview`); files live under `docs/exercise_content/`.
   - 📖 **read-only** — concept or API reading; no code in the repo.
3. **Tick the objective checkboxes** — they are the study guide's actual exam objectives. You're ready for a domain when you can *do* each one from memory.

**Start the notebook server** (from the repo root):

```bash
uv run sphinx-build -M html docs/ docs/_build/    # one-time / after content changes
uv run launch_notebooks                            # opens JupyterLab at docs/_build/jupyter_execute
```

Notebook paths are **relative to the JupyterLab root** (`docs/_build/jupyter_execute/`).

---

## Domain 1 — Composition (23%) 🥇

> *Author, design with, and debug composition arcs; know all arcs, how they work, when to use each, and debug complex LIVRPS scenarios.* This is nearly a quarter of the exam — spend the most time here.

**Step 1a — Composition Basics**

| Chapter | Practice |
|---|---|
| Layers | 📖 `composition-basics/layers.md` |
| Strength Ordering | 📖 `composition-basics/strength-ordering.md` |
| Specifiers (`def`/`over`/`class`) | 📓 `composition-basics/specifiers.ipynb` |
| References | 📓 `composition-basics/references.ipynb` |
| Default Prim | 📓 `composition-basics/default-prim.ipynb` |
| Variant Sets | 📓 `composition-basics/variant-sets.ipynb` |

**Step 1b — Creating Composition Arcs (deep dive)** 🛠️ `docs/exercise_content/composition_arcs/`

| Chapter | Practice |
|---|---|
| Prim Composition (composition graph) | 🛠️ `creating-composition-arcs/prim-composition` |
| Sublayers | 🛠️ `…/sublayers/working-with-sublayers` |
| References & Payloads | 🛠️ `…/references-payloads/working-with-references`, `…/working-with-payloads` |
| Encapsulation | 🛠️ `…/encapsulation/experimenting-with-encapsulation` |
| Variant Sets | 🛠️ `…/variant-sets/working-with-variant-sets` |
| Inherits & Specializes | 🛠️ `…/inherits-specializes/experimenting-with-inherits`, `…/experimenting-with-specializes` |
| Strength Ordering (LIVRPS) | 🛠️ `…/strength-ordering/tracing-through-liverps` |

**Exam objectives to master:**
- [ ] 1.1 Change the strength of an opinion.
- [ ] 1.2 Choose an appropriate instancing style for data at different scales.
- [ ] 1.3 Compare referencing, payloads, and sublayers — and identify their use cases.
- [ ] 1.4 Create a scene where multiple elements reference the same animation at different time-offsets (layer offsets / value clips).
- [ ] 1.5 Design scene-layering strategies for multi-user workflows.
- [ ] 1.6 Explain **LIVRPS** in simple terms (Local, Inherits, Variants, References, Payloads, Specializes).
- [ ] 1.7 Identify when variants are/are not appropriate for structuring assets.
- [ ] 1.8 Explain why opinions from one layer don't take effect in a composed stage.
- [ ] 1.9 Prepare an internal asset for delivery to external parties.
- [ ] 1.10 Remove properties from instanced component prims in an assembly stage.
- [ ] 1.11 Split a monolithic asset into multiple collaborative workstreams.

---

## Domain 2 — Data Exchange (15%) 🥈

> *Conceptual data mapping, custom importers/exporters, and interchange scripts.* 🛠️ `docs/exercise_content/data_exchange/`

| Chapter | Practice |
|---|---|
| What is Data Exchange | 🛠️ `data-exchange/data-exchange/exercise` |
| Data Extraction (geometry) | 🛠️ `…/data-extraction/exercise-extracting-geometry` |
| Data Extraction (materials) | 🛠️ `…/data-extraction/exercise-extracting-materials` |
| Asset Validation | 🛠️ `…/asset-validation/exercise-asset-validation-testing` |
| Data Transformation (hierarchy & export options) | 🛠️ `…/data-transformation/transformation-hierarchy`, `…/exercise-export-option` |

**CLI/API to know:** `usdchecker`, `usdcat`, `usdzip`, `usd2gltf`; `UsdGeomPoints`, `UsdProperty::GetNamespace()`, `UsdGeomModelAPI::GetExtentsHint()`.

**Exam objectives to master:**
- [ ] 4.1 Convert OpenUSD assets to common 3D formats (e.g. glTF) and ensure fidelity.
- [ ] 4.2 Document conceptual mappings between USD and another data model (e.g. materialX).
- [ ] 4.3 Explain trade-offs between **USDC and USDA** (performance, readability, archival).
- [ ] 4.4 Implement a "round-trip" pipeline in a DCC to and from USD.
- [ ] 4.5 Use USD schemas to support nonstandard attributes/structures during import/export.
- [ ] 4.6 Write a validator for the integrity of a USD asset exported from a DCC.
- [ ] 4.7 Write an exporter/converter to USD.
- [ ] 4.8 Write or extend a USD importer in a DCC.

---

## Domain 3 — Pipeline Development (14%) 🥉

> *Architectural/well-rounded tasks: pipeline design, asset management, versioning, exporter hooks, build config, flattening, removing proprietary dependencies.* Reinforced by **Asset Structure** + everything you've built so far.

| Chapter | Practice |
|---|---|
| Asset Structure: why it matters | 📖 `asset-structure/asset-structure-principles/why-necessary` |
| Your First Asset + Asset Interface (pt 1–3) | 🛠️ `…/your-first-asset`, `…/asset-interface-pt1..3` |
| Organizing hierarchy & encapsulation | 🛠️ `…/organizing-prim-hierarchy`, `…/encapsulating-your-asset` |
| Reference/Payload pattern | 🛠️ `…/reference-payload-pattern/exercise-ref-payload-pattern` |
| Export options & flatten | 🛠️ `data-exchange/data-transformation/exercise-export-option` |

**API to know:** `UsdStage::Flatten()`, `UsdStage::Traverse()`, Edit Targets, `SdfChangeBlock`, `UsdNotice`, custom `Ar` resolvers.

**Exam objectives to master:**
- [ ] 7.1 Convert OpenUSD assets to common 3D formats (e.g. glTF) and ensure fidelity.
- [ ] 7.2 Document asset structure guidelines.
- [ ] 7.3 Explain USDC vs USDA trade-offs.
- [ ] 7.4 Implement round-trip pipelines between a DCC and USD.
- [ ] 7.5 Integrate custom resolvers to manage asset paths dynamically.
- [ ] 7.6 Represent custom metadata.
- [ ] 7.7 Validate that asset paths are formatted correctly.
- [ ] 7.8 Write or extend a USD importer in a DCC.

---

## Domain 4 — Data Modeling (13%)

> *USD/Sdf data structures and types: prims, properties (attributes/relationships), primvars, value types (float, token, matrix4d…), timeSamples, built-in schemas.*

**Step 4a — Setting the Stage**

| Chapter | Practice |
|---|---|
| Stage | 📓 `stage-setting/stage.ipynb` |
| Prims | 📓 `stage-setting/prims.ipynb` |
| Attributes | 📓 `stage-setting/properties/attributes.ipynb` |
| Relationships | 📓 `stage-setting/properties/relationships.ipynb` |
| Time Codes & Time Samples | 📓 `stage-setting/timecodes-timesamples.ipynb` |
| Prim & Property Paths | 📓 `stage-setting/prim-property-paths.ipynb` |
| Metadata | 📓 `stage-setting/metadata.ipynb` |
| USD File Formats / Modules | 📖 `stage-setting/usd-file-formats.md`, `usd-modules.md` |

**Step 4b — Scene Description Blueprints & Beyond Basics (data types)**

| Chapter | Practice |
|---|---|
| Schemas (IsA vs applied API) | 📓 `scene-description-blueprints/schemas.ipynb` |
| Primvars | 📓 `beyond-basics/primvars.ipynb` |
| Value Resolution | 📓 `beyond-basics/value-resolution.ipynb` |
| Custom Properties | 📓 `beyond-basics/custom-properties.ipynb` |
| Units | 📓 `beyond-basics/units.ipynb` |

**API to know:** `UsdAttribute` interpolation, `UsdGeomPrimvar`, `Sdf` value types, `UsdGeomBoundable`/`Xformable`/`XformCache`, `UsdStage::SetInterpolationType()`.

**Exam objectives to master:**
- [ ] 5.1 Add a primvar to a mesh.
- [ ] 5.2 Choose appropriate value types to store attribute data.
- [ ] 5.3 Represent custom metadata.
- [ ] 5.4 Retrieve properties of a prim.
- [ ] 5.5 Understand what causes unexpected visual results.
- [ ] 5.6 Update the `extent` attribute of a mesh after updating its points.

---

## Domain 5 — Debugging & Troubleshooting (11%)

> *Introspect stages to fix unexpected composition, find bad data, optimize load/render times.* Heavily overlaps Composition — drill the composition-graph and value-resolution tools.

| Chapter | Practice |
|---|---|
| Prim Composition / inspecting authored data | 🛠️ `creating-composition-arcs/prim-composition` |
| Value Resolution | 📓 `beyond-basics/value-resolution.ipynb` |
| Strength Ordering (LIVRPS tracing) | 🛠️ `…/strength-ordering/tracing-through-liverps` |
| Active / Inactive Prims | 📓 `beyond-basics/active-inactive-prims.ipynb` |
| Stage Traversal | 📓 `beyond-basics/stage-traversal.ipynb` |

**Tools to know:** `usdview` LayerStack tab, `usdchecker`, `TfDebug`, diagnostic delegates (`TfDiagnosticMgr`, `UsdUtilsCoalescingDiagnosticDelegate`), `Trace`, `TfMallocTag`, `SdfChangeBlock`, `UsdStage::MuteLayer()`, `UsdProperty::GetPropertyStack()`, `Pcp` composition errors.

**Exam objectives to master:**
- [ ] 6.1 Identify when `SdfChangeBlock` can alleviate performance bottlenecks.
- [ ] 6.2 Identify why opinions from one layer don't take effect in a composed stage.
- [ ] 6.3 Resolve issues related to asset management.
- [ ] 6.4 Understand what causes unexpected visual results.
- [ ] 6.5 Work with diagnostics for debugging and profiling (TfDebug, diagnostic delegates, Trace, TfMallocTag…).

---

## Domain 6 — Content Aggregation (10%)

> *Build modular, reusable components; leverage native and point instancing; override instanced assets for large-scale scenes.*

| Chapter | Practice |
|---|---|
| Model Kinds | 📓 `beyond-basics/model-kinds.ipynb` |
| Model Hierarchy (groups/components/assemblies) | 🛠️ `asset-structure/model-hierarchy/exercise-groups`, `…/exercise-components`, `…/exercise-assemblies` |
| Asset Modularity | 🛠️ `asset-modularity-instancing/asset-modularity/exercise-assets-overview` |
| Scenegraph Instancing (+ nested) | 🛠️ `…/authoring-scenegraph-instancing/exercise-authoring-scenegraph-instancing`, `…/exercise-nested-instancing` |
| Refining Scenegraph Instances | 🛠️ `…/refining-scenegraph-instances/*` |
| Point Instancing (author + refine) | 🛠️ `…/authoring-point-instancing/exercise-authoring-point-instancing`, `…/refining-point-instances` |

**API to know:** `UsdGeomPointInstancer`, scenegraph instancing / `instanceProxy`, `Kind` core hierarchy, `UsdShadeNodeGraph`.

**Exam objectives to master:**
- [ ] 2.1 Add a new prototype to a `pointInstancer`.
- [ ] 2.2 Edit the color of an `instanceProxy` mesh without changing its instancing status.
- [ ] 2.3 Hide instances of a `pointInstancer` efficiently.
- [ ] 2.4 Implement and manage USD instances for efficient reuse in large scenes.
- [ ] 2.5 Remove properties from instanced component prims in an assembly stage.

---

## Domain 7 — Visualization (8%)

> *UsdGeom, UsdShade, UsdLux — meshes, cameras, materials, lights. Used in almost every USD case.*

| Chapter | Practice |
|---|---|
| Xform / XformCommonAPI | 📓 `scene-description-blueprints/xform.ipynb`, `xformcommonapi.ipynb` |
| Scope | 📓 `scene-description-blueprints/scope.ipynb` |
| Lights (UsdLux) | 📓 `scene-description-blueprints/lights.ipynb` |
| Materials & Shaders (UsdPreviewSurface) | 📓 `scene-description-blueprints/materials-shaders.ipynb` |
| Primvars (drive shading) | 📓 `beyond-basics/primvars.ipynb` |
| Hydra (rendering concepts) | 📖 `beyond-basics/hydra.md` |

**API to know:** `UsdShadeMaterial`, `UsdShadeMaterialBindingAPI`, `UsdGeomSubset`, `UsdGeomMesh`, `UsdLuxLightAPI`/`ShadowAPI`/`DistantLight`/`ShapingAPI`.

**Exam objectives to master:**
- [ ] 8.1 Add a primvar to a mesh.
- [ ] 8.2 Assign `UsdPreviewSurface` materials for asset visualization.
- [ ] 8.3 Bind materials to a mesh.
- [ ] 8.4 Create a `UsdPreviewSurface` shading network that reads diffuse colors from a primvar.

---

## Domain 8 — Customizing USD (6%)

> *USD plugin development: custom schemas, file-format plugins, custom model kinds, custom resolvers, SceneIndex plugins.* Lightest Learn OpenUSD coverage and the most **C++/plugin**-heavy — mostly reading + the model-kinds notebook. Don't over-invest given the 6% weight, but know the concepts.

| Chapter | Practice |
|---|---|
| Model Kinds (custom kinds) | 📓 `beyond-basics/model-kinds.ipynb` |
| Schemas (basis for custom schemas) | 📓 `scene-description-blueprints/schemas.ipynb` |
| Plugin concepts | 📖 *What Are OpenUSD Schemas?*, "Creating a File Format Plug-in", `usdGenSchema`, `PlugRegistry`, `Ar: Asset Resolution` (study-guide links) |

**Exam objectives to be familiar with:**
- [ ] 3.1 Build a USD plug-in against a given version of USD.
- [ ] 3.2 Build USD from scratch with custom dependencies.
- [ ] 3.3 Create custom model kinds when appropriate.
- [ ] 3.4 Create custom schemas for proprietary data models.
- [ ] 3.5 Integrate a custom resolver to manage asset paths dynamically.
- [ ] 3.6 Use USD schemas to support nonstandard attributes/structures during import/export.
- [ ] 3.7 Write a SceneIndex plug-in to generate renderable geometry directly as Hydra prims.
- [ ] 3.8 Write an AssetResolver that generates in-memory renderable primitives.

---

## Final review (last few days)

1. **Re-run the 📓 notebooks for Domains 1, 4, 6, 7** without reading the prose — these test hands-on API fluency, which the exam probes hardest.
2. **Re-trace LIVRPS** until automatic, and practice answering "why doesn't this opinion win?" (Domains 1 & 5 = 34% combined).
3. **Drill the C++/plugin reading** for Customizing USD (Domain 8) — concepts only, given its 6% weight.
4. **Take the [practice questions](./PRACTICE_QUESTIONS.md)** (from the study guide, pp. 10–18) as a timed mock; review every miss against its domain above.
5. Keep the **[OpenUSD Cheatsheet](https://docs.nvidia.com/learn-openusd/latest/openusd-cheatsheet.html)** and **[Glossary](https://docs.nvidia.com/learn-openusd/latest/glossary.html)** open for quick reference.

### Suggested pacing (≈3 weeks)

| Week | Focus (by weight) |
|---|---|
| 1 | Domain 1 Composition (23%) + Domain 4 Data Modeling (13%) — the API core. |
| 2 | Domain 2 Data Exchange (15%) + Domain 3 Pipeline (14%) + Domain 5 Debugging (11%). |
| 3 | Domain 6 Content Aggregation (10%) + Domain 7 Visualization (8%) + Domain 8 Customizing (6%), then sample-question mock + weak-spot drilling. |

> **Note:** Python is enough for ~all hands-on practice in this repo, but the exam expects **C++/plugin** awareness for Customizing USD and parts of Pipeline/Debugging — read those API references even though there's no notebook for them. Viewing 3D scenes needs internet (the in-browser `model-viewer` loads from a CDN); the OpenUSD API practice itself runs fully offline / CPU-only (see the README's Platform Support section).
