# OpenUSD Development Certification — Simulation Exam 3

A full-length **70-question** mock exam. The domain mix mirrors the *NCP: OpenUSD Development Exam Study Guide v1.1.0* weights (see [`EXAM_PREP.md`](./EXAM_PREP.md)). This is **Exam 3 of 3** — see also Exams [1](./SIMULATION_EXAM_1.md), [2](./SIMULATION_EXAM_2.md).

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

During composition, an opinion authored directly in a layer or any of its recursive sublayers (with no other composition arc involved) is classified as which LIVRPS component?

- **A.** Inherits
- **B.** Local
- **C.** References
- **D.** Specializes

---

## Question 2 — Composition

Within a single layer stack, LIVRPS orders these arcs from STRONGEST to WEAKEST. Which three orderings are correct? (Select three.)

- **A.** Inherits are stronger than variant sets
- **B.** Variant sets are stronger than references
- **C.** References are stronger than payloads
- **D.** Payloads are stronger than inherits
- **E.** Specializes is stronger than references

---

## Question 3 — Composition

Two sublayers `workstreamA.usd` (weaker, listed later) and `workstreamB.usd` (stronger, listed earlier) are composed. For a property, workstreamA has a local opinion but workstreamB and the root layer have none. Which opinion wins, and why?

- **A.** workstreamB, because it is the stronger sublayer regardless of whether it authors an opinion
- **B.** workstreamA, because the stronger layers have no opinion for that property and only workstreamA contributes one
- **C.** Neither; a missing opinion in the stronger layer blocks the weaker one
- **D.** The root layer always wins by default

---

## Question 4 — Composition

You author a value directly on a prim in the root layer (a local opinion), and the same prim also has a reference whose target authors a different value. Why does the local opinion win?

- **A.** Because references are not composed unless explicitly loaded
- **B.** Because Local is the strongest component in LIVRPS, stronger than References
- **C.** Because the reference target's defaultPrim is unset
- **D.** Because root-layer opinions are ignored during composition

---

## Question 5 — Composition

A USD layer is brought in as a reference but has NO defaultPrim set and the reference omits an explicit prim path. What is the result?

- **A.** The first def prim alphabetically is used as the entry point
- **B.** It resolves as an empty layer and outputs a warning message in the log
- **C.** All top-level prims are brought in automatically
- **D.** USD raises a fatal error and aborts stage opening

---

## Question 6 — Composition

Which two statements about specifiers and default stage traversal are correct? (Select two.)

- **A.** Default traversals visit only defined (def), non-abstract prims
- **B.** Prims that resolve to a class specifier are composed on the stage but skipped by default traversals
- **C.** Class prims are never present on the composed stage
- **D.** An over specifier always forces a prim to be traversed regardless of what it resolves to

---

## Question 7 — Composition

The resolved specifier of `over "Box"` is determined by composition. If the strongest contributing opinion for that prim across all layers is a `def`, what specifier does the prim resolve to, and is it traversed?

- **A.** It resolves to over and is skipped by default traversals
- **B.** It resolves to def and is visited by default traversals, because over resolves to either a def or class
- **C.** It resolves to class and is skipped by default traversals
- **D.** It resolves to over and remains abstract

---

## Question 8 — Composition

Both a variant set and a reference on the same prim author a conflicting value for one attribute. According to LIVRPS, which opinion is stronger?

- **A.** The reference, because references graft entire layer stacks
- **B.** The variant, because variant sets (V) are stronger than references (R) in LIVRPS
- **C.** The reference, because it is loaded after the variant
- **D.** They tie and USD picks randomly

---

## Question 9 — Composition

Which two things can a variant within a variant set do, according to the Variant Sets lesson? (Select two.)

- **A.** Sparsely override properties on the prim or any descendant prim
- **B.** Define new descendant prims
- **C.** Change the layer's #usda version header
- **D.** Delete the prim that defines the variant set from disk

---

## Question 10 — Composition

Why does the lesson note say changing VISIBILITY is faster than changing ACTIVATION when switching variants?

- **A.** Visibility deletes prims while activation only hides them
- **B.** Visibility is purely a rendering concept that does not affect composition, while activation determines whether prims are composed onto the stage at all
- **C.** Activation is cached on disk but visibility is not
- **D.** There is no performance difference; both rebuild the composition index

---

## Question 11 — Composition

A prim is composed from prim specs and property specs authored across layers. What are the authored values stored in those specs called, and what API family lets you interact with them?

- **A.** They are called 'opinions', and you interact with specs via the SdfSpec API (prim spec and property spec each have their own derived API)
- **B.** They are called 'arcs', and you interact with them only through UsdReferences
- **C.** They are called 'stages', accessed through UsdStage only
- **D.** They are called 'layers', accessed through the Gf math API

---

## Question 12 — Composition

In an UNENCAPSULATED asset, a cube under `/World` binds to a material in a sibling `/Looks` scope that is NOT under `/World`. When another stage references `/World`, what is the observed problem?

- **A.** The cube is brought over but the material binding no longer works because the material does not exist in the new context
- **B.** Both the cube and the material are brought over and the binding works fine
- **C.** The reference brings in /Looks automatically as a sibling
- **D.** The stage fails to open

---

## Question 13 — Composition

Which two statements about layer stacks are correct? (Select two.)

- **A.** Composition operations such as references and payloads are evaluated in the context of a layer stack, not an individual layer
- **B.** A layer stack also includes local composition arcs like internal references, variant sets, inherits, and specializes
- **C.** A layer stack is limited to exactly one layer with no sublayers
- **D.** Within a layer stack the last sublayer is the strongest

---

## Question 14 — Composition

Payload composition arcs can be applied to the same prim in a list-editable way, just like references. Where do payloads sit relative to references in LIVRPS strength?

- **A.** Payloads are stronger than references
- **B.** Payloads are weaker than references
- **C.** Payloads and references have identical strength
- **D.** Payloads are stronger than local opinions

---

## Question 15 — Composition

You want to add a 'lights' variant set with 'on' and 'off' variants to a street-lamp prim and author different intensity/emissive values for each. Which Python pattern correctly authors the 'on' variant's data?

- **A.** vset = root.GetVariantSets().AddVariantSet('lights'); vset.AddVariant('on'); vset.SetVariantSelection('on')
with vset.GetVariantEditContext():
    toggle_lights(True)
- **B.** vset = root.GetReferences().AddVariantSet('lights'); vset.AddVariant('on'); toggle_lights(True)
- **C.** root.SetVariant('lights', 'on'); toggle_lights(True)
- **D.** with root.GetInherits().AddInherit('on'):
    toggle_lights(True)

---

## Question 16 — Composition

Which sequence of Python calls retrieves the recursive sublayers list of a given layer through the Sdf API?

- **A.** root_layer = stage.GetRootLayer(); sublayers = root_layer.subLayerPaths
- **B.** root_layer = stage.GetRootLayer(); sublayers = root_layer.GetReferences()
- **C.** sublayers = stage.GetVariantSets().subLayerPaths
- **D.** sublayers = stage.GetDefaultPrim().subLayerPaths

