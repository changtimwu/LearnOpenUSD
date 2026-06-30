# OpenUSD Development Certification — Simulation Exam 1

A full-length **70-question** mock exam. The domain mix mirrors the *NCP: OpenUSD Development Exam Study Guide v1.1.0* weights (see [`EXAM_PREP.md`](./EXAM_PREP.md)). This is **Exam 1 of 3** — see also Exams [2](./SIMULATION_EXAM_2.md), [3](./SIMULATION_EXAM_3.md).

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

A layer's root opinion has the sublayers listed in this order:

```usda
#usda 1.0
(
    subLayers = [
        @./sublayerB.usda@,
        @./sublayerA.usda@
    ]
)
```

Both `sublayerA.usda` and `sublayerB.usda` author a value for `radius` on the same `/World/Sphere` prim, and the root layer itself authors no opinion for `radius`. Which sublayer's opinion wins?

- **A.** sublayerB.usda, because sublayers earlier in the list are stronger
- **B.** sublayerA.usda, because sublayers later in the list are stronger
- **C.** Neither; sublayers cannot author conflicting opinions on the same prim
- **D.** Whichever sublayer was saved to disk most recently

---

## Question 2 — Composition

You want to bring the same sphere asset into a scene three times so the copies appear as distinct sibling prims at different paths (e.g. `/World/Ball_01`, `/World/Ball_02`, `/World/Ball_03`). Which composition mechanism is designed for this?

- **A.** Sublayering the sphere layer three times
- **B.** References, which graft the prim hierarchy onto differently named destination prims
- **C.** A single inherit arc on the sphere
- **D.** Setting the defaultPrim metadata to three different values

---

## Question 3 — Composition

Which two variant operations are considered LIGHTWEIGHT switches because they only affect value resolution and do not require USD to rebuild the composition index? (Select two.)

- **A.** Changing an attribute value, such as a color from red to blue
- **B.** Changing a prim's visibility
- **C.** Changing which reference path is brought into the scene
- **D.** Changing a prim's activation opinion (activating/deactivating prims)

---

## Question 4 — Composition

Inside a variant set, you want all the opinions you author in a Python `with` block to be recorded inside the currently selected variant. Which call provides that scope?

- **A.** vset.GetVariantEditContext()
- **B.** vset.AddVariant('name')
- **C.** vset.ClearVariantSelection()
- **D.** prim.GetReferences().AddReference()

---

## Question 5 — Composition

What is the key behavioral difference between an inherit arc and a reference arc?

- **A.** Inherits create isolated instances, while references broadcast source changes to all consumers
- **B.** Inherits broadcast modifications of the source prim to all inheriting prims across layer stacks, while references create isolated instances
- **C.** Both create fully isolated instances that never share opinions
- **D.** References can be loaded/unloaded on demand, while inherits cannot graft data

---

## Question 6 — Composition

In a material library, a base `Plastic` material is specialized by a `RoughPlastic` material that lowers the glossiness value. Later, a stronger opinion authors a different glossiness on `RoughPlastic`. What happens to that glossiness?

- **A.** The specializes value always wins, so the stronger opinion is ignored
- **B.** The stronger opinion takes precedence, because specializes is the weakest composition arc
- **C.** USD reports an error for conflicting glossiness opinions
- **D.** The two glossiness values are averaged together

---

## Question 7 — Composition

A studio loads a massive city stage with all payloads unloaded so it opens quickly, then loads only the few assets for the block being inspected. Which two statements about payloads are correct? (Select two.)

- **A.** Payloads can be loaded and unloaded on demand, deferring the composition of their layers
- **B.** Payloads are weaker than references in the LIVRPS strength order
- **C.** Payloads automatically delete unused prims from disk
- **D.** Payloads remap namespaces like sublayers do

---

## Question 8 — Composition

Given this asset where the material binding lives OUTSIDE the entry-point prim:

```usda
def Xform "World" {
    def Cube "Box" (
        prepend apiSchemas = ["MaterialBindingAPI"]
    ) {
        rel material:binding = </Looks/Red>
    }
}
def Scope "Looks" {
    def Material "Red" {}
}
```

When another stage references just `/World`, what happens to the binding?

- **A.** The binding still resolves because relationship targets are always remapped automatically
- **B.** The binding breaks because /Looks/Red is not a descendant of the referenced /World prim
- **C.** The /Looks scope is automatically pulled in along with /World
- **D.** The reference fails to compose entirely

---

## Question 9 — Composition

Which two statements correctly contrast how sublayers combine content versus how references do? (Select two.)

- **A.** Sublayers include content with no remapping (like including)
- **B.** References graft content onto a destination prim with path translation (like grafting)
- **C.** Adding the same layer twice as a sublayer creates two distinct copies at different paths
- **D.** Sublayers remap namespaces exactly the way references do

---

## Question 10 — Composition

What composes a layer stack, and which layer is the strongest within it?

- **A.** Only the directly authored sublayers, with the last sublayer strongest
- **B.** A layer plus the recursive gathering of all its sublayers, with the layer itself first and strongest
- **C.** All layers reachable through references, with the deepest reference strongest
- **D.** A layer and its payloads only, with the payload strongest

---

## Question 11 — Composition

An asset file has this content. Another stage adds a reference to the file WITHOUT specifying a target prim path:

```usda
#usda 1.0
(
    defaultPrim = "Car"
)
def Xform "Car" {}
def Xform "Building" {}
```

What shows up in the consuming stage?

- **A.** Both Car and Building are brought in
- **B.** Only Car, because it is the defaultPrim and serves as the entry point
- **C.** Only Building, because it is defined last
- **D.** Nothing, plus a warning, because a path must always be specified

---

## Question 12 — Composition

Which two statements about the `over` specifier are correct? (Select two.)

- **A.** It holds overrides for opinions that already exist in the composed scene on another layer
- **B.** It is the strongest of the three specifiers and forces traversal of the prim
- **C.** It enables non-destructive editing because the override does not translate back to the original prim
- **D.** It concretely defines a brand-new prim that default traversals always visit

---

## Question 13 — Composition

When a prim is composed via a reference arc, in what order does USD build the result?

- **A.** It applies the destination prim's overrides first, then composes the referenced layer stack
- **B.** It composes the referenced prim's layer stack, adds the resulting prim spec to the destination, then applies the destination's overrides and additional arcs
- **C.** It merges the referenced layer's defaultPrim metadata into the destination layer's metadata
- **D.** It discards all destination opinions and uses only the referenced data

---

## Question 14 — Composition

Which two statements about internal references are correct? (Select two.)

- **A.** An internal reference targets a prim within the same USD layer rather than a prim in a different layer
- **B.** The asset path can be omitted for an internal reference because the target is in the same layer
- **C.** Internal references are composed more strongly than local opinions
- **D.** Internal references require the layer's defaultPrim to be unset

---

## Question 15 — Composition

In LIVRPS, the engine composes opinions recursively. When composing the contents brought in by a reference, what is the strength ordering applied to opinions WITHIN that referenced context?

