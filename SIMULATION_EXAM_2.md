# OpenUSD Development Certification — Simulation Exam 2

A full-length **70-question** mock exam. The domain mix mirrors the *NCP: OpenUSD Development Exam Study Guide v1.1.0* weights (see [`EXAM_PREP.md`](./EXAM_PREP.md)). This is **Exam 2 of 3** — see also Exams [1](./SIMULATION_EXAM_1.md), [3](./SIMULATION_EXAM_3.md).

> ⚠️ **Authored practice, not official.** These questions were written for this repo and grounded in the [Learn OpenUSD](https://docs.nvidia.com/learn-openusd/latest/index.html) lessons (the answer key cites the source lesson for each). The official sample questions are in [`PRACTICE_QUESTIONS.md`](./PRACTICE_QUESTIONS.md).

**How to use:** Give yourself a fixed time budget, answer all 70 *without* scrolling to the [Answer Key](#answer-key), then score by domain to find weak spots. Watch the **"Select N options"** prompts — several are multi-select.

| Domain | Weight | Questions |
|---|---|---|
| Composition | 23% | Q1–Q16 |
| Data Exchange | 15% | Q17–Q26 |
| Pipeline Development | 14% | Q27–Q36 |
| Data Modeling | 13% | Q37–Q45 |
| Debugging & Troubleshooting | 11% | Q46–Q53 |
| Content Aggregation | 10% | Q54–Q60 |
| Visualization | 8% | Q61–Q66 |
| Customizing USD | 6% | Q67–Q70 |
| **Total** | **100%** | **70** |

---

## Question 1 — Composition

A reference list is composed for a prim that uses list editing. Given:

```usda
def Xform "Prop" (
    prepend references = @./strong.usd@
    append references = @./weak.usd@
)
{
}
```

If both referenced layers author the same attribute, which contributes the stronger opinion?

- **A.** weak.usd, because appended items are composed last and last wins
- **B.** strong.usd, because prepended references are inserted before (stronger than) appended ones
- **C.** Neither; appended and prepended references cannot coexist
- **D.** Whichever file is larger on disk

---

## Question 2 — Composition

Which two statements about list editing reference arcs and the `AddReference` API are correct? (Select two.)

- **A.** Calling AddReference without a position prepends the reference by default, giving the intuitive 'on top' result
- **B.** A prepended reference is inserted before (stronger than) references in weaker sublayers
- **C.** AddReference always replaces any existing references on the prim
- **D.** Appended references are stronger than prepended ones

---

## Question 3 — Composition

A specialize arc is best described as broadcasting specs across layer stacks as:

- **A.** The strongest opinions, overriding everything else
- **B.** Fallback values, applied to destination prims only when no other authored opinion exists for that spec
- **C.** Loadable payloads that can be toggled at runtime
- **D.** Sublayer includes with no remapping

---

## Question 4 — Composition

A prim resolves to a `class` specifier on the composed stage. Which two statements are true? (Select two.)

- **A.** It is present and composed on the stage
- **B.** It is intended as a target of reference, payload, inherit, or specialize arcs
- **C.** It will be visited by default stage traversals such as rendering
- **D.** Hydra renders it and all of its descendants by default

---

## Question 5 — Composition

You author an `over` on `/World/Box` in a stronger layer that sublayers a base file, changing only `size`, and add an inherits arc to a class. Inspecting the prim with `prim.GetPrimStack()` (returns specs strong-to-weak), what does the printout reveal about the contributions?

- **A.** Only the strong layer contributes; the base's def is discarded
- **B.** The strong layer contributes the over while the base layer contributes the def, and the class opinions arrive via the inherit
- **C.** GetPrimStack returns only the resolved values, not per-layer specs
- **D.** The inherit arc removes the base layer's def from the stack

---

## Question 6 — Composition

A reusable asset is defined so that you can later override ALL references of that asset within another layer stack by editing one source prim. Which arc should the asset author include for that source prim?

- **A.** A sublayer of the asset
- **B.** An inherit arc with an unencapsulated source prim
- **C.** A payload pointing back to the asset
- **D.** An over specifier on each reference site

---

## Question 7 — Composition

Using inherits, you author an override on the source prim in the context of `Scenario_A`. What is the scope of that broadcast?

- **A.** It affects every stage that references the asset, including Scenario_B
- **B.** It applies only to the context (Scenario_A) in which it is authored and does not affect Scenario_B
- **C.** It permanently rewrites the original asset on disk
- **D.** It has no effect because inherits cannot be overridden

---

## Question 8 — Composition

A character asset uses a 'geometry' variant set (hat / no hat) and a separate 'surface' variant set (dry / wet). Why does the lesson warn that combinations are 'not free'?

- **A.** Variant sets cannot be combined; only one may exist per prim
- **B.** You must author relevant data for each combination, risking unused combos or non-functional ones if data is missing
- **C.** Each combination forces a full re-download of the asset from disk
- **D.** Combining variant sets disables non-destructive editing

---

## Question 9 — Composition

A variant can author a new composition arc that only exists when that variant is active. For example, a 'color variant' set on `/World` where each variant references a different material. When you switch the variant from red to green, what changes for the `CubeMaterial` prim?

- **A.** The material's local def opinion is edited in place
- **B.** The reference target for CubeMaterial changes (e.g. from /Looks/Red to /Looks/Green in materials.usd)
- **C.** The defaultPrim of materials.usd is reassigned
- **D.** Nothing changes; variants cannot author references

---

## Question 10 — Composition

In the LIVRPS recursive gathering of a layer stack, sublayers of sublayers are collected in a depth-first order. Given root with sublayers `[sublayer1.usda, sublayer2.usda]`, and `sublayer1.usda` has its own sublayers `sublayer1A.usda` and `sublayer1B.usda`, which opinions are stronger?

- **A.** Opinions in sublayer2.usda are stronger than those in sublayer1A and sublayer1B
- **B.** Opinions in sublayer1A and sublayer1B are stronger than those in sublayer2.usda
- **C.** All four sublayers have equal strength
- **D.** sublayer1B is stronger than the root layer itself

---

## Question 11 — Composition

Which two statements correctly contrast inherits and specializes? (Select two.)

- **A.** Inherits broadcasts override-strength opinions that take precedence as a stronger arc
- **B.** Specializes broadcasts fallback values that lose to any other authored opinion, being the weakest arc
- **C.** Specializes is the strongest arc and always wins
- **D.** Inherits is the weakest arc, used for material fallbacks

---

## Question 12 — Composition

When a `class` prim is used as the source for an inherit arc, why does this serve the typical purpose well in Hydra?

- **A.** Hydra renders the class prim twice for redundancy
- **B.** Hydra does not image the class prim or any of its descendants, which is desirable for purely abstract data
- **C.** Hydra converts the class to a def automatically at render time
- **D.** Hydra refuses to compose any stage containing a class prim

---

## Question 13 — Composition

An author adds the same skyscraper layer twice to a scene. The lesson explains why they used references rather than sublayers for this. What is the reason?

- **A.** Sublayers compose faster than references
- **B.** Adding the layer twice as a sublayer would overwrite itself rather than create a second skyscraper, since sublayers do not remap paths
- **C.** Sublayers cannot contain geometry
- **D.** References require a defaultPrim while sublayers do not

---

## Question 14 — Composition

Which two statements about encapsulation and references are correct? (Select two.)

- **A.** A prim is encapsulated when it is a descendant of the referenced (entry-point) prim, so it is brought along with that ancestor
- **B.** A bound material that is a descendant of the referenced prim keeps its binding intact after referencing
- **C.** Relationship targets are always automatically remapped, so encapsulation never matters
- **D.** An unencapsulated material binding always still resolves in the new context

---

## Question 15 — Composition

A reference targets a SPECIFIC prim path within an asset rather than relying on the asset's defaultPrim:

```usda
def "Mat" (
    prepend references = @./materials.usd@</Looks/Green>
)
{
}
```

What does specifying `</Looks/Green>` accomplish here?

- **A.** It overrides the materials.usd file's defaultPrim metadata permanently
- **B.** It grafts the /Looks/Green prim hierarchy specifically, instead of using the layer's defaultPrim as the entry point
- **C.** It forces the reference to be treated as a payload
- **D.** It is invalid; reference paths must always be omitted

---

## Question 16 — Composition

Which two statements about variant selections are correct? (Select two.)

- **A.** A variant selection can be authored in a different layer that references the prim, so each reference can pick a different option
- **B.** Only the opinions of the currently selected variant are composed onto the stage
- **C.** A variant selection must always be authored on the same layer that defines the variant set
- **D.** Selecting a variant composes the opinions of all variants in the set simultaneously

---

## Question 17 — Data Exchange

Which TWO statements about OpenUSD file format plugins are correct?

*(Select two options.)*

- **A.** They can be bidirectional, supporting both reading from and writing to the source file format
- **B.** They allow non-file-based sources, such as databases or procedurally generated content, to be composed into a stage
- **C.** Once composed, the source file is permanently converted to USDC and the original is discarded
- **D.** They can only be used inside DCC applications and never as standalone converters
- **E.** They require the source format to be Y-up

---

## Question 18 — Data Exchange

A developer is building a dedicated microservice that translates between FBX and USD and documents that it supports both directions. Which implementation type is this, and what is a key responsibility noted for it?

- **A.** A standalone converter; because direction is not implied, the developer must document whether it is one-way or two-way
- **B.** An importer; it must be a native feature of a DCC application
- **C.** A file format plugin; the source format must always remain the source of truth
- **D.** An exporter; it must produce only a single-file USDZ output

---

## Question 19 — Data Exchange

A source format has a compound 'material' record with grouped fields (roughness, color). USD has no native struct type. According to the lessons, what is the standard convention for representing this grouped data in USD?

- **A.** Encode the entire record as a single JSON string in customData
- **B.** Create a separate prim for each field and link them with relationships
- **C.** Use namespace-prefixed attributes (e.g., mtl:roughness, mtl:color) to group related properties
- **D.** Store the fields as time samples on a single attribute

---

## Question 20 — Data Exchange

In the material extraction exercise, the OBJ shininess is converted with:

```python
roughness = 1 - math.sqrt(mtl["SHININESS"] / 1000.0)
```

What does this line accomplish in the conceptual data mapping?

- **A.** It maps an OBJ specular shininess value to a UsdPreviewSurface roughness input
- **B.** It converts the diffuse color from one color space to another
- **C.** It computes the metallic value for a PBR workflow
- **D.** It validates that the material name is a legal identifier

---

## Question 21 — Data Exchange

In the material graph built during extraction, which line lofts the shader's output up to the material container so the material exposes a surface?

- **A.** shader.CreateIdAttr("UsdPreviewSurface")
- **B.** material.CreateSurfaceOutput().ConnectToSource(shader.ConnectableAPI(), UsdShade.Tokens.surface)
- **C.** UsdShade.MaterialBindingAPI.Apply(usd_mesh.GetPrim())
- **D.** shader.CreateInput("roughness", Sdf.ValueTypeNames.Float).Set(roughness)

---

## Question 22 — Data Exchange

Which line correctly applies the MaterialBindingAPI to a mesh prim and binds a material to it?

- **A.** usd_mesh.BindMaterial(material)
- **B.** material.Bind(usd_mesh.GetPrim())
- **C.** binding_api = UsdShade.MaterialBindingAPI.Apply(usd_mesh.GetPrim()); binding_api.Bind(material)
- **D.** stage.SetDefaultPrim(material.GetPrim())

---

## Question 23 — Data Exchange

Which TWO statements about the reusability boundary between extraction and transformation code are correct?

*(Select two options.)*

- **A.** Extraction code from a source format to USD is largely unique to that format
- **B.** Transformation code (export options, optimizations, asset structure) can be shared across multiple exporters because it operates on USD data
- **C.** Extraction code is the most reusable layer and should be shared across all exporters
- **D.** Transformation code must be re-implemented per source format because it depends on the original file structure
- **E.** Both extraction and transformation code are equally source-specific and never reusable

---

## Question 24 — Data Exchange

When designing workflows that involve round-trip data exchange (import then export), what guidance do the lessons give about transformations?

- **A.** Apply as many transformations as possible to maximize end-client compatibility
- **B.** Minimize transformations, because extensive transforms complicate accurately reversing changes on reimport
- **C.** Always flatten the stage into a single USDC layer before exporting
- **D.** Skip the extraction phase and transform directly from the source format

---

## Question 25 — Data Exchange

An asset that has been completed needs to be delivered to an XR experience along with all of its texture files as one self-contained package, with no further editing expected. Which format is intended for this?

- **A.** USDA
- **B.** USDC
- **C.** USDZ
- **D.** A .usd file with the binary backend

---

## Question 26 — Data Exchange

Why is the geometric stage metadata set via UsdGeom (e.g., UsdGeom.SetStageUpAxis, UsdGeom.SetStageMetersPerUnit) rather than directly on Usd.Stage?

- **A.** Because Usd.Stage cannot store any metadata
- **B.** Because the API for this geometric stage metadata lives in UsdGeom, not Usd.Stage where you might first look
- **C.** Because these values are stored as time samples on a prim
- **D.** Because only UsdGeom can write to USDA files

---

## Question 27 — Pipeline Development

An asset structure uses `reference`/`payload` pairs to create a boundary between a lightweight entry-point interface and the heavier prim hierarchies and properties behind the payload. Which scalable-asset-structure principle is this technique most directly serving?

- **A.** Performance
- **B.** Legibility
- **C.** Navigability
- **D.** Modularity

---

## Question 28 — Pipeline Development

Which TWO practices does the curriculum associate with the Navigability principle of scalable asset structure?

*(Select two options.)*

- **A.** Use relationships and collections to boost discoverability without relying on rigid naming conventions
- **B.** Keep model-hierarchy component boundaries shallow and consistent for ease of navigation
- **C.** Use reference/payload pairs to separate lightweight interfaces from heavy contents
- **D.** Encapsulate local dependencies with anchored paths
- **E.** Prefer ASCII or UTF-8 identifiers to avoid conceptual overload

---

## Question 29 — Pipeline Development

When partitioning an asset hierarchy into organizational containers like `Geometry` and `Materials`, the curriculum recommends `Scope` prims over `Xform` prims. What is the primary reason?

- **A.** Scope has no additional semantics like xform's transform operations, avoiding ambiguity such as a Sphere parented under a Material
- **B.** Scope prims are referenceable while Xform prims are not
- **C.** Scope prims are automatically instanced, reducing composed prim count
- **D.** Xform prims cannot have children, so they cannot act as containers

---

## Question 30 — Pipeline Development

A reparenting script sorts each child of the default prim into the right organizational scope. Which check correctly routes mesh prims under a `Geometry` scope and material prims under a `Looks` scope?

- **A.** `prim.IsA(UsdGeom.Mesh)` for meshes and `prim.IsA(UsdShade.Material)` for materials
- **B.** `prim.GetTypeName() == 'Scope'` for both, then sort by name
- **C.** `prim.HasAPI(UsdGeom.Mesh)` for meshes and `prim.HasAPI(UsdShade.Material)` for materials
- **D.** `prim.IsInstance()` to detect meshes and `prim.IsAbstract()` to detect materials

---

## Question 31 — Pipeline Development

An asset's interface includes its entry-point prims, which act as the targets of references for downstream users. Which TWO statements about asset interfaces and entry points are correct per the curriculum?

*(Select two options.)*

- **A.** A single asset entry point can typically be specified using the root layer's `defaultPrim` metadata, which the composition engine respects when referencing
- **B.** Key descendant prims designated stable for downstream overrides (e.g. materials or subcomponent prims) are also considered part of the asset's interface
- **C.** An entry point must always be a `class` prim placed at the top of the root layer
- **D.** Domain-specific entry points like `renderSettingsPrimPath` are forbidden; only `defaultPrim` is allowed
- **E.** An asset can have at most one entry point prim

---

## Question 32 — Pipeline Development

A synthetic-data generation pipeline dynamically partitions a simulation across multiple processes and machines, then stitches the per-process results back together with a layer stack. The partition layout changes from one evaluation to the next. How does the curriculum classify this kind of workstream?

- **A.** A computational workstream, which can be dynamic and may not remain consistent between evaluations
- **B.** A user workstream, since each process is treated like a separate artist
- **C.** A hybrid workstream, because synthetic data always mixes hand-authored state
- **D.** A version-control workstream, since the layer stack replaces the asset versioning system

---

## Question 33 — Pipeline Development

A studio keeps adding layers to a single asset's layer stack on every procedural simulation run, expecting the stack to serve as a running history of all past states. What does the curriculum warn about this approach?

- **A.** Layer stacks are not a replacement for asset versioning systems, and stacks that grow procedurally over time incur a real cost to resolve and open each layer
- **B.** Layer stacks may only contain a maximum of three sublayers before composition fails
- **C.** Procedural layers cannot be sublayered, only referenced, so the stack will not compose
- **D.** Each added layer forces a new instancing prototype, exhausting memory

---

## Question 34 — Pipeline Development

Which TWO fields or APIs does the curriculum cite as good candidates for lofting above a payload because they let consumers preview or query payloaded content before loading it?

*(Select two options.)*

- **A.** `extentsHint` from `UsdGeomModelAPI`/`UsdModelAPI`
- **B.** Variant sets and inheritable properties
- **C.** The payload's `faceVertexIndices` arrays
- **D.** The full shading network of every material
- **E.** Per-vertex point positions of the heaviest mesh

---

## Question 35 — Pipeline Development

When lofting a nested `exteriorType` variant set from a `Looks` scope up to the asset entry point, the script creates a matching variant set on the default prim. Within each lofted variant's edit context it executes `vset.SetVariantSelection(variant)` on the inner set. What is the single opinion each lofted variant carries?

- **A.** It selects the matching variant from the inner `exteriorType` variant set on the Looks prim
- **B.** It redefines the material shading network for that variant
- **C.** It deactivates the Looks scope unless the variant matches
- **D.** It overrides the inner variant set's list of available variants

---

## Question 36 — Pipeline Development

A workflow centers on practical lights scattered throughout an asset, and downstream users frequently need to find and adjust just those lights (intensity, on/off). According to the curriculum, what is the recommended way to express this membership at the asset entry point so the lights are easy to discover and operate on?

- **A.** Use a collection at the asset entry point to highlight the practical-light prims and their role
- **B.** Rename every practical light with a reserved `LIGHT_` prefix and rely on string matching
- **C.** Move all practical lights into a separate payload that loads only when needed
- **D.** Create a variant set whose variants toggle each light individually

---

## Question 37 — Data Modeling

Which TWO statements correctly distinguish a relationship from an attribute in OpenUSD?

*(Select two options.)*

- **A.** A relationship has no value type; it records linkage to prim path(s).
- **B.** A relationship can have multiple targets, useful for grouping objects.
- **C.** A relationship can be time-sampled to animate which prim it points to.
- **D.** An attribute can point to multiple prim paths just like a relationship.
- **E.** A relationship is used to connect two already-existing attributes.

---

## Question 38 — Data Modeling

A stage's root layer has timeCodesPerSecond = 30 and references an asset authored at timeCodesPerSecond = 60. What happens to the referenced asset's animation during composition?

- **A.** USD automatically scales the referenced time samples so playback timing stays correct relative to the root layer.
- **B.** Nothing is reconciled; the assembler must manually rescale the time samples.
- **C.** The referenced animation is dropped because the rates do not match.
- **D.** The root layer's timeCodesPerSecond is overwritten to 60 by the reference.

---

## Question 39 — Data Modeling

A stage has timeCodesPerSecond = 24 with samples at time codes 0 and 24, and no other samples. What value is returned when evaluating the attribute at time code 12, assuming default (linear) interpolation?

- **A.** A value linearly interpolated halfway between the samples at time codes 0 and 24.
- **B.** The sample at time code 0, held unchanged until time code 24.
- **C.** The attribute's schema fallback value, since 12 has no authored sample.
- **D.** An error, because no sample is authored exactly at time code 12.

---

## Question 40 — Data Modeling

Which pair of methods sets the timeline boundaries (startTimeCode and endTimeCode metadata) for a stage?

- **A.** stage.SetStartTimeCode(1) and stage.SetEndTimeCode(60)
- **B.** attr.Set(1) and attr.Set(60)
- **C.** stage.SetTimeCodesPerSecond(1) and stage.SetTimeCodesPerSecond(60)
- **D.** stage.SetStartFrame(1) and stage.SetEndFrame(60)

---

## Question 41 — Data Modeling

Why is UsdGeomPointBased considered an abstract IsA schema while UsdGeomMesh is concrete?

- **A.** UsdGeomPointBased provides a name but no typeName (serving as a base class), while UsdGeomMesh provides both a name and a typeName and can be instantiated.
- **B.** UsdGeomPointBased has a typeName but UsdGeomMesh does not.
- **C.** Both have typeNames, but only UsdGeomMesh is listed in apiSchemas.
- **D.** Abstract schemas are API schemas and concrete schemas are IsA schemas.

---

## Question 42 — Data Modeling

You need an attribute that stores a label restricted to an enumerated set of names (e.g. interpolation modes like 'constant', 'uniform', 'vertex'). Which Sdf value type is the idiomatic choice over a free-form text type?

- **A.** Sdf.ValueTypeNames.Token
- **B.** Sdf.ValueTypeNames.String
- **C.** Sdf.ValueTypeNames.Int
- **D.** Sdf.ValueTypeNames.Color3f

---

## Question 43 — Data Modeling

A UsdGeomSphere schema defines radius as 'double radius = 1'. A prim of that type has no authored radius. You then call sphere.GetRadiusAttr().Set(10). What does GetRadiusAttr().Get() return before and after the Set call?

- **A.** Before: 1 (the schema fallback via value resolution); after: 10 (the authored value overrides the fallback).
- **B.** Before: 0 (uninitialized); after: 10.
- **C.** Before and after: 1, because fallbacks cannot be overridden.
- **D.** Before: error (no value); after: 10.

---

## Question 44 — Data Modeling

Which TWO statements about creating stages match the curriculum?

*(Select two options.)*

- **A.** Usd.Stage.CreateNew(path) creates a stage whose root layer is the file at the given path.
- **B.** Usd.Stage.CreateInMemory() creates a stage whose root layer exists only in memory until you Export it.
- **C.** Usd.Stage.CreateInMemory() immediately writes a file to disk at a temp path.
- **D.** Usd.Stage.CreateNew() can only create .usda files, never .usdc.
- **E.** CreateInMemory() and CreateNew() are aliases that behave identically.

---

## Question 45 — Data Modeling

A primvar authored on a parent prim becomes available to descendant gprims that do not author their own value. What primvar capability does this describe?

- **A.** Primvar inheritance down namespace, enabling sparse authoring of shareable data.
- **B.** Time sampling, which carries the value to children frame by frame.
- **C.** Path translation, which remaps the primvar to children.
- **D.** List editing, which merges primvar opinions from siblings.

---

## Question 46 — Debugging & Troubleshooting

A prop is referenced into a shot and rendered fine, but a downstream lighting tool's `stage.Traverse()` never visits the prop's geometry. The prop is brought in via a payload that the tool opens with payloads unloaded for speed. Which two statements correctly explain the situation? (Select 2)

- **A.** The default traversal predicate includes Usd.PrimIsLoaded; with the payload unloaded the prop's descendant prims are not composed onto the stage and so are not visited.
- **B.** Unloading a payload is non-destructive: loading it (or using TraverseAll) restores visibility without re-authoring anything.
- **C.** Unloading a payload deletes the underlying layer, permanently removing the geometry.
- **D.** Traverse() ignores all referenced prims by design; only TraverseAll() can see across any reference.

---

## Question 47 — Debugging & Troubleshooting

You made several unsaved override edits to a layer in an interactive session and now want to discard all of them and return the in-memory layer to its on-disk state. Which call does this?

- **A.** layer.Reload() — reloads the layer from disk, discarding unsaved in-memory opinions.
- **B.** layer.Save() — writes the edits, which also clears them from memory.
- **C.** stage.MuteLayer(layer.identifier) — permanently erases the unsaved edits.
- **D.** prim.SetActive(False) — deactivates the prims so the edits no longer apply.

---

## Question 48 — Debugging & Troubleshooting

A developer writes `predicate = Usd.PrimIsActive and Usd.PrimIsDefined` and passes it to Usd.PrimRange, but the filtering behaves unexpectedly. What is the correct fix?

- **A.** Combine predicates with the bitwise operator `&`: `Usd.PrimIsActive & Usd.PrimIsDefined`; Python `and`/`or`/`not` do not combine USD predicates correctly.
- **B.** Wrap each predicate in bool(): `bool(Usd.PrimIsActive) and bool(Usd.PrimIsDefined)`.
- **C.** Pass them as a list: `[Usd.PrimIsActive, Usd.PrimIsDefined]`.
- **D.** Use `+` to add predicates: `Usd.PrimIsActive + Usd.PrimIsDefined`.

---

## Question 49 — Debugging & Troubleshooting

During a traversal you want to visit `/World/Environment` itself but skip its entire subtree without paying the cost of descending into it. Which approach matches the lesson?

- **A.** Iterate the range via iter(), and when you reach `/World/Environment`, call PruneChildren() on the iterator to skip its descendants.
- **B.** Call SetActive(False) on `/World/Environment` before traversing, which is the only way to skip a subtree.
- **C.** Switch from Traverse() to TraverseAll(), which skips subtrees you don't visit.
- **D.** Use a Python `continue` inside the loop; PruneChildren is not exposed in Python.

---

## Question 50 — Debugging & Troubleshooting

A prim `/World/Box` and everything under it (its Geometry and Materials scopes) silently disappears from `stage.Traverse()` after a colleague's layer is added, but the prims still exist in scene description. The colleague's layer authored `active = false` on `/World/Box`. What is happening, and how could it be reversed non-destructively?

- **A.** Box was deactivated, pruning it and its descendants from default traversal; a stronger layer authoring active = true reactivates the subtree without deleting anything.
- **B.** The prims were permanently deleted; they must be re-authored from scratch.
- **C.** Only /World/Box is hidden; its children remain in Traverse() because deactivation does not affect descendants.
- **D.** Deactivation removes the prims from disk, so layer.Reload() is required to recover them.

---

## Question 51 — Debugging & Troubleshooting

You are choosing diagnostic tooling to investigate two separate problems: (1) tracking exactly which subsystems are emitting internal trace output, and (2) finding which calls allocate the most memory in a heavy stage-open. Which tools match these needs? (Select 2)

- **A.** TfDebug — enable named debug symbols to see verbose internal trace/diagnostic output from USD subsystems for problem (1).
- **B.** TfMallocTag — instrument and report memory allocation by tagged scope to find allocation hot spots for problem (2).
- **C.** Sdf.ChangeBlock — to measure memory allocations during stage open for problem (2).
- **D.** Usd.PrimRange.PruneChildren — to enable verbose subsystem logging for problem (1).

---

## Question 52 — Debugging & Troubleshooting

When debugging why a referenced asset failed to bring in expected prims, you want to surface errors raised during the composition (Pcp) phase rather than during value reads. Which describes the right layer of the problem to inspect?

- **A.** Composition errors (Pcp composition errors) are raised while USD builds the per-prim index of sources; inspect these to find broken arcs, missing reference/payload targets, or cycles.
- **B.** Composition cannot raise errors; any failure must occur during value resolution on Get().
- **C.** Pcp errors only occur when calling attr.Get() with EarliestTime() and are unrelated to references.
- **D.** Composition errors are identical to relationship target merge warnings and can be ignored safely.

---

## Question 53 — Debugging & Troubleshooting

Two sublayers are listed as `subLayerPaths = [strong.usda, weak.usda]`. weak.usda authors `customData["author"] = "alice"` and `customData["dept"] = "fx"`; strong.usda authors only `customData["author"] = "bob"`. A teammate is confused that GetCustomData() returns both an author and a dept key. What is the correct explanation?

- **A.** Dictionary metadata like customData merges element-by-element: `author` resolves to the stronger layer's "bob", while `dept` persists from the weaker layer because the stronger layer didn't author it.
- **B.** customData uses strongest-wins as a whole dictionary, so only `author = bob` should appear; getting `dept` indicates corruption.
- **C.** customData merges targets via list ops like a relationship, duplicating the author key.
- **D.** The layer order is reversed; weak.usda is actually strongest because the last entry wins.

---

## Question 54 — Content Aggregation

Which describes the correct inheritance structure of OpenUSD's predefined kinds?

- **A.** model is the base; component, group, and subcomponent all inherit from it, and assembly inherits from component.
- **B.** model is the base with component and group as subkinds; assembly is a subkind of group; subcomponent is separate and does not inherit from model.
- **C.** assembly is the base; group, component, and subcomponent inherit from it.
- **D.** group is the base; assembly, component, and subcomponent inherit from it equally.

---

## Question 55 — Content Aggregation

A pipeline wants its own taxonomy distinguishing 'location' assemblies from 'world' assemblies. What does the curriculum recommend regarding the Kind registry?

- **A.** Always extend the Kind registry, because custom kinds are the primary mechanism for site-specific taxonomies.
- **B.** Kinds cannot be extended; you must use a different schema type for each taxonomy.
- **C.** Extending kinds is possible via plugin info but discouraged; prefer custom properties, user properties, or schemas, and rarely expose taxonomies to the Kind library.
- **D.** Add the custom kinds without plugin info so other clients automatically pick them up.

---

## Question 56 — Content Aggregation

An author sees a /__Prototype_01 entry in usdview and writes:\n\nover "__Prototype_01" {\n    color3f primvars:asset_color = [(1, 0, 0)]\n}\n\nWhat is the result?

- **A.** All instances sharing that prototype turn red.
- **B.** It is an invalid edit producing a dangling override unrelated to the actual prototype, because prototypes are a runtime data model and are not editable.
- **C.** It deinstances every instance of that prototype.
- **D.** It creates a new prototype with the red color.

---

## Question 57 — Content Aggregation

Which two refinement techniques on instanced boxes will trigger the creation of a NEW prototype while keeping the instance count unchanged? (Select 2)

- **A.** Hierarchical refinement using primvars on the instanceable prims.
- **B.** Adding an ad hoc composition arc (e.g. an internal reference) to the instanceable prims.
- **C.** Selecting a different variant on the instanceable prims via a variant set.
- **D.** Deinstancing the boxes with SetInstanceable(False).

---

## Question 58 — Content Aggregation

In the warehouse exercise, enabling scenegraph instancing on the component models reduced the stage from 44,408 prims with 0 prototypes to which result?

- **A.** 1,711 prims, 1,450 instances, 3 prototypes.
- **B.** 44,408 prims, 1,450 instances, 0 prototypes.
- **C.** 1,711 prims, 3 instances, 1,450 prototypes.
- **D.** 22,204 prims, 1,450 instances, 1 prototype.

---

## Question 59 — Content Aggregation

You need to permanently prune a single point instance (index 1228) from a PointInstancer so it is removed entirely, not animated on and off. Which API call does this?

- **A.** pi.GetInvisibleIdsAttr().Set([1228])
- **B.** pi.DeactivateId(1228)
- **C.** pi.GetPositionsAttr().Set([])
- **D.** pi.SetInstanceable(False)

---

## Question 60 — Content Aggregation

Which arrangements are VALID model hierarchies according to the kind rules? (Select 2)

- **A.** World (assembly) > North (group) > lrg_bldgF (component) > Annex (component)
- **B.** World (assembly) > North (group) > lrg_bldgF (component) > Annex (subcomponent)
- **C.** World (assembly) > Block (assembly) > lrg_bldgF (component)
- **D.** World (component) > North (group) > lrg_bldgF (component)

---

## Question 61 — Visualization

A `UsdLux.DistantLight` is defined to simulate sunlight. Along which axis does a DistantLight emit, and what common lighting term describes it?

- **A.** It emits along the -Z axis and is commonly known as a directional light
- **B.** It emits radially outward from a point and is known as a point light
- **C.** It emits along the +Y axis and is known as an area light
- **D.** It emits from a flat rectangle and is known as a rect light

---

## Question 62 — Visualization

Why can you call `UsdGeom.XformCommonAPI(distant_light).SetRotate(...)` to orient a `UsdLux.DistantLight`?

- **A.** Lights are Xformable, so XformCommonAPI can position and rotate them like other transformable prims
- **B.** Lights are Scopes, which support transformation through XformCommonAPI
- **C.** SetRotate works on any prim regardless of whether it is transformable
- **D.** DistantLight has no transform, so SetRotate only changes its intensity

---

## Question 63 — Visualization

Which pair of `UsdLux` light accessor methods is used to set a light's color and brightness?

- **A.** GetColorAttr() and GetIntensityAttr()
- **B.** GetPurposeAttr() and GetVisibilityAttr()
- **C.** GetSurfaceOutput() and GetIdAttr()
- **D.** GetPointsAttr() and GetExtentAttr()

---

## Question 64 — Visualization

Your renderer needs a lightweight stand-in drawn in preview while a high-cost prim is reserved for final renders. You author purpose tokens and a proxyPrim link. Which TWO statements are correct?

*(Select two options.)*

- **A.** The high-cost prim's purpose is set to 'render' and the lightweight stand-in's purpose is set to 'proxy'
- **B.** Imageable.ComputeProxyPrim() returns the proxy prim that preview tools should draw
- **C.** proxyPrim is set as a float attribute on the proxy prim pointing back at the render prim
- **D.** The 'proxy' purpose causes the prim to be drawn only in final renders, never in preview

---

## Question 65 — Visualization

You want to author per-vertex colors on a mesh so the color smoothly interpolates across the surface between the mesh's points. Which primvar interpolation mode should you choose?

- **A.** vertex (one value per point, interpolated across the surface)
- **B.** constant (a single value for the whole gprim)
- **C.** uniform (one value per face, no interpolation between faces)
- **D.** faceVarying applied as a single global value

---

## Question 66 — Visualization

A `UsdGeom.Mesh` is being defined programmatically. Which set of attributes specifies the mesh topology (its faces and how they reference points)?

- **A.** points, faceVertexCounts, and faceVertexIndices
- **B.** intensity, color, and exposure
- **C.** diffuseColor, roughness, and metallic
- **D.** translate, rotate, and scale

---

## Question 67 — Customizing USD

What is the key difference between a single-apply and a multiple-apply API schema?

- **A.** A single-apply schema is applied to only a single instance on a prim, while a multiple-apply schema can be applied to the same prim multiple times using different instance names.
- **B.** A single-apply schema sets a typeName, while a multiple-apply schema does not.
- **C.** A single-apply schema derives from UsdTyped, while a multiple-apply schema derives from UsdAPISchemaBase.
- **D.** A single-apply schema is queried with HasAPI, while a multiple-apply schema cannot be queried at all.

---

## Question 68 — Customizing USD

Unlike an IsA schema, how is an API schema recorded on a prim and how is its presence queried at runtime?

- **A.** It is list-edited into the prim's apiSchemas metadata and queried with the HasAPI method.
- **B.** It is written to the prim's typeName metadata and queried with IsA.
- **C.** It is stored in the kind metadata and queried with IsModel.
- **D.** It is set as the stage's default prim and queried with GetDefaultPrim.

---

## Question 69 — Customizing USD

A developer needs to define a brand-new prim type that answers "what is this prim?" and assigns the prim a typeName. Which schema kind should they author?

- **A.** An IsA (typed) schema.
- **B.** A single-apply API schema.
- **C.** A multiple-apply API schema.
- **D.** A custom model kind in the Kind registry.

---

## Question 70 — Customizing USD

OpenUSD shows a trend toward 'codeless' schemas. What primarily characterizes a codeless schema and why is it favored?

- **A.** It focuses on data modeling without compiled C++ behavior code, making schemas more lightweight and easier to distribute.
- **B.** It removes the schema's properties so prims carry no data, reducing file size.
- **C.** It compiles the schema directly into the USD core so it cannot be overridden.
- **D.** It stores the schema definition inside each asset layer instead of in a plugin.

---

## Answer Key

| Q | Answer | Domain | Why | Grounded in |
|---|---|---|---|---|
| 1 | **B** | Composition | Prepend inserts a reference before weaker references so it contributes stronger opinions, while append places references in the weaker position. | `creating-composition-arcs/references-payloads/references-faq.md` |
| 2 | **A, B** | Composition | AddReference prepends by default, and prepend inserts the reference before weaker references so it contributes stronger opinions. | `creating-composition-arcs/references-payloads/references-faq.md` |
| 3 | **B** | Composition | Specializes broadcasts specs as fallback values that apply to destination prims only when there is no other authored opinion for that spec. | `creating-composition-arcs/inherits-specializes/what-is-specializes.md` |
| 4 | **A, B** | Composition | Class prims are present and composed on the stage and serve as blueprints/targets for arcs, but default traversals (and Hydra rendering) skip them and their descendants. | `composition-basics/specifiers.md` |
| 5 | **B** | Composition | GetPrimStack lists the contributing prim specs strong-to-weak: the strong layer's over, the base layer's def, plus the class opinions applied through the inherit arc. | `composition-basics/specifiers.md` |
| 6 | **B** | Composition | Including an inherit arc with an unencapsulated source prim lets you broadcast overrides to all references of the asset by editing the single source prim in another layer stack. | `creating-composition-arcs/inherits-specializes/what-is-inherits.md` |
| 7 | **B** | Composition | The inherits broadcast applies only to the context in which the opinion is authored, so an opinion in Scenario_A does not affect Scenario_B; that differs from editing the asset directly. | `creating-composition-arcs/inherits-specializes/what-is-inherits.md` |
| 8 | **B** | Composition | Each combination requires authored data; you risk creating combinations that are never used or leaving some combinations without data, creating bad UX. | `creating-composition-arcs/variant-sets/what-are-variant-sets.md` |
| 9 | **B** | Composition | Variants can author composition arcs that only exist in that variant, so switching the variant changes which material reference target is composed for CubeMaterial. | `creating-composition-arcs/variant-sets/working-with-variant-sets.md` |
| 10 | **B** | Composition | Because sublayer1 precedes sublayer2 in the list, sublayers gathered recursively under sublayer1 (1A and 1B) are stronger than sublayer2 in the depth-first strength ordering. | `creating-composition-arcs/strength-ordering/what-is-liverps.md` |
| 11 | **A, B** | Composition | Inherits sits high in LIVRPS and broadcasts overriding opinions, while specializes is the weakest arc and broadcasts fallback values that yield to any other authored opinion. | `composition-basics/strength-ordering.md` |
| 12 | **B** | Composition | Hydra does not render a class prim or its descendants, which is exactly the behavior wanted for abstract data meant to be leveraged via inherits/specializes. | `creating-composition-arcs/inherits-specializes/what-is-inherits.md` |
| 13 | **B** | Composition | Adding the same layer twice as a sublayer simply overwrites the same paths instead of creating a second instance, whereas references graft the hierarchy onto distinct destination prims. | `creating-composition-arcs/references-payloads/references-faq.md` |
| 14 | **A, B** | Composition | An encapsulated prim is a descendant of the referenced prim and is brought along; a material that is a descendant keeps its binding, whereas an unencapsulated target breaks. | `creating-composition-arcs/encapsulation/what-is-encapsulation.md` |
| 15 | **B** | Composition | A reference may include an explicit prim path to graft that specific prim; omitting the path is only valid when the layer has a defaultPrim to use as the entry point. | `creating-composition-arcs/references-payloads/what-are-references.md` |
| 16 | **A, B** | Composition | Variant selections can be authored on a referencing layer so each reference picks its own option, and only the selected variant's opinions are composed. | `composition-basics/variant-sets.md` |
| 17 | **A, B** | Data Exchange | File format plugins can be bidirectional and can compose non-file-based sources (databases, procedural content). The source can remain the source of truth (not discarded/converted), and they can be used as standalone converters with tools like usdcat. Up-axis is unrelated. | `data-exchange/data-exchange/what-is-data-exchange.md` |
| 18 | **A** | Data Exchange | A script, executable, or microservice dedicated to translating to/from another format is a standalone converter. Unlike importers/exporters with clear directionality, a converter may be one-way or two-way, so the developer should document what it does. | `data-exchange/data-exchange/what-is-data-exchange.md` |
| 19 | **C** | Data Exchange | Because USD lacks a native struct type, namespace-prefixed attributes are the standard convention for grouping related properties from compound/grouped source data (structs, records, grouped fields). | `data-exchange/data-extraction/what-is-data-extraction.md` |
| 20 | **A** | Data Exchange | OBJ has no direct roughness concept, so the conceptual mapping derives a UsdPreviewSurface roughness input from OBJ shininess. It is not a color-space, metallic, or identifier operation. | `data-exchange/data-extraction/exercise-extracting-materials.md` |
| 21 | **B** | Data Exchange | The material is a container for the material graph; CreateSurfaceOutput().ConnectToSource(...) connects the shader's surface output up to the material's surface output. CreateIdAttr sets the shader type, MaterialBindingAPI binds the material to a mesh, and CreateInput sets a shader parameter. | `data-exchange/data-extraction/exercise-extracting-materials.md` |
| 22 | **C** | Data Exchange | Binding requires applying the MaterialBindingAPI schema to the target mesh prim, then calling Bind(material) on the returned binding API object. The other forms are not the API used in the exercise. | `data-exchange/data-extraction/exercise-extracting-materials.md` |
| 23 | **A, B** | Data Exchange | Extraction is largely source-specific, while transformation operates on already-USD data and can be designed for reuse across exporters. The other options invert or contradict this. | `data-exchange/data-transformation/what-is-data-transformation.md` |
| 24 | **B** | Data Exchange | For round-trip workflows you should minimize transformations; extensive transforms make it hard to reverse changes accurately on reimport and can cause loss or inconsistency. | `data-exchange/data-transformation/what-is-data-transformation.md` |
| 25 | **C** | Data Exchange | USDZ is an atomic zipped archive used to package and ship a completed asset together with its dependencies (e.g., a mesh with its textures). It is generally read-only and optimal for XR delivery; you would not use it while still editing. | `stage-setting/usd-file-formats.md` |
| 26 | **B** | Data Exchange | The lesson explicitly notes that the API for upAxis and metersPerUnit geometric stage metadata is found in UsdGeom, not on Usd.Stage where one might first expect it. | `data-exchange/asset-validation/exercise-asset-validation-testing.md` |
| 27 | **A** | Pipeline Development | Reference/payload pairs create boundaries between an asset's lightweight entry-point interface and its complex contents, accelerating read/write speeds - this is the Performance principle. | `asset-structure/asset-structure-principles/why-necessary.md` |
| 28 | **A, B** | Pipeline Development | Navigability is served by using relationships and collections for discoverability and by keeping model-hierarchy boundaries shallow and consistent. Reference/payload is Performance, anchored paths are Modularity, and ASCII/UTF-8 identifiers are Legibility. | `asset-structure/asset-structure-principles/why-necessary.md` |
| 29 | **A** | Pipeline Development | Scope prims are best for organizational primitives because they carry no additional semantics like the transform operations an Xform has; this avoids ambiguous semantics (e.g., what it means for a Sphere to be parented to a Material) and namespace collisions. | `asset-structure/asset-structure-principles/asset-interface-pt3.md` |
| 30 | **A** | Pipeline Development | The exercise sorts prims by type using IsA checks: `prim.IsA(UsdGeom.Mesh)` reparents under the Geometry scope and `prim.IsA(UsdShade.Material)` reparents under the Looks scope. | `asset-structure/asset-structure-principles/organizing-prim-hierarchy.md` |
| 31 | **A, B** | Pipeline Development | A single entry point is typically specified by the root layer's `defaultPrim` metadata. Beyond the root interface layer, key descendant prims marked stable for downstream overrides (materials, subcomponents) are also part of the asset's interface. Domains may introduce other entry-point identifiers (e.g. `renderSettingsPrimPath`), and library assets can have many entry points. | `asset-structure/asset-structure-principles/asset-interface-pt1.md` |
| 32 | **A** | Pipeline Development | Partitioning synthetic-data simulation across multiple processes/machines and stitching results with a layer stack is a computational workstream; these can be dynamic and may not stay consistent from one evaluation to the next. | `asset-structure/workstreams/modeling-workstreams.md` |
| 33 | **A** | Pipeline Development | Layer stacks manage complexity but are not a substitute for version control. The lesson warns to avoid modeling workstreams in layer stacks that grow procedurally over time, since there is a cost to resolving and opening each layer. | `asset-structure/workstreams/modeling-workstreams.md` |
| 34 | **A, B** | Pipeline Development | Inexpensive, important fields such as variant sets and inheritable properties are lofted above the payload, and `UsdModelAPI`/`UsdGeomModelAPI` fields like `extentsHint` (which enable previewing payloaded content before loading) are good lofting candidates. Heavy data like face indices, full shading networks, and dense point arrays are exactly what stays in the payload. | `asset-structure/reference-payload-pattern/what-is-ref-payload-pattern.md` |
| 35 | **A** | Pipeline Development | Each variant on the new lofted variant set has just one opinion: it selects its matching variant from the `exteriorType` variant set on the Looks prim, so the entry-point control drives the nested set. | `asset-structure/reference-payload-pattern/lofting-variant-sets.md` |
| 36 | **A** | Pipeline Development | Collections and relationships at the asset entry point indicate membership and roles of certain prims; highlighting practical lights in a collection at the entry point makes them easy to find and interact with downstream, without relying on rigid naming conventions. | `asset-structure/asset-parameterization/what-is-asset-parameterization.md` |
| 37 | **A, B** | Data Modeling | The relationships lesson states a relationship has no data type and records linkage, and that relationships can have multiple targets for grouping. Relationships cannot be time-sampled (metadata/links are non-animatable), attributes hold typed values not paths, and connecting attributes uses attribute connections, not relationships. | `stage-setting/properties/relationships.md` |
| 38 | **A** | Data Modeling | The units lesson lists timeCodesPerSecond as automatically handled: USD automatically scales time samples during value resolution so animation plays back correctly relative to the root layer's rate. This contrasts with metersPerUnit, which is manual. | `beyond-basics/units.md` |
| 39 | **A** | Data Modeling | The time codes lesson states that when an attribute is evaluated at a time code between authored samples, the value is linearly interpolated from the surrounding time samples for smooth animation playback. | `stage-setting/timecodes-timesamples.md` |
| 40 | **A** | Data Modeling | The time codes lesson uses UsdStage.SetStartTimeCode and UsdStage.SetEndTimeCode to set the start and end time code metadata that establish the stage's timeline. | `stage-setting/timecodes-timesamples.md` |
| 41 | **A** | Data Modeling | The schemas lesson states concrete schemas provide both a name and a typeName and can be instantiated (e.g. UsdGeomMesh), while abstract (non-concrete) schemas provide a name but no typeName and serve as base classes (e.g. UsdGeomPointBased). | `scene-description-blueprints/schemas.md` |
| 42 | **A** | Data Modeling | Enumerated labels from a fixed vocabulary use the Token type, which is the convention in USD (interpolation values are UsdGeom.Tokens). String is for free-form text. Picking the right value type for the data is the modeling principle stressed in the curriculum. | `beyond-basics/custom-properties.md` |
| 43 | **A** | Data Modeling | The attributes/schemas lessons explain that fallbacks apply only when nothing is authored: Get returns the schema fallback (1) until a value is authored, after which value resolution returns the authored value (10). | `stage-setting/properties/attributes.md` |
| 44 | **A, B** | Data Modeling | The stage lesson states CreateNew makes the passed file the stage's root layer, while CreateInMemory creates a stage whose root layer is in memory until Export is called. CreateInMemory does not write to disk, CreateNew supports multiple formats, and the two are not aliases. | `stage-setting/stage.md` |
| 45 | **A** | Data Modeling | The primvars lesson lists the need to inherit attributes down namespace to allow sparse authoring of shareable data as a core problem primvars solve, i.e. primvar inheritance. | `beyond-basics/primvars.md` |
| 46 | **A, B** | Debugging & Troubleshooting | Default Traverse() yields active, LOADED, defined, non-abstract prims, so an unloaded payload's contents (not composed) are skipped. Unloading is non-destructive; loading the payload restores the prims. Traverse() does compose referenced prims normally, and unloading does not delete layers. | `beyond-basics/stage-traversal.md` |
| 47 | **A** | Debugging & Troubleshooting | layer.Reload() reloads from the backing store, discarding unsaved in-memory opinions and returning the layer to its on-disk state. layer.Save() persists edits rather than discarding them; muting only excludes a layer from composition; SetActive prunes prims, not edits. | `beyond-basics/value-resolution.md` |
| 48 | **A** | Debugging & Troubleshooting | The lesson explicitly cautions that boolean operators (and/or/not) will NOT combine traversal predicates as intended; you must use bitwise operators (&, \|, ~). The example shows `Usd.PrimIsActive & Usd.PrimIsLoaded`. | `beyond-basics/stage-traversal.md` |
| 49 | **A** | Debugging & Troubleshooting | The lesson shows obtaining the iterator with iter() and calling Usd.PrimRange.PruneChildren() to skip all children of a visited prim while still visiting the prim itself. PruneChildren is exposed in Python and is the targeted way to skip a branch. | `beyond-basics/stage-traversal.md` |
| 50 | **A** | Debugging & Troubleshooting | SetActive(False) / active=false is a non-destructive prune: the prim is excluded from default traversal and all of its descendants' scene description is not composed. The inactive state can be overridden by a stronger layer opinion setting active=true, reactivating the subtree. | `beyond-basics/active-inactive-prims.md` |
| 51 | **A, B** | Debugging & Troubleshooting | TfDebug toggles named debug flags to surface verbose internal diagnostic/trace output from USD subsystems. TfMallocTag is the memory profiler that attributes allocations to tagged scopes, ideal for finding allocation hot spots. ChangeBlock batches edits and PruneChildren skips traversal branches; neither is a diagnostic logger or memory profiler. | `beyond-basics/value-resolution.md` |
| 52 | **A** | Debugging & Troubleshooting | Composition (the Pcp phase) builds the cached per-prim index of sources; failures there — such as unresolvable reference/payload targets or arc cycles — surface as Pcp composition errors. This is distinct from value resolution, which runs later on each Get(). | `creating-composition-arcs/prim-composition.md` |
| 53 | **A** | Debugging & Troubleshooting | Dictionaries such as customData combine per key: a key present in the stronger layer wins for that key, while keys only present in a weaker layer persist. So author=bob (strong) and dept=fx (only in weak) both appear. The first-listed sublayer is strongest. | `beyond-basics/value-resolution.md` |
| 54 | **B** | Content Aggregation | Component, group, and assembly inherit from the abstract base "model" (assembly being a subkind of group). Subcomponent is the outlier and does not inherit from model. | `beyond-basics/model-kinds.md` |
| 55 | **C** | Content Aggregation | The Kind library is extensible via plugin info, but the rules governing model hierarchy are core to composition. Mixing internal taxonomies risks invalid hierarchies, so the guidance is to favor custom properties, user properties, or schemas and rarely expose taxonomies to Kind. | `asset-structure/model-hierarchy/model-hierarchy-considerations.md` |
| 56 | **B** | Content Aggregation | Prototypes are a runtime data model and are not editable. Authoring an over on the synthetic __Prototype_01 path produces a dangling override completely unrelated to the actual prototype. | `asset-modularity-instancing/refining-scenegraph-instances/scenegraph-instance-refinement.md` |
| 57 | **B, C** | Content Aggregation | Both ad hoc arc addition and variant-set selection change composition, so OpenUSD creates a new prototype while the instance count stays the same. Primvar refinement creates no new prototype; deinstancing removes instances rather than adding a prototype. | `asset-modularity-instancing/refining-scenegraph-instances/scenegraph-variant-set-refinement.md` |
| 58 | **A** | Content Aggregation | The exercise reports the instanced stage at 1,711 prims, 1,450 instances, and 3 prototypes (one per distinct component composition: the box, the pallet, and the rack), down from 44,408 prims. | `asset-modularity-instancing/authoring-scenegraph-instancing/exercise-authoring-scenegraph-instancing.md` |
| 59 | **B** | Content Aggregation | DeactivateId authors inactiveIds metadata that prunes the point entirely. invisibleIds is for animating instances on/off over time; positions and SetInstanceable are unrelated to pruning a single point. | `asset-modularity-instancing/refining-point-instances.md` |
| 60 | **B, C** | Content Aggregation | All ancestors of a component must be group or a subkind of group (assembly), and a component cannot contain another component model, only subcomponents. B (subcomponent under component) and C (assembly nested in assembly, then component) are valid. A nests a component under a component; D roots a tree at a component with a group/component below it. | `asset-structure/model-hierarchy/what-are-model-kinds.md` |
| 61 | **A** | Visualization | UsdLuxDistantLight emits from a distant source along the -Z axis and is commonly known as a directional light. | `scene-description-blueprints/lights.md` |
| 62 | **A** | Visualization | UsdLux lights are Xformable; the lesson explicitly notes 'Lights are Xformable' and positions/rotates them using XformCommonAPI (SetRotate on the distant light, SetTranslate on the sphere light). | `scene-description-blueprints/lights.md` |
| 63 | **A** | Visualization | Lights expose common emissive attributes accessed via GetColorAttr() (light color) and GetIntensityAttr() (light intensity), as shown setting the distant and sphere light properties. | `scene-description-blueprints/lights.md` |
| 64 | **A, B** | Visualization | The render-purpose prim's proxyPrim relationship targets the proxy-purpose prim; ComputeProxyPrim() returns the prim that should be drawn in preview. proxyPrim is a relationship (not a float attribute), and 'proxy' purpose is for preview, not final render. | `stage-setting/properties/relationships.md` |
| 65 | **A** | Visualization | The primvars example shows vertex interpolation stores one value per point and interpolates across the primitive's surface; constant is one value for the whole gprim and uniform is one value per face. | `beyond-basics/primvars.md` |
| 66 | **A** | Visualization | The mesh examples create topology via CreatePointsAttr (points), CreateFaceVertexCountsAttr (faceVertexCounts), and CreateFaceVertexIndicesAttr (faceVertexIndices), which together define the vertices and face connectivity. | `beyond-basics/primvars.md` |
| 67 | **A** | Customizing USD | The lesson states single-apply API schemas are applied to only a single instance of a prim, and multiple-apply API schemas can be applied multiple times to the same prim with different instance names. Neither type of API schema sets a typeName. | `scene-description-blueprints/schemas.md` |
| 68 | **A** | Customizing USD | The lesson states API schemas do not assign a typeName; instead they are list-edited in the apiSchemas metadata and are queryable via the HasAPI method, annotating already-typed prims with additional properties. | `scene-description-blueprints/schemas.md` |
| 69 | **A** | Customizing USD | IsA (typed) schemas tell a prim what it is and assign the typeName metadata; each prim subscribes to one IsA schema. API schemas do not set a typeName and instead annotate already-typed prims. To define a new prim type you author an IsA schema. | `scene-description-blueprints/schemas.md` |
| 70 | **A** | Customizing USD | The lesson notes a trend toward codeless schemas for easier distribution, suggesting schemas become more lightweight, focusing on data modeling rather than behavior implementation. Codeless schemas require no compiled C++ to be loaded and used. | `scene-description-blueprints/schemas.md` |

> **Sourcing:** Authored for this repository to extend self-study — **not** official exam questions. Each item is grounded in the cited [Learn OpenUSD](https://docs.nvidia.com/learn-openusd/latest/index.html) lesson (the `docs/…` source of the corresponding published page). Domain weights and the study plan are in [`EXAM_PREP.md`](./EXAM_PREP.md); more practice in [`PRACTICE_QUESTIONS_EXTRA.md`](./PRACTICE_QUESTIONS_EXTRA.md).