---

## Question 17 — Data Exchange

According to the two-phase data exchange model, what is the primary goal of the EXTRACT phase?

- **A.** To restructure the content to match the destination organization's asset structure
- **B.** To translate the source data to OpenUSD as directly as possible, preserving fidelity and mapping concepts faithfully
- **C.** To apply user export options such as up-axis selection
- **D.** To merge meshes and prune irrelevant data for performance

---

## Question 18 — Data Exchange

Which TWO activities belong to the TRANSFORM phase rather than the EXTRACT phase?

*(Select two options.)*

- **A.** Applying user export options
- **B.** Implementing optimizations such as mesh merging
- **C.** Faithfully mapping source vertices and faces to USD points and faceVertexIndices
- **D.** Setting subdivisionScheme to none to respect a polygonal source
- **E.** Sanitizing source names into valid USD identifiers

---

## Question 19 — Data Exchange

The lessons cite the USD and MaterialX concept mapping as an example of which practice?

- **A.** Conceptual data mapping (analyzing how data models from one format map to another)
- **B.** Transient data caching during conversion
- **C.** Asset validation with usdchecker
- **D.** Packaging assets into USDZ for delivery

---

## Question 20 — Data Exchange

When converting between formats such as FBX and glTF, intermediate texture maps, shader definitions, or color values may be produced that are not necessarily written to the final output. The lessons describe this as an example of what?

- **A.** A schema gap requiring a new OpenUSD schema
- **B.** Transient data from the extraction phase that can be useful (and optionally cached) during transformation
- **C.** A mandatory export option
- **D.** A round-trip reversibility violation

---

## Question 21 — Data Exchange

An OBJ file stores one global vertex list shared across all objects, while OpenUSD tracks vertices on a per-mesh basis. In the obj2usd exercise, who handles mapping the OBJ global vertices into per-mesh vertices?

- **A.** usdchecker, during validation
- **B.** Assimp, which already maps the global vertices of OBJ to per-mesh
- **C.** The transform() function, via NamespaceEditor
- **D.** Tf.MakeValidIdentifier, during name sanitization

---

## Question 22 — Data Exchange

What is usdchecker, and what does it validate an asset against?

- **A.** A USD Toolset command-line tool that validates a stage or USDZ package against rules for interchange and Hydra renderability
- **B.** A Python library that automatically fixes invalid USD and adds custom rules via a GUI
- **C.** A converter that exports USD to glTF for web delivery
- **D.** A profiler that measures USDC load times versus USDA

---

## Question 23 — Data Exchange

The obj2usd converter output faithfully reproduces OBJ's flat hierarchy with multiple prims directly under the pseudo-root. Why does the transformation step add a /World prim and reparent everything under it?

- **A.** To compress the geometry into USDC for faster loading
- **B.** So the prims share a common ancestor, making the entire asset easy to reference, and to allow defaultPrim to be set
- **C.** To convert the material binding into a collection-based binding
- **D.** To change the up-axis from Y to Z

---

## Question 24 — Data Exchange

Which TWO statements about USDA versus USDC trade-offs are correct per the lessons?

*(Select two options.)*

- **A.** USDC is a compressed binary format designed to minimize load time and is extremely efficient for numerically-heavy data like geometry
- **B.** USDA is human-readable text, making it ideal for manual editing, inspection, and diffing
- **C.** USDA loads faster and uses less memory than USDC for heavy geometry
- **D.** USDC files cannot be referenced by other layers
- **E.** USDA uses memory mapping to accelerate file access

---

## Question 25 — Data Exchange

Most end users expect a single-file export by default, yet the lessons note that in practice most organizations have an established asset structure the export must fit. If an export is just one workstream of a larger asset structure, what should the exporter be able to do?

- **A.** Always overwrite the existing asset structure with a flattened single file
- **B.** Distill the extraction down to that workstream's unique contributions, defining new prims, applying overrides on existing prims, or both
- **C.** Refuse to export until the user converts the source to USDZ
- **D.** Discard all extracted data and re-run extraction per workstream

---

## Question 26 — Data Exchange

Even the native USD formats (.usda, .usdc, .usd, .usdz) are implemented as which mechanism, illustrating that any data provider can implement support to natively speak USD?

- **A.** Standalone converters
- **B.** File format plugins
- **C.** Importers
- **D.** Hydra render delegates

---

## Question 27 — Pipeline Development

A pipeline architect anchors an asset's local dependencies with relative/anchored paths, defines a stable entry-point interface, and keeps versioned instances atomic via storage deduplication so the asset can be reused and iteratively improved. Which principle of scalable asset structure do these practices serve?

- **A.** Modularity
- **B.** Legibility
- **C.** Performance
- **D.** Navigability

---

## Question 28 — Pipeline Development

The curriculum stresses that there is no single best way to structure an OpenUSD asset. Which TWO statements reflect its guidance on what an asset is and how to structure one?

*(Select two options.)*

- **A.** An asset is a named, versioned, and structured container of one or more resources, which may include composable USD layers, textures, volumetric data, and more
- **B.** A well-designed asset structure should be tailored to the needs of clients and collaborators, since usage and domains vary
- **C.** An asset must be a single flattened `.usdc` file with no external dependencies
- **D.** An asset is any prim that has been marked `instanceable = true` on a stage
- **E.** There is one universally best asset structure that all pipelines should adopt

---

## Question 29 — Pipeline Development

In this library-asset interface layer, how should the prims be characterized?

```usda
#usda 1.0
def Scope "World" {
  def "City" (references = @uri:/project/city.usd@) {}
  def "TaxiCab" (references = @uri:/project/taxi_cab.usd@) {}
}
```

- **A.** A `Scope` is used to group multiple independent entry points (`City`, `TaxiCab`), each of which can be referenced individually downstream
- **B.** `World` is the single asset entry point and `City`/`TaxiCab` are internal-only prims that must not be referenced
- **C.** The Scope makes `City` and `TaxiCab` instanceable prototypes that cannot be overridden
- **D.** This is invalid because a Scope cannot contain prims that carry reference arcs

---

## Question 30 — Pipeline Development

The curriculum notes there is "no general utility for lofting" fields above a payload. What does this imply for how a pipeline should implement lofting at publish time?

- **A.** Lofting is typically achieved through site- or project-specific post-scripts associated with asset generation and publishing
- **B.** Lofting can only be done manually by hand-editing each USDA file in a text editor
- **C.** Lofting is automatically performed by OpenUSD whenever a payload is authored
- **D.** Lofting requires flattening the entire stage before any field becomes accessible

---

## Question 31 — Pipeline Development

A surfacing artist's tool re-exports the entire asset on every save, rewriting the geometry opinions that the modeler already authored, instead of writing only material and binding opinions. According to the workstreams lesson, what is the problem with this, and what is the correct practice?