- **A.** References inside the referenced context are evaluated as the strongest there
- **B.** LIVRPS resets and applies again: local opinions within the reference are strongest, then inherits, then variants, and so on
- **C.** Only specializes arcs are evaluated inside a reference
- **D.** Strength ordering does not apply inside references; first-authored wins

---

## Question 16 — Composition

Which Python sequence correctly creates a variant set named 'color', adds a 'red' variant, and selects it on a prim?

- **A.** vsets = prim.GetVariantSets(); vset = vsets.AddVariantSet('color'); vset.AddVariant('red'); vset.SetVariantSelection('red')
- **B.** prim.AddVariant('color'); prim.SetVariant('red')
- **C.** prim.GetReferences().AddVariantSet('color')
- **D.** vset = prim.GetInherits().AddVariantSet('color'); vset.Select('red')

---

## Question 17 — Data Exchange

A data provider wants OpenUSD to be able to read a proprietary CAD format directly as a referenced layer, translating it on the fly while keeping the CAD file as the source of truth. Which of the four implementation types should they build?

- **A.** An importer in the CAD application
- **B.** An exporter in the CAD application
- **C.** A file format plugin
- **D.** A one-way standalone converter

---

## Question 18 — Data Exchange

In OpenUSD's data exchange terminology, what does an importer do?

- **A.** Translates data from the application's runtime format into OpenUSD
- **B.** Translates data from OpenUSD into the application's runtime format
- **C.** Composes an external file format as a payload without translating it
- **D.** Packages an asset and its textures into a single USDZ archive

---

## Question 19 — Data Exchange

An OBJ material is named "Material.001" in the source file. In the obj2usd extraction code, why is the following line necessary before defining the prim?

```python
sanitized_mat_name = Tf.MakeValidIdentifier(mtl["NAME"])
```

- **A.** It converts the material color values from sRGB to linear space
- **B.** It replaces invalid characters (like the period) so the name can be used as a valid prim identifier
- **C.** It guarantees the material name is globally unique across the stage
- **D.** It maps the OBJ shininess value to a USD roughness value

---

## Question 20 — Data Exchange

When extracting an OBJ polygonal mesh into UsdGeom.Mesh, the converter sets:

```python
usd_mesh.CreateSubdivisionSchemeAttr(UsdGeom.Tokens.none)
```

Which TWO statements correctly explain this choice?

*(Select two options.)*

- **A.** It treats the mesh explicitly as a polygonal mesh rather than a subdivision (SubD) surface
- **B.** OBJ does not support SubD meshes, so this respects the source format's intent
- **C.** It forces Hydra to ignore any authored normals on the mesh
- **D.** It is required for the mesh to receive a bound material
- **E.** It flattens the per-face indices into the faceVertexIndices array

---

## Question 21 — Data Exchange

The data extraction lesson notes that the output USD from the extract phase is often treated as transient data. Why should an exporter author still consider exposing the in-memory extracted stage or writing it to disk?

- **A.** Because usdchecker can only validate stages that have been written to disk
- **B.** Because it provides a common foundation that third-party developers can use to write their own transformation steps without reverse-engineering yours
- **C.** Because USDZ packaging requires the extracted stage to exist on disk
- **D.** Because the transform phase cannot run on in-memory stages

---

## Question 22 — Data Exchange

usdchecker reports three errors on a freshly converted obj2usd asset, all related to stage metadata. Which set of metadata does it flag as missing?

- **A.** kind, purpose, and visibility
- **B.** upAxis, metersPerUnit, and defaultPrim
- **C.** subdivisionScheme, normals, and faceVertexCounts
- **D.** documentation, customLayerData, and timeCodesPerSecond

---

## Question 23 — Data Exchange

Which capabilities does the Omniverse Asset Validator add on top of usdchecker?

- **A.** A GUI, command-line interface, Python API, automatic fixes for failed validations, and the ability to add new rules
- **B.** The ability to convert USD assets to glTF and FBX
- **C.** Real-time Hydra rendering of the validated stage in a viewport
- **D.** Automatic generation of conceptual data mapping documents

---

## Question 24 — Data Exchange

An organization's pipeline is Z-up, but OBJ is Y-up. When the user passes --up-axis Z, the converter sets the stage up-axis to Z and then runs:

```python
xformable = UsdGeom.Xformable(stage.GetDefaultPrim())
xformable.AddRotateXOp(opSuffix="unitsResolve").Set(90.0)
```

Why is the rotate-X op applied?

- **A.** To convert the mesh from meters to centimeters
- **B.** To reorient the geometry so it remains upright (face up) in the new Z-up coordinate system
- **C.** To merge all meshes into a single prim under /World
- **D.** To flatten the prim hierarchy before packaging

---

## Question 25 — Data Exchange

A developer needs a small interface layer that mostly references other content and that they intend to inspect and diff by hand, plus archive in legible form. Which native USD format best fits this role?

- **A.** USDC (Crate binary)
- **B.** USDA (human-readable text)
- **C.** USDZ (zipped archive)
- **D.** glTF

---

## Question 26 — Data Exchange

The transformation hierarchy exercise reparents all root-namespace prims under a new /World prim. Which API is used to perform the reparenting?

- **A.** Usd.NamespaceEditor with ReparentPrim() and ApplyEdits()
- **B.** Sdf.CopySpec() with a layer stack swap
- **C.** UsdShade.MaterialBindingAPI.Apply()
- **D.** Usd.Stage.Flatten()

---

## Question 27 — Pipeline Development

A studio adopts a naming standard where capitalized prim names (e.g. `Body`) mark elements safe for downstream overrides, while underscore-prefixed names (e.g. `_rigCache`) signal internal-only prims that collaborators should not author opinions on. Which principle of scalable asset structure does this convention primarily serve?

- **A.** Legibility
- **B.** Performance
- **C.** Modularity
- **D.** Navigability
- **E.** Instancing

---

## Question 28 — Pipeline Development

In an exercise, three references of a building asset are added to `scene.usda`, but usdview emits warnings such as: the relationship target `</roof>` from the material binding refers to a path outside the scope of the reference. The roof renders white instead of green. What is the root cause?

- **A.** The roof material prim was not encapsulated beneath the `World` entry point, so it lives outside the referenced subtree and its binding target is ignored
- **B.** The stage's `defaultPrim` metadata was set to the wrong prim, so referencing picked up the wrong subtree
- **C.** The Crate (.usdc) file was localized by a mirroring resolver, corrupting the relationship
- **D.** Variant selections on the entry point generated new prototypes that dropped the binding

---

## Question 29 — Pipeline Development

A library asset is a palette of related materials, where each material may be individually referenced by downstream assets rather than the whole palette being brought in through one entry point. What is the recommended way to group and organize these many entry points?

- **A.** Use `Scope` prims to group and organize the entry points
- **B.** Use a single `Xform` named `World` as the sole `defaultPrim`
- **C.** Place each material under its own payload so they load lazily
- **D.** Use a `class` prim per material to provide the reference target

---

## Question 30 — Pipeline Development

Looking at `export.usd` in usdview, the mesh and material prims sit directly in the root namespace with no common ancestor. A learner points at the top "root" node in the tree view and says "I can just reference that to bring everything in together." Why is this incorrect?

- **A.** The "root" node represents the stage's pseudo-root, which is not a defined prim and cannot be referenced
- **B.** The root node is a `Scope` and Scopes block referencing of their children
- **C.** The root node is the `defaultPrim` but referencing ignores `defaultPrim` metadata
- **D.** Referencing the root would include the pseudo-root's session layer overrides, which is disallowed

---

## Question 31 — Pipeline Development

A team parameterizes a car asset two ways: a `color_variant` variant set with discrete swatches at the entry point, and a continuous `primvars:asset_base_color` on the entry point. They want to maximize scene-graph instancing while keeping memory low for a single continuous color knob. Which TWO statements are correct?

*(Select two options.)*

- **A.** Authoring different variant selections will generate new prototypes for downstream contexts
- **B.** Primvar-based variation does not generate new prototypes, so it remains compatible with instancing
- **C.** Primvars must be discretized into a variant set before they can be inherited down the hierarchy
- **D.** Variant selection is always lighter on memory than a primvar for a single continuous property
- **E.** Primvars cannot be authored on an asset entry point

---

## Question 32 — Pipeline Development

Why does the curriculum prefix entry-point primvars with `asset_` (e.g. `primvars:asset_base_color`)?

- **A.** To avoid namespace collisions with other primvars in the hierarchy
- **B.** To force the primvar to be constant-interpolated instead of inherited
- **C.** To mark the primvar as private and block downstream edits
- **D.** Because OpenUSD requires all entry-point primvars to carry that reserved prefix

---

## Question 33 — Pipeline Development

A pipeline TD writes a publish post-process to loft a `primvars:accentColor` attribute from `contents.usd` up onto the `/World` prim in the asset interface layer so it is discoverable with the payload unloaded. Which approach matches the curriculum's recommended technique?

- **A.** Use the Sdf API: `Sdf.CopySpec(contents_layer, prop_spec.path, asset_layer, prop_spec.path)` for each property, then `prim_spec.RemoveProperty(prop_spec)` on the contents layer
- **B.** Call `stage.Flatten()` to merge the payload into the root layer permanently
- **C.** Add a sublayer arc from the interface layer to the payload so the attribute composes through
- **D.** Run a built-in `UsdUtils.LoftFields()` utility shipped with OpenUSD

---

## Question 34 — Pipeline Development

Consider this reference/payload interface layer snippet:

```usda
class "prop_MyAsset" {}

def Xform "MyAsset" (
    variantSets = ["color_variant"]
    variants = { string color_variant = "red" }
    payload = [@./contents.usd@]
) {
    variantSets "color_variant" {
        "red" {}
        "blue" {}
    }
}
```

What is the role of the lofted `variantSets`/`variants` declared on `MyAsset` above the payload?

- **A.** They advertise the sets and selections specified by the underlying payload so they are discoverable without loading it; they contribute no opinions themselves
- **B.** They override and replace the variant sets authored inside `contents.usd`
- **C.** They are required for the payload arc to resolve at all
- **D.** They force the payload to load eagerly whenever the variant is read

---

## Question 35 — Pipeline Development

A modeler exports geometry to `geometry.usd` while a surfacing artist's tool writes only material and binding opinions to `shading.usd`, which is then sublayered into the asset. The lesson stresses that each workstream "should only author its unique contributions sparsely." Which TWO statements about this sparse, split-layer approach are correct?

*(Select two options.)*

- **A.** The modeler and surfacer can iterate on the same asset independently and in parallel without blocking each other or overwriting one another's work
- **B.** Splitting parallel workstreams can offer performance benefits, reducing storage needs and publishing time
- **C.** It replaces the need for a version control system because each layer is independently tracked
- **D.** It guarantees fewer composed prims by collapsing both layers into one prototype
- **E.** It forces all materials to be instanced automatically

---

## Question 36 — Pipeline Development

In the `obj2usd` converter, a `set_up_axis` step that changes the stage to Z-up also calls `xformable.AddRotateXOp(opSuffix="unitsResolve").Set(90.0)` on the default prim. Why suffix the op with `unitsResolve`, and why is this transformation step structured as an opt-in export option rather than a mandatory chaser?

- **A.** The suffix conveys the op's purpose to downstream users by convention, and it is optional because not every pipeline wants the up-axis changed from OBJ's Y default
- **B.** The suffix is required by OpenUSD for any rotate op, and it is optional because rotations are computationally expensive
- **C.** The suffix hides the op from usdview, and it is optional because mandatory chasers cannot modify transforms
- **D.** The suffix marks the op as internal, and it is optional because Z-up stages cannot be referenced

---

## Question 37 — Data Modeling

In OpenUSD, 'property' is an umbrella term. Which statement correctly describes the relationship between properties, attributes, and relationships?

- **A.** Attributes and relationships are the two kinds of properties; a property contains the 'real data' on a prim.
- **B.** Properties are a kind of attribute, and relationships are a separate, unrelated concept.
- **C.** A property is a kind of prim that only stores metadata.
- **D.** Attributes are properties, but relationships are a form of metadata, not properties.

---

## Question 38 — Data Modeling

A character asset is referenced into a scene, which moves the prim that a relationship targets to a new path. Why does the relationship still resolve to the correct object?

- **A.** Relationships are robust against path translation and automatically remap to the target's new location.
- **B.** Relationships store the target prim's memory address, which never changes.
- **C.** The relationship is re-authored automatically every time the stage is saved.
- **D.** Relationships are converted to attributes during referencing, which preserves the value.

---

## Question 39 — Data Modeling

Which TWO statements about the OpenUSD path syntax used by Sdf.Path are correct?

*(Select two options.)*

- **A.** A forward slash '/' separates prim names in the namespace hierarchy, e.g. /geo/box.
- **B.** A period '.' after an identifier introduces a property, e.g. /geo/box.weight.
- **C.** Square brackets '[]' indicate a variant selection, e.g. /geo/box[size=large].
- **D.** A colon ':' separates prims from their parent, e.g. /geo:box.
- **E.** A property name cannot use a namespace prefix such as userProperties:tag.

---

## Question 40 — Data Modeling

You have an Sdf.Path object for /World/Geometry. Which method builds the child path /World/Geometry/Sphere?

- **A.** path.AppendChild('Sphere')
- **B.** path.AppendProperty('Sphere')
- **C.** path.GetParentPath('Sphere')
- **D.** path.IsPrimPath('Sphere')

---

## Question 41 — Data Modeling

A pipeline tool wants to attach author name, creation date, and rendering hints to a prim WITHOUT changing the schema, and these values never animate. Which OpenUSD facility is the intended fit?

- **A.** Metadata, which stores non-animatable name-value pairs on prims, properties, or layers.
- **B.** Time samples, which efficiently store values per frame.
- **C.** A relationship, which links the prim to a metadata prim.
- **D.** A typed IsA schema, which must be registered for any extra data.