- **A.** Each workstream should author only its unique contributions sparsely; needlessly overwriting another workstream's work causes conflicts and blocks parallel iteration
- **B.** Re-exporting everything is correct because each layer must be self-contained to compose
- **C.** The surfacing layer should be a reference, not a sublayer, so overwriting geometry is harmless
- **D.** Geometry and shading must always live in a single layer to guarantee correct binding

---

## Question 32 — Pipeline Development

The Legibility principle advises avoiding identifiers that complicate storage deduplication and discourages constructs that cause conceptual overload. Which TWO of the following align with the curriculum's Legibility / Performance guidance on naming and identifiers?

*(Select two options.)*

- **A.** Prefer straightforward ASCII or UTF-8 identifiers for prim, property, and resource names
- **B.** Avoid baking timestamps, UUIDs, and version numbers into layer names that might complicate storage deduplication
- **C.** Embed composition arcs into every prim name to make arcs self-documenting
- **D.** Use deep, deeply-nested model-hierarchy boundaries to encode full provenance
- **E.** Localize every Crate layer with a mirroring resolver before reading to speed up I/O

---

## Question 33 — Pipeline Development

The curriculum states that, unless explicitly documented or annotated as internal, variants and primvars authored on an asset entry point should be treated a certain way by downstream contexts. How?

- **A.** As "public" and safe to edit and set, with an expectation of stability
- **B.** As read-only and never overridable, since the entry point is locked
- **C.** As internal scaffolding that downstream users should ignore
- **D.** As deprecated unless re-declared in the consuming layer

---

## Question 34 — Pipeline Development

A team weighs parameterizing a single continuous tint knob as a primvar on the entry point versus a variant set. The lesson frames this as a trade-off. What does it identify as the cost of choosing primvars for a single property?

- **A.** Additional lookups in materials, in exchange for upfront memory savings
- **B.** The creation of a new instancing prototype per primvar value
- **C.** Loss of the ability to inherit the value down the prim hierarchy
- **D.** A mandatory payload reload every time the primvar changes

---

## Question 35 — Pipeline Development

A layer might contribute synthesized motion authored by a procedural process on top of a hand-authored initial state created by a user. How does the curriculum classify this combination of workstreams?

- **A.** A hybrid workstream, combining both computational and user-driven elements
- **B.** Purely a computational workstream, since motion is procedural
- **C.** Purely a user workstream, since a human created the initial state
- **D.** A version-control workstream that bypasses the layer stack

---

## Question 36 — Pipeline Development

The asset-interface lesson advises content authors to "embed context directly into assets" through embedded-context conventions. What is the stated purpose of doing this?

- **A.** To hint to users that these assets are intended to be included by reference
- **B.** To force the asset to flatten its composition arcs on load
- **C.** To prevent the asset from ever being opened as a top-level stage
- **D.** To automatically generate variant sets for every embedded prim

---

## Question 37 — Data Modeling

Given the property path /World/Geom/Sphere.userProperties:tag, which method extracts /World/Geom/Sphere (the owning prim path) from the Sdf.Path?

- **A.** path.GetPrimPath()
- **B.** path.AppendProperty()
- **C.** path.AppendChild()
- **D.** path.IsPrimPath()

---

## Question 38 — Data Modeling

A stage has animation. You call attr.Get() with no time code on an animated attribute and unexpectedly get a single static value. Which call reliably returns the first authored animated value instead?

- **A.** attr.Get(Usd.TimeCode.EarliestTime())
- **B.** attr.Get(Usd.TimeCode.Default())
- **C.** attr.Get() with no arguments, which already returns animation.
- **D.** attr.GetMetadata('timeSamples')

---

## Question 39 — Data Modeling

Per the value-resolution lesson, which TWO statements are correct?

*(Select two options.)*

- **A.** Composition is cached (indexed at the prim level), but value resolution is not pre-calculated.
- **B.** An attribute has up to three value sources at a location: value clips, time samples, and a default value.
- **C.** Value resolution is cached while composition is recomputed on every Get().
- **D.** Attributes have exactly one value source: the default value.
- **E.** Metadata always resolves by merging dictionaries regardless of type.

---

## Question 40 — Data Modeling

A USD file does not author upAxis. What up axis does OpenUSD assume by fallback?

- **A.** Y
- **B.** Z
- **C.** X
- **D.** There is no fallback; an error is raised.

---

## Question 41 — Data Modeling

Which API and metadata field define the mass/density scale (kilograms per unit) used for physics simulations on a stage?

- **A.** kilogramsPerUnit, set via UsdPhysics.SetStageKilogramsPerUnit(stage, value)
- **B.** metersPerUnit, set via UsdGeom.SetStageMetersPerUnit(stage, value)
- **C.** timeCodesPerSecond, set via stage.SetTimeCodesPerSecond(value)
- **D.** kilogramsPerUnit, set via UsdGeom.SetStageUpAxis(stage, value)

---

## Question 42 — Data Modeling

Which TWO statements about OpenUSD native file formats match the curriculum?

*(Select two options.)*

- **A.** .usdz is an atomic, zipped package archive for delivering assets together (e.g. a mesh with its textures), generally read-only and optimal for XR.
- **B.** A .usd file can be either ASCII or binary, so the underlying format can change without breaking references.
- **C.** .usda is the compressed Crate binary format optimized for numerically-heavy geometry data.
- **D.** .usdc is the human-readable text format best suited for manual editing and diffing.
- **E.** .usdz files are the recommended format while you are still actively editing an asset.

---

## Question 43 — Data Modeling

You are mapping a compound IoT sensor record (temperature, humidity, pressure) onto a single prim as custom attributes. Which TWO practices follow the custom-properties lesson?

*(Select two options.)*

- **A.** Use nested namespace prefixes such as acme:sensor:temperature to identify the org and group related properties.
- **B.** Author each attribute with custom=True to flag that it is not part of an existing schema.
- **C.** Combine all three readings into one relationship target list.
- **D.** Store them as a single unnamed attribute since USD has a native struct type.
- **E.** Set the prim's typeName to 'sensor' to register the fields.

---

## Question 44 — Data Modeling

After updating a mesh's points to deform its geometry over time, which attribute should also be updated so the prim's bounding box stays correct?

- **A.** extent — the attribute that defines the boundaries of a geometric prim.
- **B.** displayColor — the per-vertex color primvar.
- **C.** purpose — the imageable render/proxy purpose token.
- **D.** typeName — the prim's schema type.

---

## Question 45 — Data Modeling

A texture coordinate primvar should produce smooth interpolation across each face, with a distinct value at every corner of every face (so shared points can hold different UVs per face). Which interpolation token applies?

- **A.** faceVarying — one value per face-corner.
- **B.** vertex — one value per point.
- **C.** uniform — one value per face.
- **D.** constant — one value for the whole gprim.

---

## Question 46 — Debugging & Troubleshooting

In a layer stack with root.usda having sublayers [workstreamB.usda, workstreamA.usda], a property resolves to workstreamA's value even though workstreamA is the weaker sublayer (listed later). Tracing through LIVRPS, what explains this?

- **A.** workstreamA holds a Local opinion for that property while root and workstreamB do not; a Local opinion anywhere in the layer stack outranks the absence of an opinion in stronger layers.
- **B.** Sublayer order is ignored for properties; only prim order matters.
- **C.** workstreamA must contain a reference arc, which is stronger than the Local opinions in workstreamB.
- **D.** The resolved value is undefined because two sublayers conflict; USD picked one at random.

---

## Question 47 — Debugging & Troubleshooting

You suspect an opinion on /World/Box is coming from the wrong layer. You print `for spec in prim.GetPrimStack(): print(spec.layer.identifier, spec.specifier)`. How should you read this output to debug strength ordering?

- **A.** GetPrimStack() returns SdfPrimSpec handles ordered strongest to weakest; the first spec is the winning layer for strongest-wins metadata and the top of the LIVRPS-resolved stack.
- **B.** The list is ordered weakest to strongest, so the last entry is always the winning opinion.
- **C.** The order is arbitrary disk order, so it cannot be used to reason about strength.
- **D.** GetPrimStack() returns only the single winning spec, never the full contributing list.

---

## Question 48 — Debugging & Troubleshooting

A tool needs to audit EVERY prim in scene description, including class (abstract) templates and inactive prims, for a validation report. Which two statements about the right traversal choice are correct? (Select 2)

- **A.** stage.TraverseAll() visits all prims regardless of the default predicate, so it includes class/abstract and inactive prims.
- **B.** Default stage.Traverse() filters to active, loaded, defined, non-abstract prims, so it would omit the class and inactive prims the audit needs.
- **C.** stage.Traverse() already includes abstract and inactive prims by default, so TraverseAll() is unnecessary.
- **D.** GetDefaultPrim().GetAllChildren() recurses the whole subtree and is equivalent to TraverseAll().

---

## Question 49 — Debugging & Troubleshooting

An attribute has both an authored default value and time samples. A reviewer wants to verify (a) the static default and (b) the first authored animated value. Which two Get calls correctly retrieve each? (Select 2)

- **A.** attr.Get(Usd.TimeCode.Default()) returns the authored default (static, non-time-varying) value.
- **B.** attr.Get(Usd.TimeCode.EarliestTime()) returns the first authored time sample value.
- **C.** attr.Get() with no argument returns the first time sample, identical to EarliestTime().
- **D.** attr.Get(Usd.TimeCode.Default()) interpolates between the first two time samples.

---

## Question 50 — Debugging & Troubleshooting

You author over "Box" { double size = 10 } in a strong layer, trusting that Box is defined in a weaker base layer. After removing the base layer, the override no longer shows up at all in default traversals. Why does an orphaned over not appear as a concrete prim?

- **A.** An over is the weakest specifier and only holds overrides for an existing prim; with no underlying def, it does not resolve to a defined prim and so is skipped by default traversals.
- **B.** An over is automatically promoted to a def when its target is missing, so it should appear; its absence indicates a bug.
- **C.** An over always resolves to a class specifier, which is why it is hidden.
- **D.** Default traversals skip any prim named Box for safety reasons.

---

## Question 51 — Debugging & Troubleshooting

Conceptually, which OpenUSD diagnostic facility is best suited for COLLECTING and de-duplicating the many diagnostic messages USD emits during an operation, so you can review them programmatically as a batch instead of scattered to stderr?

- **A.** A diagnostic delegate such as UsdUtilsCoalescingDiagnosticDelegate, which captures and coalesces diagnostics via TfDiagnosticMgr.
- **B.** Trace, which only times function execution and cannot capture diagnostic text.
- **C.** Sdf.ChangeBlock, which suppresses all diagnostics by deferring notifications.
- **D.** UsdAttributeQuery, which caches resolved attribute values for reuse.

---

## Question 52 — Debugging & Troubleshooting

You need to find which functions in a custom traversal are the performance bottleneck during a slow stage process, capturing call timing and a profile of where time is spent. Which OpenUSD facility is intended for this?

- **A.** Trace (the TraceCollector/Trace framework) — instruments scopes to record timing and produce a performance profile.
- **B.** TfDebug — used to print verbose categorized debug messages, not to time code.
- **C.** TfMallocTag — used to profile memory allocations, not execution time.
- **D.** Sdf.ChangeBlock — used to coalesce edits, with no profiling output.

---

## Question 53 — Debugging & Troubleshooting

An asset's material relationship that worked in isolation points at the wrong target once the asset is referenced into a master stage. Investigating, you find the relationship targeted a prim OUTSIDE the referenced asset's root. Which principle explains the broken binding?

- **A.** Encapsulation: composition arcs like references encapsulate the referenced layer stack, so a relationship target outside the referenced prim's namespace does not map across the reference and the binding breaks.
- **B.** Relationships never survive references; all bindings must be re-authored after every reference.
- **C.** The target was muted by the reference, and IsLayerMuted would report it.
- **D.** References reverse relationship strength, so the weakest target wins and appears wrong.

---

## Question 54 — Content Aggregation

What is nested instancing in OpenUSD?

- **A.** Authoring multiple variant sets inside a single instanceable prim.
- **B.** When an instance subgraph itself contains instances, such as instanced assemblies whose instanced components also reuse instanced material networks.
- **C.** Placing a PointInstancer's prototypes under a nested Scope hierarchy.
- **D.** Referencing the same asset more than once without enabling instanceable.

---

## Question 55 — Content Aggregation

When promoting a point instance to a fully editable prim, why is ComputeInstanceTransformsAtTime() preferred over reading the positions attribute directly?

- **A.** It is the only way to author the inactiveIds metadata.
- **B.** It returns the complete transform matrix, combining the prototype root transform with per-instance scales, orientations, positions, and velocities.
- **C.** It supports sparse overrides of a single point in the positions array.
- **D.** It converts the PointInstancer into a scenegraph instance automatically.

---

## Question 56 — Content Aggregation

Which properties are MANDATORY on a UsdGeomPointInstancer? (Select 3)

- **A.** prototypes (relationship to the prototype prim hierarchies)
- **B.** protoIndices (int[] mapping each point to a prototype)
- **C.** positions (point3f[] location of each instance)
- **D.** orientations (quath[] per-instance rotation)

---

## Question 57 — Content Aggregation

In broadcasted refinement, an opinion authored once on a shared inherit/specialize target (e.g. a _PalletBox class) ripples to all boxes on a pallet. What happened to the prototype and instance counts in the exercise (relative to 1711 prims / 1450 instances / 3 prototypes)?

- **A.** Prototypes stayed at 3; instances dropped because boxes were deinstanced.
- **B.** A new prototype was created (3 to 4) while the instance count stayed at 1450, shared by all affected boxes.
- **C.** Both prototype and instance counts doubled.
- **D.** No new prototype was created because inherit and specialize never change composition.