---

## Question 42 — Data Modeling

When two composed layers both author the dictionary metadata customData, how does value resolution combine them?

- **A.** Dictionaries combine element by element (per key); the result contains keys from both layers, with stronger opinions winning per shared key.
- **B.** The entire customData dictionary from the strongest layer replaces the weaker one wholesale.
- **C.** Only the weakest layer's customData survives because metadata is bottom-up.
- **D.** The two dictionaries are concatenated as strings.

---

## Question 43 — Data Modeling

Which TWO statements correctly contrast IsA (typed) schemas with API schemas?

*(Select two options.)*

- **A.** An IsA schema is assigned via the typeName metadata, and a prim can subscribe to only one IsA schema at a time.
- **B.** An API schema does not assign a typeName; it is list-edited into the apiSchemas metadata and queried with HasAPI.
- **C.** Both IsA and API schemas set the prim's typeName, but only one IsA schema is allowed.
- **D.** An API schema sets typeName, while an IsA schema is stored in apiSchemas.
- **E.** API schemas can only be applied to typeless prims, while IsA schemas require a typeName.

---

## Question 44 — Data Modeling

You are authoring a custom attribute named 'serial_number' that is not part of any schema. Which approach matches OpenUSD best practice from the curriculum?

- **A.** prim.CreateAttribute('my_namespace:serial_number', Sdf.ValueTypeNames.String, custom=True)
- **B.** prim.CreateAttribute('serial_number', Sdf.ValueTypeNames.String) with no namespace and custom=False
- **C.** prim.CreateRelationship('serial_number', custom=True)
- **D.** prim.SetTypeName('serial_number')

---

## Question 45 — Data Modeling

A primvar's displayColor is authored with the 'uniform' interpolation token on a mesh with 2 faces. How many color values must be supplied?

- **A.** 2 — one value per face.
- **B.** 1 — a single value for the whole gprim.
- **C.** 6 — one value per point.
- **D.** 8 — one value per face-corner.

---

## Question 46 — Debugging & Troubleshooting

A developer profiles a tool that calls attr.Get() on the same attribute hundreds of times per frame and notices it never gets faster on repeat calls, even though the stage's composition is unchanged. Which statement about OpenUSD best explains this and how to fix it?

- **A.** Composition is cached as a per-prim index, but value resolution is NOT cached and re-runs on every Get(); cache the result yourself with a tool like UsdAttributeQuery.
- **B.** Value resolution is cached but composition is not, so calling stage.Reload() each frame would speed it up.
- **C.** Both composition and value resolution are cached, so the slowdown must be in Python overhead unrelated to USD.
- **D.** Neither composition nor value resolution is cached; the only fix is to flatten the stage before querying.

---

## Question 47 — Debugging & Troubleshooting

A character rig has time-sampled animation. A pipeline script reads the joint scale with `value = attr.Get()` (no time code) and always gets the rest-pose value instead of the animated motion. What is the correct diagnosis?

- **A.** Get() with no argument implies Usd.TimeCode.Default(), which returns the non-time-sampled default; use Usd.TimeCode.EarliestTime() or a specific time code to read the animation.
- **B.** Get() with no argument returns the value at the stage's StartTimeCode, so the start frame must be set incorrectly.
- **C.** Time samples are never readable through Get(); you must open the value clip files directly.
- **D.** Get() returns an interpolated average across all time samples, which happens to equal the rest pose here.

---

## Question 48 — Debugging & Troubleshooting

An artist authored a strong `over` opinion setting `radius = 5.0` in the root layer, but the composed sphere still shows the weaker base value. You suspect the layer carrying the override was muted with UsdStage.MuteLayer. Which two statements about muting are correct? (Select 2)

- **A.** UsdStage.IsLayerMuted(layerIdentifier) returns True when the layer is muted, confirming it is excluded from composition.
- **B.** Muting is non-destructive: the layer's authored opinions are untouched on disk and recompose once the layer is unmuted.
- **C.** Muting a layer permanently deletes its prim and property specs from the layer.
- **D.** Muting a layer sets active=false on every prim authored in that layer.

---

## Question 49 — Debugging & Troubleshooting

A renderer iterates `for prim in stage.Traverse():` and reports that a `class "_chair"` template prim (used as a reference target) never appears, even though it is clearly present in the .usda file. Why?

- **A.** The default Traverse() predicate visits only active, loaded, defined, non-abstract prims; a class prim is abstract, so it is skipped. Use TraverseAll() to include it.
- **B.** Class prims are removed from the stage during composition and only exist on disk.
- **C.** Traverse() skips any prim whose name starts with an underscore, a naming convention for templates.
- **D.** The class prim must be inactive; calling SetActive(True) is the only way to make Traverse() visit it.

---

## Question 50 — Debugging & Troubleshooting

While debugging why a property's expected opinion does not win in the composed result, which of the following are legitimate reasons covered by the lessons? (Select 2)

- **A.** A stronger arc or layer overrides it per LIVRPS strength ordering.
- **B.** The layer carrying the opinion was muted with UsdStage.MuteLayer and is not composed.
- **C.** USD silently discards any opinion authored with an over specifier, so over opinions never compose.
- **D.** Value resolution caches the first value it ever reads, so later opinions are ignored until cache invalidation.

---

## Question 51 — Debugging & Troubleshooting

You want to inspect every layer that contributes an opinion to a single property at a given time, ordered strongest to weakest, to trace why a value resolves the way it does. Which call returns this ordered list of property specs?

- **A.** property.GetPropertyStack(time)
- **B.** property.GetTargets()
- **C.** stage.GetLayerStack()
- **D.** property.GetResolvedValue()

---

## Question 52 — Debugging & Troubleshooting

A large scene authors thousands of prims in a tight loop, and the tool is slow because each edit triggers change notification and recomposition. Which OpenUSD facility is specifically intended to batch these edits and reduce notification/recompose overhead?

- **A.** Sdf.ChangeBlock — wrap the batch of edits so notifications and recomposition are coalesced.
- **B.** TfMallocTag — to free memory between each authored prim.
- **C.** UsdStage.MuteLayer — to suspend composition during authoring.
- **D.** Usd.PrimRange.PruneChildren — to skip recomposing child prims.

---

## Question 53 — Debugging & Troubleshooting

Two sublayers each author the relationship `material:binding` with different targets. While debugging, you call GetTargets() expecting only the strongest layer's target, but you get targets from both layers. What is the correct explanation?

- **A.** Relationships do not pick the single strongest opinion; their targets are merged across layers via list-editing semantics, so targets from multiple layers combine.
- **B.** GetTargets() is buggy for relationships and returns disk order instead of resolved order.
- **C.** Relationships always resolve strongest-wins like ordinary metadata, so this indicates a corrupted layer.
- **D.** The two targets are duplicates that USD failed to deduplicate; only one is actually composed.

---

## Question 54 — Content Aggregation

In the OpenUSD Kind registry, which token represents an abstract base kind that should never be authored as the kind of any prim?

- **A.** component
- **B.** model
- **C.** assembly
- **D.** subcomponent