---

## Question 58 — Content Aggregation

You author primvars:cleanness with vertex interpolation on a PointInstancer, but the boxes do not change appearance. Why, and what fixes it?

- **A.** Primvars cannot be authored on a PointInstancer; you must deinstance first.
- **B.** The prototype's own (descendant) primvar opinion is stronger than the instancer's ancestor opinion; Block() the prototype's primvar so the instancer's value applies.
- **C.** vertex interpolation is invalid on a PointInstancer; switch to constant interpolation.
- **D.** You must re-author the entire positions array for the primvar to take effect.

---

## Question 59 — Content Aggregation

A scene needs roughly 100,000 leaves on a tree. Which statements support choosing point instancing over scenegraph instancing here? (Select 2)

- **A.** Point instancing is designed for massive numbers of simpler items where the overhead of an instance outweighs the benefits of reuse.
- **B.** Scenegraph instancing would require defining ~100,000 individual instanceable prims (Leaf_000001, Leaf_000002, ...) in the scenegraph.
- **C.** Scenegraph instancing cannot reuse the same leaf asset more than once.
- **D.** Scenegraph instancing does not support per-instance transforms, so leaves could not be positioned.

---

## Question 60 — Content Aggregation

Which statements about deinstancing refinement (SetInstanceable(False)) are TRUE? (Select 2)

- **A.** It restores full editability of the instance's subgraph so you can author overrides on former instance proxies.
- **B.** Prim-count cost scales roughly linearly as you deinstance more copies, losing instancing optimization for each.
- **C.** It is the preferred technique when you must apply identical overrides to many copies, because it reuses one new prototype.
- **D.** It permanently removes the asset's composition arcs from the scene.

---

## Question 61 — Visualization

In the relationships example, a `UsdShade.Shader` named PreviewSurface is created with `green_ps.CreateIdAttr("UsdPreviewSurface")` and a diffuseColor input. How is the shader's output wired into the material so a renderer can use it?

- **A.** green.CreateSurfaceOutput().ConnectToSource(green_ps.ConnectableAPI(), "surface")
- **B.** green.CreateRelationship("material:binding").SetTargets([green_ps.GetPath()])
- **C.** green_ps.GetIntensityAttr().Set(green.GetPath())
- **D.** green.AddXformOp(UsdGeom.XformOp.TypeTransform, green_ps)

---

## Question 62 — Visualization

A relationship in OpenUSD differs from an attribute in an important way. Which statement is correct?

- **A.** A relationship has no data type; it records linkage by storing path values that target other prims
- **B.** A relationship stores typed numeric values just like an attribute does
- **C.** A relationship can directly connect two already-existing attributes
- **D.** A relationship can only ever target exactly one prim

---

## Question 63 — Visualization

All classes in `UsdGeom` inherit from `UsdGeomImageable`. What is the intent of UsdGeomImageable?

- **A.** To capture any prim type that might want to be rendered or visualized
- **B.** To provide shading network connection points for materials
- **C.** To store the topology (points and faces) of a mesh
- **D.** To define the emissive color and intensity of a light

---

## Question 64 — Visualization

A user wants to organize all of a scene's lights and materials into logical groups without affecting their spatial placement, and to be able to deactivate an entire group by deactivating one prim. Which prim type best fits this requirement?

- **A.** Scope, a non-transformable grouping prim used to organize related prims
- **B.** Xform, since grouping requires a transform to be applied to children
- **C.** Mesh, since organizational containers must define geometry
- **D.** Material, since lights and materials must share a shading container

---

## Question 65 — Visualization

A modeler authors a parent `UsdGeom.Xform` at `/World/Parent_Prim` with translate, rotate, and scale via XformCommonAPI, places a cone Child_A under it, and places Child_B under a separate `/World/Alt_Parent`. Which TWO statements are correct?

*(Select two options.)*

- **A.** Child_A inherits Parent_Prim's translate, rotate, and scale because it is a descendant
- **B.** Child_B does NOT inherit Parent_Prim's transforms because it lives under a different parent hierarchy
- **C.** Child_A's final placement ignores its own local SetTranslate and uses only the parent transform
- **D.** Child_B inherits Parent_Prim's transforms because all transforms are global to the stage

---

## Question 66 — Visualization

Which `UsdLux` schema represents an environment/sky light that surrounds the scene, as opposed to an area or directional light?

- **A.** UsdLuxDomeLight
- **B.** UsdLuxRectLight
- **C.** UsdLuxDistantLight
- **D.** UsdLuxCylinderLight

---

## Question 67 — Customizing USD

In a USD plugin pipeline, what is the role of usdGenSchema?

- **A.** It generates schema classes (C++/Python code and plugin definitions) from a schema definition file such as schema.usda.
- **B.** It resolves asset paths at runtime by anchoring relative paths to layers.
- **C.** It procedurally generates renderable Hydra prims from a scene index.
- **D.** It composes a foreign file format into a USD stage as a layer.

---

## Question 68 — Customizing USD

What is the primary purpose of a USD file-format plugin (an SdfFileFormat implementation)?

- **A.** It lets USD read a foreign or custom file format and compose its contents into a stage as a layer, without first converting the file to .usd on disk.
- **B.** It changes how asset identifier strings are resolved to physical locations.
- **C.** It registers new model kinds into the Kind library.
- **D.** It generates schema classes from a schema.usda definition.

---

## Question 69 — Customizing USD

A studio is packaging a custom schema and a custom asset resolver as plugins. Which TWO statements about USD plugin mechanics are correct? (Select 2)

- **A.** A plugInfo.json file, discovered by the plugin registry (PlugRegistry), declares the plugin and the types it provides so USD can load it.
- **B.** A custom Ar asset resolver lets a pipeline resolve and anchor asset paths dynamically, and can serve assets such as in-memory layers rather than only files on disk.
- **C.** Compiled plugins are version-agnostic and can be loaded by any USD build regardless of the version they were built against.
- **D.** Plugins must be embedded inside each .usda asset layer rather than registered separately.

---

## Question 70 — Customizing USD

Which two Usd.SchemaRegistry methods are the documented way to look up a registered schema's info and its typeName in Python?

- **A.** Usd.SchemaRegistry.FindSchemaInfo() and Usd.SchemaRegistry.GetSchemaTypeName()
- **B.** Usd.ModelAPI.GetKind() and Usd.ModelAPI.SetKind()
- **C.** prim.HasAPI() and prim.IsA()
- **D.** UsdGeom.Sphere.Define() and sphere.GetRadiusAttr()

---

## Answer Key

| Q | Answer | Domain | Why | Grounded in |
|---|---|---|---|---|
| 1 | **B** | Composition | A local opinion is one authored directly in a layer or any of its recursive sublayers without any additional composition, and local is the strongest LIVRPS component. | `creating-composition-arcs/strength-ordering/what-is-liverps.md` |
| 2 | **A, B, C** | Composition | LIVRPS order is Local > Inherits > Variants > References > Payloads > Specializes, so inherits>variants, variants>references, and references>payloads all hold. | `creating-composition-arcs/strength-ordering/what-is-liverps.md` |
| 3 | **B** | Composition | Strength ordering only resolves conflicts among authored opinions; since the stronger layers have no opinion, the weaker workstreamA layer's local opinion is the winning one. | `creating-composition-arcs/strength-ordering/tracing-through-liverps.md` |
| 4 | **B** | Composition | Local opinions are the L in LIVRPS and are the strongest, so a local root-layer opinion outranks the value contributed through a reference arc. | `composition-basics/strength-ordering.md` |
| 5 | **B** | Composition | If no defaultPrim is set and no path is specified, the referenced/payloaded layer resolves as an empty layer and emits a warning in the log. | `composition-basics/default-prim.md` |
| 6 | **A, B** | Composition | Default traversals visit only def, non-abstract prims; class prims are present and composed but ignored by default traversals such as rendering. | `composition-basics/specifiers.md` |
| 7 | **B** | Composition | An over is the weakest specifier and resolves to either a def or a class; if a def contributes, the prim resolves to def and is traversed. | `composition-basics/specifiers.md` |
| 8 | **B** | Composition | In LIVRPS order (Local, Inherits, Variants, References, Payloads, Specializes), variants are stronger than references, so the variant's opinion wins. | `composition-basics/strength-ordering.md` |
| 9 | **A, B** | Composition | A variant can sparsely override properties on the prim or any descendant, define new descendant prims, or author new composition arcs scoped to that variant. | `creating-composition-arcs/variant-sets/what-are-variant-sets.md` |
| 10 | **B** | Composition | Visibility only changes a rendering value, but activation changes whether prims are composed at all, forcing USD to recompute part of the composition index. | `creating-composition-arcs/variant-sets/what-are-variant-sets.md` |
| 11 | **A** | Composition | The authored values stored in prim and property specs are opinions, and you interact with specs via the SdfSpec API, which prim spec and property spec each derive from. | `creating-composition-arcs/prim-composition.md` |
| 12 | **A** | Composition | Because the Looks scope is not encapsulated inside World, referencing /World brings the cube but not the material, so the binding target does not exist in the new context and breaks. | `creating-composition-arcs/encapsulation/what-is-encapsulation.md` |
| 13 | **A, B** | Composition | References and payloads are evaluated per layer stack, and a layer stack includes the layer's local composition arcs (internal references, variant sets, inherits, specializes) along with its recursive sublayers. | `creating-composition-arcs/strength-ordering/what-is-liverps.md` |
| 14 | **B** | Composition | In LIVRPS order, payloads (P) are weaker than references (R); occasionally a payload is chosen specifically to achieve a weaker composition order. | `creating-composition-arcs/references-payloads/what-are-payloads.md` |
| 15 | **A** | Composition | You add the variant set and variants, select the variant, then author its data inside GetVariantEditContext() so the opinions only apply when 'on' is active. | `creating-composition-arcs/variant-sets/working-with-variant-sets.md` |
| 16 | **A** | Composition | subLayerPaths is the pythonic Sdf.Layer property holding a layer's sublayers, retrieved from the root layer (and reusable on those sublayers to recurse). | `creating-composition-arcs/sublayers/sublayers-faq.md` |
| 17 | **B** | Data Exchange | Extraction aims to translate the source to USD as directly as possible to maintain fidelity, mapping source concepts to USD concepts. Restructuring, export options, and optimizations like mesh merging are transform-phase activities. | `data-exchange/data-exchange/what-is-data-exchange.md` |
| 18 | **A, B** | Data Exchange | Export options and optimizations like mesh merging are optional transform-phase steps. Direct concept mapping, choosing subdivisionScheme none, and sanitizing identifiers are all part of faithful extraction. | `data-exchange/data-exchange/what-is-data-exchange.md` |
| 19 | **A** | Data Exchange | Conceptual data mapping analyzes how data models map between formats and surfaces schema gaps. The USD/MaterialX concept mapping is given as an example, alongside the obj2usd mapping document. | `data-exchange/data-extraction/what-is-data-extraction.md` |
| 20 | **B** | Data Exchange | Intermediate texture maps, shader definitions, and color values used to map between formats are cited as examples of transient data; such data may be discarded or cached/reused across iterations. | `data-exchange/data-extraction/what-is-data-extraction.md` |
| 21 | **B** | Data Exchange | The lesson states Assimp has already taken care of mapping OBJ's single global vertex list to per-mesh vertices, so the converter does not have to. The other components handle validation, reparenting, and identifier sanitization respectively. | `data-exchange/data-extraction/exercise-extracting-geometry.md` |
| 22 | **A** | Data Exchange | usdchecker is part of the USD Toolset (command-line tools) and validates a stage or USDZ package against defined rules to ensure the asset is interchangeable and renderable by Hydra. The GUI/auto-fix/custom-rule description is the Omniverse Asset Validator built on top of it. | `data-exchange/asset-validation/what-is-asset-validation.md` |
| 23 | **B** | Data Exchange | A flat hierarchy has no common ancestor and no defaultPrim, so the whole asset is hard to reference. Creating /World, setting it as defaultPrim, and reparenting all root prims gives a single referenceable root. It is unrelated to compression, binding style, or up-axis. | `data-exchange/data-transformation/transformation-hierarchy.md` |
| 24 | **A, B** | Data Exchange | USDC (Crate) is compressed binary, efficient for heavy numeric data and uses memory mapping for fast access; USDA is human-readable text ideal for editing/inspection/diffing. USDA is not faster for heavy data, both formats are referenceable, and memory mapping is a USDC feature. | `stage-setting/usd-file-formats.md` |
| 25 | **B** | Data Exchange | When the export is one workstream of a larger structure, it can be additive: defining new prims, applying overrides on existing prims, or both. The exporter must distill the extraction down to that workstream's unique contributions (e.g., sparse overs). | `data-exchange/data-transformation/what-is-data-transformation.md` |
| 26 | **B** | Data Exchange | The file formats lesson notes that any 3D format can be loaded into stages through plugins, and even .usdc, .usd, .usda, and .usdz are themselves file format plugins. This shows any data provider can implement a plugin to natively speak USD. | `stage-setting/usd-file-formats.md` |
| 27 | **A** | Pipeline Development | Defining clear entry points and stable interfaces, encapsulating local dependencies with anchored paths, and keeping atomic versioned instances are all Modularity practices that enable reuse, iterative improvement, and parallel workstreams. | `asset-structure/asset-structure-principles/why-necessary.md` |
| 28 | **A, B** | Pipeline Development | An asset is a named, versioned, structured container of one or more resources (layers, textures, volumetric data, and more). There is no one-size-fits-all structure; a well-designed structure is tailored to the needs of clients and collaborators because usage and domains vary. | `asset-structure/asset-structure-principles/why-necessary.md` |
| 29 | **A** | Pipeline Development | For library assets with many entry points, Scope prims group and organize those entry points. Here `World` is a Scope organizing two independent entry points (`City`, `TaxiCab`) that downstream assets can reference individually; `World` itself is just a convention to keep the scene out of the root namespace. | `asset-structure/asset-structure-principles/asset-interface-pt2.md` |
| 30 | **A** | Pipeline Development | Since there is no general lofting utility, it is typically achieved through site- or project-specific post-scripts tied to asset generation and publishing (implemented with the Sdf API). | `asset-structure/reference-payload-pattern/what-is-ref-payload-pattern.md` |
| 31 | **A** | Pipeline Development | Ideally each workstream authors only its unique contributions sparsely rather than needlessly overwriting another workstream's work. The shading layer should sparsely export only material/binding data so the modeler and surfacer can iterate independently without conflicts. | `asset-structure/workstreams/adding-user-workstreams.md` |
| 32 | **A, B** | Pipeline Development | Legibility favors straightforward ASCII/UTF-8 identifiers and steering clear of constructs that cause conceptual overload. The Performance tips say to avoid timestamps, UUIDs, and versions in layer names that complicate dedup. Embedding arcs in names, deep boundaries, and a mirroring resolver localizing layers (which undermines Crate I/O optimizations) all run counter to the guidance. | `asset-structure/asset-structure-principles/why-necessary.md` |
| 33 | **A** | Pipeline Development | Unless documented or annotated as internal, variants and primvars on an asset entry point should generally be considered public and safe for downstream contexts to edit and set, with an expectation of stability. | `asset-structure/asset-parameterization/what-is-asset-parameterization.md` |
| 34 | **A** | Pipeline Development | Primvars are the lighter choice for single-property parameters, offering upfront memory savings at the cost of additional lookups in materials. Unlike variant selections, primvar variation does not generate new prototypes. | `asset-structure/asset-parameterization/what-is-asset-parameterization.md` |
| 35 | **A** | Pipeline Development | A workstream that combines computational and user-driven elements - such as a layer contributing synthesized motion on top of a hand-authored initial state - is classified as a hybrid workstream. | `asset-structure/workstreams/modeling-workstreams.md` |
| 36 | **A** | Pipeline Development | Embedding context directly into assets hints to users that those assets are intended to be included by reference, complementing legible naming and stable access conventions when organizing the prim hierarchy. | `asset-structure/asset-structure-principles/asset-interface-pt3.md` |
| 37 | **A** | Data Modeling | The paths lesson uses GetPrimPath() on a property path to obtain the owning prim path (e.g. attr_property_path.GetPrimPath()). AppendProperty/AppendChild build paths and IsPrimPath returns a boolean. | `stage-setting/prim-property-paths.md` |
| 38 | **A** | Data Modeling | The value-resolution lesson explains that Get() with no time code implies Usd.TimeCode.Default() (the non-time-varying default), which is usually not what you want with animation; use Usd.TimeCode.EarliestTime() to get the actual first animated value. | `beyond-basics/value-resolution.md` |
| 39 | **A, B** | Data Modeling | The lesson states composition is cached but value resolution is not, and attributes have three possible value sources (value clips, time samples, default). Most metadata resolves strongest-wins (only dictionaries merge), so C, D, and E are wrong. | `beyond-basics/value-resolution.md` |
| 40 | **A** | Data Modeling | The units lesson states that if upAxis is not explicitly authored, the fallback is 'Y' (and metersPerUnit fallback is 0.01). | `beyond-basics/units.md` |
| 41 | **A** | Data Modeling | The units lesson defines kilogramsPerUnit (KGPU) as the mass/density scale for physics and sets it via UsdPhysics.SetStageKilogramsPerUnit. metersPerUnit is linear scale, timeCodesPerSecond is temporal, and upAxis is orientation. | `beyond-basics/units.md` |
| 42 | **A, B** | Data Modeling | The file-formats lesson describes USDZ as an atomic zipped archive delivering assets together, read-only and optimal for XR, and states .usd can be ASCII or binary so the format can change without breaking references. USDA is text (not Crate), USDC is binary (not human-readable text), and USDZ is not used while still editing. | `stage-setting/usd-file-formats.md` |
| 43 | **A, B** | Data Modeling | The custom-properties lesson recommends nested namespaces (org:group:property, e.g. acme:sensor:temperature) since USD has no native struct type, and authoring with custom=True to mark non-schema attributes. Relationships are for links, and typeName does not register custom fields. | `beyond-basics/custom-properties.md` |
| 44 | **A** | Data Modeling | The attributes lesson lists extent as the attribute that defines the boundaries (bounding box) of a geometric prim; when points change, the extent must be recomputed to stay correct. displayColor, purpose, and typeName do not describe geometric bounds. | `stage-setting/properties/attributes.md` |
| 45 | **A** | Data Modeling | The primvars lesson defines faceVarying as one value per face-corner, which allows distinct values at each corner of each face (e.g. UV seams). vertex is per point, uniform per face, and constant is one value for the whole gprim. | `beyond-basics/primvars.md` |
| 46 | **A** | Debugging & Troubleshooting | The tracing example notes that even though workstreamA is the weaker sublayer, it holds the winning opinion because it has a Local opinion where the root and other sublayer do not. The L in LIVRPS (local opinions, including all sublayers) is strongest, and a present opinion beats an absent one. | `creating-composition-arcs/strength-ordering/tracing-through-liverps.md` |
| 47 | **A** | Debugging & Troubleshooting | GetPrimStack() returns the list of SdfPrimSpec handles contributing to the composed prim, ordered strong to weak (as the specifiers lesson demonstrates printing). The first entry is the strongest contributor, which determines strongest-wins resolution and reflects LIVRPS ordering. | `composition-basics/specifiers.md` |
| 48 | **A, B** | Debugging & Troubleshooting | TraverseAll() visits every prim regardless of the default predicate, which a full audit needs. Default Traverse() filters to active, loaded, defined, non-abstract prims and therefore omits class/abstract and inactive prims. GetAllChildren returns only immediate children, not a full recursive walk. | `beyond-basics/stage-traversal.md` |
| 49 | **A, B** | Debugging & Troubleshooting | The lesson notes Get(Usd.TimeCode.Default()) returns the authored (non-time-sampled) default, while Get(Usd.TimeCode.EarliestTime()) returns the first time sampled value. Get() with no argument equals Default() (not EarliestTime), and Default() does not interpolate samples. | `beyond-basics/value-resolution.md` |
| 50 | **A** | Debugging & Troubleshooting | The specifiers lesson states over is the weakest specifier and resolves to either a def or class. It holds overrides for opinions defined elsewhere; without an underlying def the composed prim is not defined, and default traversals visit only defined, non-abstract prims, so it is skipped. | `composition-basics/specifiers.md` |
| 51 | **A** | Debugging & Troubleshooting | Diagnostic delegates installed on TfDiagnosticMgr (e.g., UsdUtilsCoalescingDiagnosticDelegate) capture and coalesce/de-duplicate diagnostic messages for programmatic review. Trace is a performance/timing profiler, ChangeBlock batches edits (not diagnostics), and UsdAttributeQuery caches values. | `beyond-basics/value-resolution.md` |
| 52 | **A** | Debugging & Troubleshooting | Trace is OpenUSD's performance profiling framework: it instruments scopes to record execution timing and produce profiles, ideal for finding time bottlenecks. TfDebug prints categorized diagnostics, TfMallocTag profiles memory, and ChangeBlock batches edits — none of which time execution. | `beyond-basics/value-resolution.md` |
| 53 | **A** | Debugging & Troubleshooting | References encapsulate the referenced layer stack and remap paths relative to the referenced prim's namespace. A relationship target pointing outside that encapsulated namespace cannot be mapped across the reference, so the binding breaks — a classic encapsulation diagnosis. It is not about muting or strength reversal. | `creating-composition-arcs/strength-ordering/what-is-liverps.md` |
| 54 | **B** | Content Aggregation | Nested instancing is when an instance subgraph has instances within it, e.g. instanced assemblies with instanced components, or instanced components with instanced material networks. PointInstancers and scenegraph instances can also be nested in each other. | `asset-modularity-instancing/authoring-scenegraph-instancing/nested-instancing.md` |
| 55 | **B** | Content Aggregation | ComputeInstanceTransformsAtTime returns the full world-relative transform for an instance, automatically combining the prototype root transform and the instance-specific transforms (positions, velocities, orientations, angular velocities, scales), which is more robust than reading positions alone. | `asset-modularity-instancing/refining-point-instances.md` |
| 56 | **A, B, C** | Content Aggregation | prototypes, protoIndices, and positions are the mandatory properties of a PointInstancer. orientations (along with scales and ids) is optional and used to further pose instances. | `asset-modularity-instancing/authoring-point-instancing/point-instancing-intro.md` |
| 57 | **B** | Content Aggregation | Broadcasted refinement via a specialize/inherit target changes composition for the affected instances, producing one new shared prototype (3 to 4) while the instance count remains 1450. It is more efficient than editing each box individually. | `asset-modularity-instancing/refining-scenegraph-instances/scenegraph-broadcasted-refinement.md` |
| 58 | **B** | Content Aggregation | Primvars inherit down the hierarchy only if a descendant has not set a value. The referenced prototype's primvar opinion is stronger than the ancestor instancer's, so you must Block() the prototype's primvar to let the instancer's vertex-interpolated values apply per instance. | `asset-modularity-instancing/refining-point-instances.md` |
| 59 | **A, B** | Content Aggregation | Point instancing targets massive numbers of simpler items where instance overhead outweighs reuse, and it avoids cluttering the scenegraph with ~100,000 instanceable prims that scenegraph instancing would require. Scenegraph instancing does reuse assets and does support per-instance transforms, so C and D are false. | `asset-modularity-instancing/authoring-point-instancing/point-instancing-intro.md` |
| 60 | **A, B** | Content Aggregation | Deinstancing regains full editability for that copy but costs more prims roughly linearly per deinstanced copy. For many identical overrides, a prototype-creating technique (variant sets, ad hoc arcs, broadcasted refinement) is better; deinstancing does not remove composition arcs. | `asset-modularity-instancing/refining-scenegraph-instances/scenegraph-deinstance-refinement.md` |
| 61 | **A** | Visualization | The material exposes a surface output and connects it to the shader's surface output via material.CreateSurfaceOutput().ConnectToSource(shader.ConnectableAPI(), "surface"), linking the UsdPreviewSurface shader into the material container. | `stage-setting/properties/relationships.md` |
| 62 | **A** | Visualization | A relationship is a property with no data type; it records linkage by storing path values pointing to other scene elements, can have multiple targets, and (unlike attribute connections) cannot connect two existing attributes. | `stage-setting/properties/relationships.md` |
| 63 | **A** | Visualization | The Xform lesson states all UsdGeom classes inherit from UsdGeomImageable, whose intent is to capture any prim type that might want to be rendered or visualized. | `scene-description-blueprints/xform.md` |
| 64 | **A** | Visualization | A Scope is a lightweight, non-transformable grouping prim used to organize related prims (e.g. materials, lights, geometry); child transformable prims still evaluate correctly, and deactivating the root scope deactivates the whole group. | `scene-description-blueprints/scope.md` |
| 65 | **A, B** | Visualization | Parent transforms apply hierarchically, so Child_A (a descendant of Parent_Prim) inherits its transforms while Child_B under Alt_Parent does not. Child_A combines the parent's transform with its own local SetTranslate; transforms are hierarchical, not stage-global. | `scene-description-blueprints/xformcommonapi.md` |
| 66 | **A** | Visualization | UsdLux lists Dome lights (UsdLuxDomeLight) as a distinct category from area lights (Rect, Disk, Sphere, Cylinder) and the directional DistantLight; DomeLight provides environment/surrounding illumination. | `scene-description-blueprints/lights.md` |
| 67 | **A** | Customizing USD | usdGenSchema is the standard OpenUSD tool that reads a schema definition (schema.usda) and generates the corresponding schema classes and plugin metadata, codifying the data model. Path resolution is an Ar resolver's job; Hydra generation is a scene index plugin's; format composition is a file-format plugin's. | `scene-description-blueprints/schemas.md` |
| 68 | **A** | Customizing USD | A file-format plugin (SdfFileFormat) teaches USD how to interpret a non-native or custom format and present its data as a layer that participates in composition, often dynamically/in-memory. Resolving identifiers is the Ar resolver's role; the other options describe unrelated plugin types. | `scene-description-blueprints/schemas.md` |
| 69 | **A, B** | Customizing USD | Plugins are declared via plugInfo.json and discovered by PlugRegistry. A custom Ar resolver dynamically resolves/anchors asset paths and can serve in-memory or computed assets. Compiled plugins are tied to the USD version/ABI they were built against (C is false), and plugins are registered via plugInfo.json, not embedded in asset layers (D is false). | `scene-description-blueprints/schemas.md` |
| 70 | **A** | Customizing USD | The lesson's 'Working With Python' section shows Usd.SchemaRegistry.FindSchemaInfo() to retrieve schema info for a registered schema and Usd.SchemaRegistry.GetSchemaTypeName() to retrieve the schema typeName. The other options are kind, prim-query, or geometry helpers. | `scene-description-blueprints/schemas.md` |

> **Sourcing:** Authored for this repository to extend self-study — **not** official exam questions. Each item is grounded in the cited [Learn OpenUSD](https://docs.nvidia.com/learn-openusd/latest/index.html) lesson (the `docs/…` source of the corresponding published page). Domain weights and the study plan are in [`EXAM_PREP.md`](./EXAM_PREP.md); more practice in [`PRACTICE_QUESTIONS_EXTRA.md`](./PRACTICE_QUESTIONS_EXTRA.md).