---

## Question 55 — Content Aggregation

Which statement correctly describes the role of a subcomponent in OpenUSD?

- **A.** It is a model kind that inherits from group and organizes components.
- **B.** It is the abstract base kind that all other kinds inherit from.
- **C.** It is not a model kind; it annotates an important prim inside a component, since a component cannot contain other component models.
- **D.** It is a subkind of assembly used to aggregate other assemblies.

---

## Question 56 — Content Aggregation

A developer enables scenegraph instancing on three crates as shown:\n\ndef Xform "Warehouse"\n{\n    def "Crate_01" (instanceable = true) {}\n    def "Crate_02" (instanceable = true) {}\n    def "Crate_03" (instanceable = true) {}\n}\n\nHow many prototypes does OpenUSD generate?

- **A.** Three, one per instanceable prim.
- **B.** One, shared by all three crates.
- **C.** Zero, because none of the prims have a composition arc to derive a prototype from.
- **D.** Three instances and one prototype.

---

## Question 57 — Content Aggregation

Which Python call correctly sets a prim's kind metadata to component?

- **A.** prim.SetKind(Kind.Tokens.component)
- **B.** Usd.ModelAPI(prim).SetKind(Kind.Tokens.component)
- **C.** UsdGeom.Component.Define(stage, prim.GetPath())
- **D.** prim.SetTypeName(Kind.Tokens.component)

---

## Question 58 — Content Aggregation

According to the OpenUSD instancing FAQ, which characteristics are specific to scenegraph (native) instancing rather than point instancing? (Select 2)

- **A.** It is composition-based, deriving implicit prototypes from composition arcs.
- **B.** Instances are identifiable via an integer index.
- **C.** Instances and their descendants are identifiable via path, with editable instanceable prims but read-only instance proxies.
- **D.** It is schema-based, requiring explicit prototypes in the scene description.

---

## Question 59 — Content Aggregation

A PointInstancer has rel prototypes = [<Prototypes/Peanut_01>, <Prototypes/Peanut_02>] and int[] protoIndices = [0, 0, 0, 1]. What does this mean?

- **A.** The first three instances use Peanut_01 and the fourth uses Peanut_02.
- **B.** The first three instances use Peanut_02 and the fourth uses Peanut_01.
- **C.** All four instances use whichever prototype is listed first.
- **D.** It is invalid because protoIndices must list paths, not integers.

---

## Question 60 — Content Aggregation

Why does the Learn OpenUSD curriculum say a gprim tagged as a component is a sign of a problem in the model hierarchy?

- **A.** Gprims cannot legally have kind metadata authored on them at all.
- **B.** It indicates a 'deep' model hierarchy, which adds composition overhead and misses the performance gains of pruned traversal at a shallow component boundary.
- **C.** It forces OpenUSD to create a prototype for every gprim in the scene.
- **D.** It prevents the prim from ever being referenced as an asset.

---

## Question 61 — Visualization

A `UsdGeom.Xform` is defined at `/World/Robot` with a translate applied, and several geometry prims are authored as its descendants. What effect does the Xform's transform have on those descendants?

- **A.** The transform is applied hierarchically, so it affects all child prims under the Xform
- **B.** The transform only affects the Xform prim itself and never its children
- **C.** Children ignore parent transforms unless each child copies the same xformOp
- **D.** Only immediate children inherit the transform; grandchildren are excluded

---

## Question 62 — Visualization

You need to inspect which transform operations are authored on a transformable prim and the order in which they are combined. Which attribute do you query?

- **A.** The xformOpOrder attribute, via GetXformOpOrderAttr()
- **B.** The purpose attribute, via GetPurposeAttr()
- **C.** The visibility attribute, via GetVisibilityAttr()
- **D.** The extent attribute, via GetExtentAttr()

---

## Question 63 — Visualization

A developer tries to position a `UsdGeom.Scope` prim by wrapping it in `UsdGeom.XformCommonAPI` and calling `SetTranslate`. Why will this fail to move the Scope's contents?

- **A.** Scope is a non-transformable grouping prim and cannot be transformed
- **B.** XformCommonAPI must be Apply()'d before it can be used on any prim
- **C.** Scope can only be translated, not scaled, so SetTranslate is rejected
- **D.** SetTranslate requires a time code argument when used on a Scope

---

## Question 64 — Visualization

Which statement most accurately describes `UsdGeom.XformCommonAPI`?

- **A.** It is a non-applied API schema providing a standard set of translate, rotate, scale, and pivot operations for interchange
- **B.** It is an applied API schema that must be Apply()'d and adds a custom shading network to a prim
- **C.** It is a concrete typed prim schema like Xform that defines its own prim type
- **D.** It is a relationship schema used to bind materials to geometry

---

## Question 65 — Visualization

In OpenUSD, `UsdShadeMaterial` is best described as which of the following?

- **A.** A container that stores the data defining a shading material for a renderer
- **B.** A single shader node that computes a surface color directly
- **C.** A relationship that targets geometry prims to be shaded
- **D.** A primvar that interpolates color values across a mesh surface

---

## Question 66 — Visualization

After binding materials, you read them back with `UsdShade.MaterialBindingAPI(prim).GetDirectBinding().GetMaterial()`. Which TWO statements are correct about material binding in OpenUSD?

*(Select two options.)*

- **A.** Material binding is encoded as a relationship named material:binding that targets a UsdShade.Material
- **B.** GetDirectBinding().GetMaterial() returns the directly bound Material prim (or none if unbound)
- **C.** Binding is stored as a float attribute holding the material's diffuse color
- **D.** A material can only be bound to a single prim across the entire stage

---

## Question 67 — Customizing USD

From which core C++ base class are IsA (typed) schemas, such as UsdGeomMesh, derived?

- **A.** UsdTyped
- **B.** UsdAPISchemaBase
- **C.** UsdSchemaRegistry
- **D.** UsdModelAPI

---

## Question 68 — Customizing USD

What distinguishes an abstract (non-concrete) IsA schema like UsdGeomPointBased from a concrete IsA schema like UsdGeomMesh?

- **A.** An abstract schema provides a name but no typeName, so it cannot be instantiated as a prim and instead serves as a base class for related concrete schemas.
- **B.** An abstract schema is list-edited into apiSchemas, while a concrete schema sets a typeName.
- **C.** An abstract schema can be applied to a prim multiple times with different instance names.
- **D.** An abstract schema assigns a typeName but its properties are namespaced, while a concrete schema's are not.

---

## Question 69 — Customizing USD

A studio wants to differentiate between two levels of assembly ("location" vs "world") and several subcomponent types. According to the model-hierarchy guidance, what is the recommended approach?

- **A.** Prefer custom properties, user properties, or schemas to describe the taxonomy, and rarely (if ever) extend the Kind library.
- **B.** Always extend the Kind library, since kinds are the only mechanism designed to carry pipeline-specific classification.
- **C.** Tag a gprim directly as a component to flatten the hierarchy.
- **D.** Define each taxonomy level as a separate concrete IsA schema deriving from UsdGeomScope.

---

## Question 70 — Customizing USD

Which TWO statements correctly describe what OpenUSD schemas are and are not? (Select 2)

- **A.** Schemas are primarily data models with documented rules on how the data should be interpreted.
- **B.** A schema necessarily ships with the implementation of the behaviors it describes (e.g., UsdPhysics includes a physics engine).
- **C.** Defining behaviors in a schema API is optional, and actual behavior enforcement can be handled by other subsystems in the runtime.
- **D.** A schema replaces the need for any composition engine because it carries its own runtime.

---

## Answer Key

| Q | Answer | Domain | Why | Grounded in |
|---|---|---|---|---|
| 1 | **A** | Composition | Sublayers are ordered by opinion strength with the first layer in the list being the strongest, so sublayerB outweighs sublayerA. | `creating-composition-arcs/sublayers/what-are-sublayers.md` |
| 2 | **B** | Composition | References graft a prim hierarchy onto a destination prim with path translation, so the same content can appear as multiple distinctly named siblings, whereas sublayering would just redefine the same path. | `creating-composition-arcs/references-payloads/what-are-references.md` |
| 3 | **A, B** | Composition | Changing attribute values and visibility only alter resolved values, while swapping reference paths or changing activation force USD to recompute the composition index. | `creating-composition-arcs/variant-sets/what-are-variant-sets.md` |
| 4 | **A** | Composition | GetVariantEditContext() returns a context manager so all specs authored in the with block are stored inside the selected variant. | `composition-basics/variant-sets.md` |
| 5 | **B** | Composition | An inherit arc lets modifications to the source prim broadcast to all inheriting prims across layer stacks, whereas a reference creates an isolated instance. | `creating-composition-arcs/inherits-specializes/what-is-inherits.md` |
| 6 | **B** | Composition | Specializes provides only fallback values and is the weakest arc, so any subsequent (stronger) opinion takes precedence. | `composition-basics/strength-ordering.md` |
| 7 | **A, B** | Composition | Payloads behave like references but can be loaded/unloaded on demand for deferred loading, and they sit weaker than references in LIVRPS. | `creating-composition-arcs/references-payloads/what-are-payloads.md` |
| 8 | **B** | Composition | The Material is unencapsulated (not a descendant of the referenced /World), so once /World is referenced into a new context, the relationship target no longer exists and the binding breaks. | `creating-composition-arcs/encapsulation/what-is-encapsulation.md` |
| 9 | **A, B** | Composition | Sublayers are like including with no remapping while references graft a hierarchy onto a destination prim; adding a layer twice as a sublayer just overwrites rather than duplicating. | `creating-composition-arcs/references-payloads/references-faq.md` |
| 10 | **B** | Composition | A layer stack is the ordered set of layers from recursively gathering all sublayers, plus the layer itself as first and strongest. | `creating-composition-arcs/strength-ordering/what-is-liverps.md` |
| 11 | **B** | Composition | When a layer is referenced without an explicit prim path, the defaultPrim (Car) is used as the entry point, eliminating the need to specify a target. | `composition-basics/default-prim.md` |
| 12 | **A, C** | Composition | An over holds sparse overrides for opinions defined elsewhere and supports non-destructive editing because the override does not modify the original prim; it is the weakest specifier, not the strongest. | `composition-basics/specifiers.md` |
| 13 | **B** | Composition | USD first composes the referenced prim's layer stack and adds the result to the destination prim, then applies the destination's overrides and any additional composition arcs. | `composition-basics/references.md` |
| 14 | **A, B** | Composition | Internal references graft from another prim in the same layer and can omit the asset/layer identifier; they are still weaker than local opinions in LIVRPS. | `creating-composition-arcs/references-payloads/what-are-references.md` |
| 15 | **B** | Composition | LIVRPS applies recursively within each composition context, so inside a reference the local opinions are strongest, followed by inherits, variants, references, payloads, and specializes. | `composition-basics/strength-ordering.md` |
| 16 | **A** | Composition | The variant APIs are GetVariantSets() -> AddVariantSet() -> AddVariant() -> SetVariantSelection(), as shown in the Variant Sets Basics lesson. | `composition-basics/variant-sets.md` |
| 17 | **C** | Data Exchange | Only a file format plugin lets an OpenUSD stage compose another format directly as a reference/payload/sublayer, translating it on the fly while the source file remains the source of truth. Importers/exporters translate to/from an application's runtime format, and a converter produces a separate output file rather than composing the source live. | `data-exchange/data-exchange/what-is-data-exchange.md` |
| 18 | **B** | Data Exchange | Importers translate data from OpenUSD to the runtime format of the application (File > Import). Exporters do the reverse, translating runtime data to OpenUSD. | `data-exchange/data-exchange/what-is-data-exchange.md` |
| 19 | **B** | Data Exchange | Tf.MakeValidIdentifier replaces invalid characters with underscores so a source name (e.g., "Material.001", whose period is illegal in a prim path) becomes a valid USD identifier. It does not enforce uniqueness or perform any color/parameter conversion. | `data-exchange/data-extraction/exercise-extracting-materials.md` |
| 20 | **A, B** | Data Exchange | Setting subdivisionScheme to none makes the mesh explicitly polygonal rather than a SubD surface, respecting OBJ which has no SubD concept. It does not disable authored normals, is unrelated to material binding, and does not perform index flattening (that is done by the separate index loop). | `data-exchange/data-extraction/exercise-extracting-geometry.md` |
| 21 | **B** | Data Exchange | The extracted stage is a faithful, common foundation valuable to third-party developers who want to author their own transforms; without access they would be left reverse-engineering your transformations. The other options are not reasons given in the lesson. | `data-exchange/data-extraction/what-is-data-extraction.md` |
| 22 | **B** | Data Exchange | The asset is missing upAxis and the linear units (metersPerUnit) metadata, plus defaultPrim. The first two are fixed in extraction; defaultPrim is better handled in the transform phase. | `data-exchange/asset-validation/exercise-asset-validation-testing.md` |
| 23 | **A** | Data Exchange | Omniverse Asset Validator is based on usdchecker and adds a UI, CLI, Python API, automatic fixes for failed validations, and the ability to add new rules. It is not a format converter, renderer, or mapping-document generator. | `data-exchange/asset-validation/what-is-asset-validation.md` |
| 24 | **B** | Data Exchange | Changing only the upAxis metadata would leave the geometry tipped over; the corrective 90-degree X rotation reorients the asset to stay upright in the new Z-up system. The op suffix 'unitsResolve' documents its purpose. It has nothing to do with units scaling, merging, or flattening. | `data-exchange/data-transformation/exercise-export-option.md` |
| 25 | **B** | Data Exchange | USDA is human-readable text, optimal for small interface/aggregator layers and for manual inspection, diffing, and legible archival. USDC is binary (efficient for heavy data), and USDZ is a delivery archive that is generally read-only. | `stage-setting/usd-file-formats.md` |
| 26 | **A** | Data Exchange | The exercise creates a /World Xform, sets it as defaultPrim, then uses Usd.NamespaceEditor.ReparentPrim(prim, world_prim) followed by ApplyEdits() to move each root prim under /World. The other APIs serve unrelated purposes. | `data-exchange/data-transformation/transformation-hierarchy.md` |
| 27 | **A** | Pipeline Development | Choosing naming conventions that convey importance and intent to downstream users (capitalized = public, underscored = internal) is a Legibility tip: names clearly convey purpose and access intent even before opening the stage. | `asset-structure/asset-structure-principles/why-necessary.md` |
| 28 | **A** | Pipeline Development | Encapsulation requires that everything part of an asset rooted at an entry point - including relationship targets like material bindings - be a descendant of that entry-point prim. Only prims defined beneath `World` are brought over by the reference; the roof material sat outside, so its binding target was out of scope and ignored. | `asset-structure/asset-structure-principles/encapsulating-your-asset.md` |
| 29 | **A** | Pipeline Development | Library assets (like a palette of related materials) may not have a single entry point; for assets with a large number of entry points, Scope prims are used to group and organize those entry points. | `asset-structure/asset-structure-principles/asset-interface-pt2.md` |
| 30 | **A** | Pipeline Development | The "root" item in the tree view represents the stage's pseudo-root and is not a defined prim, so it is not referenceable. You must create an actual entry-point prim and reparent the root prims under it to make the asset referenceable as a unit. | `asset-structure/asset-structure-principles/your-first-asset.md` |
| 31 | **A, B** | Pipeline Development | Both variant selection and entry-point primvars are instancing-compatible, but variant selections generate new prototypes downstream while primvars do not. That makes primvars the lighter choice for a single continuous property (memory savings at the cost of extra material lookups). | `asset-structure/asset-parameterization/what-is-asset-parameterization.md` |
| 32 | **A** | Pipeline Development | The `asset_` prefix is used to avoid namespace collisions, since primvars are inherited down the hierarchy and could otherwise clash with other authored primvars. | `asset-structure/asset-parameterization/what-is-asset-parameterization.md` |
| 33 | **A** | Pipeline Development | Lofting is done with the lower-level Sdf API operating per-layer without composition: copy each property spec to the asset layer with `Sdf.CopySpec`, then remove it from the contents layer with `RemoveProperty`. There is no general built-in lofting utility; it is a site/project post-script. | `asset-structure/reference-payload-pattern/lofting-primvars.md` |
| 34 | **A** | Pipeline Development | The lofted variants do not contribute any opinions; they just advertise the sets and selections specified by the underlying contents payload, making them discoverable when the payload is unloaded. | `asset-structure/reference-payload-pattern/what-is-ref-payload-pattern.md` |
| 35 | **A, B** | Pipeline Development | Splitting modeling and surfacing into separate sparse layers lets each artist iterate independently and in parallel without blocking each other, and it offers performance benefits by reducing storage needs and publishing time. Layer stacks are not a replacement for version control, and splitting layers does not force instancing or reduce prim counts. | `asset-structure/workstreams/adding-user-workstreams.md` |
| 36 | **A** | Pipeline Development | The `unitsResolve` suffix is a naming convention that explains the op's purpose (a corrective for the up-axis change) to end users. It is an export option because end users may not want or need the transformation - the rest of a pipeline might be Z-up, but OBJ is Y-up by default. | `data-exchange/data-transformation/exercise-export-option.md` |
| 37 | **A** | Data Modeling | The lessons state there are two types of properties: attributes and relationships, and that properties contain the 'real data' while prims provide organization. A relationship is explicitly described as an alternative type of property to an attribute. | `stage-setting/properties/attributes.md` |
| 38 | **A** | Data Modeling | The relationships lesson states that when querying a relationship's targets, USD performs path translations, and relationships are robust against path translations; if a target prim's path changes due to editing or referencing, the relationship automatically remaps. | `stage-setting/properties/relationships.md` |
| 39 | **A, B** | Data Modeling | The prim-and-property-paths lesson states slashes separate prims (namespace), a period introduces a property, and curly brackets (not square brackets) indicate variants. Properties can be namespaced (e.g. userProperties:tag), so E is false. | `stage-setting/prim-property-paths.md` |
| 40 | **A** | Data Modeling | The paths lesson builds prim paths with AppendChild (e.g. world_path.AppendChild('Geometry')). AppendProperty builds a property path, GetParentPath navigates up, and IsPrimPath is a boolean check. | `stage-setting/prim-property-paths.md` |
| 41 | **A** | Data Modeling | The metadata lesson describes metadata as non-animatable name-value pairs attached to prims, properties, or layers, used for author info, dates, and rendering hints, without modifying the schema. Metadata cannot be time-sampled. | `stage-setting/metadata.md` |
| 42 | **A** | Data Modeling | The value-resolution lesson states dictionaries like customData combine element by element, so if one layer has keyOne and another keyTwo the result has both; shared keys resolve by strength. This differs from most metadata where the strongest opinion wins wholesale. | `beyond-basics/value-resolution.md` |
| 43 | **A, B** | Data Modeling | The schemas lesson states IsA schemas use the typeName metadata and each prim can subscribe to only one at a time, while API schemas do not assign a typeName, are list-edited in apiSchemas metadata, and are queryable via HasAPI. API schemas are applied to already-typed prims. | `scene-description-blueprints/schemas.md` |
| 44 | **A** | Data Modeling | The custom-properties lesson uses CreateAttribute with custom=True and a namespace prefix (e.g. my_namespace:) to flag non-schema attributes and avoid name clashes. A serial number is data, not a link, so a relationship is wrong, and typeName is unrelated. | `beyond-basics/custom-properties.md` |
| 45 | **A** | Data Modeling | The primvars lesson defines uniform interpolation as one value per face. A 2-face mesh therefore needs 2 values. Constant=1 (whole gprim), vertex=per point, faceVarying=per face-corner. | `beyond-basics/primvars.md` |
| 46 | **A** | Debugging & Troubleshooting | The lesson states composition is cached (USD builds a per-prim index of sources when the stage opens), while value resolution is NOT pre-calculated and runs on each Get to keep memory low; for repeated reads of the same attribute it recommends caching yourself with UsdAttributeQuery. | `beyond-basics/value-resolution.md` |
| 47 | **A** | Debugging & Troubleshooting | The lesson shows attr.Get() is equivalent to attr.Get(Usd.TimeCode.Default()), which returns the authored default (non-time-varying) value. It explicitly advises using Usd.TimeCode.EarliestTime() (or a specific time code) to get actual animated values rather than just the default. | `beyond-basics/value-resolution.md` |
| 48 | **A, B** | Debugging & Troubleshooting | MuteLayer removes a layer from composition non-destructively; IsLayerMuted reports the state and the opinions return once unmuted. Muting neither deletes the layer's specs nor deactivates prims authored in it. | `beyond-basics/value-resolution.md` |
| 49 | **A** | Debugging & Troubleshooting | Prims resolving to the class specifier are abstract. Default Traverse() yields active, loaded, defined, non-abstract prims, so abstract class prims are not visited (matching how render traversals ignore them). TraverseAll() visits everything regardless of the default predicate. | `beyond-basics/stage-traversal.md` |
| 50 | **A, B** | Debugging & Troubleshooting | LIVRPS strength ordering means a stronger layer/arc can override a weaker opinion, and a muted layer is excluded from composition entirely. Over opinions DO compose (they are non-destructive overrides), and value resolution is explicitly NOT cached, so C and D are false. | `beyond-basics/value-resolution.md` |
| 51 | **A** | Debugging & Troubleshooting | GetPropertyStack(time) returns the ordered (strong-to-weak) list of property specs contributing to a property at the given time, analogous to prim.GetPrimStack() for prims. It is the property-level tool for tracing value resolution sources during debugging. | `creating-composition-arcs/prim-composition.md` |
| 52 | **A** | Debugging & Troubleshooting | Sdf.ChangeBlock batches scene-description edits so change notifications and recomposition are deferred/coalesced, reducing per-edit overhead during bulk authoring. TfMallocTag is a memory profiler, MuteLayer excludes layers, and PruneChildren skips traversal branches, not authoring overhead. | `beyond-basics/value-resolution.md` |
| 53 | **A** | Debugging & Troubleshooting | The lesson states relationships can have multiple targets and USD combines all opinions following list-editing (list-op) rules rather than taking just the strongest opinion. The Example 2 result shows both LookA and LookB targets resolving because list editing merged them. | `beyond-basics/value-resolution.md` |
| 54 | **B** | Content Aggregation | "model" is an abstract kind from which group, assembly, and component inherit. The lesson states it should not be assigned to any prim; only its concrete subkinds are assignable. | `beyond-basics/model-kinds.md` |
| 55 | **C** | Content Aggregation | Subcomponent is the outlier that does not inherit from "model." Because a component cannot contain other component models as descendants, subcomponents mark important interior prims, and subcomponents can contain other subcomponents. | `asset-structure/model-hierarchy/what-are-model-kinds.md` |
| 56 | **C** | Content Aggregation | Instancing is driven by composition arcs. With instanceable = true but no references, payloads, or other arcs, there is nothing for OpenUSD to derive prototypes from, so zero prototypes are generated. | `asset-modularity-instancing/authoring-scenegraph-instancing/scenegraph-instancing-intro.md` |
| 57 | **B** | Content Aggregation | Kind is prim-level metadata, not a prim type. You author it through Usd.ModelAPI(prim).SetKind(Kind.Tokens.component); there is no UsdGeom.Component schema and SetTypeName sets the prim's schema type, not its kind. | `asset-structure/model-hierarchy/exercise-components.md` |
| 58 | **A, C** | Content Aggregation | Scenegraph instancing is composition-based with implicit prototypes and path-identifiable instances whose instanceable prim is editable while the proxy subgraph is read-only. Index identification and schema-based explicit prototypes describe point instancing. | `asset-modularity-instancing/instancing-faq.md` |
| 59 | **A** | Content Aggregation | protoIndices maps each point to an index into the prototypes relationship. Index 0 = Peanut_01 and index 1 = Peanut_02, so the first three points use Peanut_01 and the fourth uses Peanut_02. | `asset-modularity-instancing/authoring-point-instancing/point-instancing-intro.md` |
| 60 | **B** | Content Aggregation | Asset structures should encourage a shallow model hierarchy. Kind is read during composition for all model-hierarchy members, so a deep hierarchy (a gprim tagged as component) adds overhead and forgoes pruned-traversal performance gains. | `asset-structure/model-hierarchy/model-hierarchy-considerations.md` |
| 61 | **A** | Visualization | An Xform stores transformation data that applies to its child prims; transformations applied to a parent Xform affect all of its child prims down the hierarchy, enabling hierarchical grouping and movement. | `scene-description-blueprints/xform.md` |
| 62 | **A** | Visualization | GetXformOpOrderAttr() retrieves xformOpOrder, which lists the transform operations and the order they are applied. Different orders can yield different results, so this attribute is essential for understanding combined transforms. | `scene-description-blueprints/xform.md` |
| 63 | **A** | Visualization | A Scope is a grouping prim that does NOT have transformability; it cannot be transformed. XformCommonAPI works on UsdGeomXformable prims, so the compatibility check on a Scope fails and no transform is applied. | `scene-description-blueprints/scope.md` |
| 64 | **A** | Visualization | XformCommonAPI is a non-applied API schema that facilitates authoring/retrieving a common set of translate, rotate, scale, and pivot operations generally compatible with import/export into many tools, simplifying interchange. | `scene-description-blueprints/xformcommonapi.md` |
| 65 | **A** | Visualization | UsdShadeMaterial provides a container to store data for defining a shading material to a renderer; the actual shading computation is performed by Shader nodes contained within the material. | `scene-description-blueprints/materials-shaders.md` |
| 66 | **A, B** | Visualization | Material binding is a relationship named material:binding targeting a UsdShade.Material, authored/read via UsdShade.MaterialBindingAPI; GetDirectBinding().GetMaterial() returns the bound Material. The example binds GreenMat to two cubes, so binding is not one-prim-only, and it is not a float attribute. | `stage-setting/properties/relationships.md` |
| 67 | **A** | Customizing USD | The lesson states IsA schemas are derived from the core class UsdTyped, the base class for all typed schemas, which is why they are also called 'typed' schemas. UsdAPISchemaBase is the base for API schemas, not typed schemas. | `scene-description-blueprints/schemas.md` |
| 68 | **A** | Customizing USD | The lesson explains abstract/non-concrete schemas provide a name but no typeName, enabling them to serve as a base class for related concrete schemas (UsdGeomPointBased for UsdGeomMesh, UsdGeomBasisCurves). Concrete schemas provide both a name and a typeName and can be instantiated. | `scene-description-blueprints/schemas.md` |
| 69 | **A** | Customizing USD | The lesson warns that mixing internal taxonomies with the model hierarchy (which is core to composition) leads to complexity and portability problems. It recommends using custom properties, user properties, or schemas to describe taxonomies, and rarely (if ever) exposing them to the Kind library. | `asset-structure/model-hierarchy/model-hierarchy-considerations.md` |
| 70 | **A, C** | Customizing USD | The lesson states schemas are primarily data models with documented rules and do NOT necessarily include behavior implementation (UsdPhysics has no physics engine). Providing behaviors in the schema API is optional, and enforcement can be managed by other subsystems. B and D contradict this. | `scene-description-blueprints/schemas.md` |

> **Sourcing:** Authored for this repository to extend self-study — **not** official exam questions. Each item is grounded in the cited [Learn OpenUSD](https://docs.nvidia.com/learn-openusd/latest/index.html) lesson (the `docs/…` source of the corresponding published page). Domain weights and the study plan are in [`EXAM_PREP.md`](./EXAM_PREP.md); more practice in [`PRACTICE_QUESTIONS_EXTRA.md`](./PRACTICE_QUESTIONS_EXTRA.md).
