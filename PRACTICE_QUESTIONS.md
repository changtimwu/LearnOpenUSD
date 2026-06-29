# OpenUSD Development Certification — Practice Questions

The 13 official sample questions from the **NCP: OpenUSD Development Exam Study Guide v1.1.0**, reproduced for self-testing. Each is tagged with its exam domain (see [`EXAM_PREP.md`](./EXAM_PREP.md) for the domain weights and study plan).

**How to use:** Answer all 13 first *without* scrolling to the [Answer Key](#answer-key). Then check yourself and re-study any domain you missed. Watch the **"Select N options"** prompts — several questions are multi-select.

---

## Question 1 — Composition

What is the primary purpose of the `defaultPrim` metadata in a USD layer?

- **A.** It specifies which prim is used as the root when the layer is referenced.
- **B.** It defines which material is used as the default for geometry in the layer.
- **C.** It sets which timeCode is used as the default for animation in the layer.
- **D.** It specifies the default prim type when defining a new custom IsA schema.

---

## Question 2 — Composition

Given the following USD layer structure, what will be the final composed value of the `xformOp:translate` attribute on `/World/Chair` on a stage with root layer `scene.usda`?

`chair_base.usda`
```usda
#usda 1.0

def Xform "Chair" {
    double3 xformOp:translate = (0, 0, 0)
    uniform token[] xformOpOrder = ["xformOp:translate"]
}
```

`chair_repositioned.usda`
```usda
#usda 1.0

def Xform "Chair" {
    double3 xformOp:translate = (1, 0, 0)
}
```

`scene.usda`
```usda
#usda 1.0

def Xform "World" {
    def Xform "Chair" (
        prepend references = [@./chair_base.usda@, @./chair_repositioned@]
    ) {
        double3 xformOp:translate = (0, 1, 0)
    }
}
```

- **A.** (0, 0, 0)
- **B.** (1, 0, 0)
- **C.** (0, 1, 0)
- **D.** (1, 1, 0)

---

## Question 3 — Data Modeling

Which of the following attributes are required to properly define a `UsdGeomMesh` prim? *(Select three options.)*

- **A.** `points`
- **B.** `faceVertexIndices`
- **C.** `primvars:displayColor`
- **D.** `extent`
- **E.** `normals`

---

## Question 4 — Data Modeling

You have a `UsdGeomMesh` with 1,000 vertices and 500 faces. The mesh uses `faceVertexIndices` to define triangular faces. What should be the length of the `faceVertexIndices` array?

- **A.** 500 — one index per face
- **B.** 1,000 — one index per vertex
- **C.** 1,500 — three indices per triangular face
- **D.** 2,000 — four indices per face (including winding order)

---

## Question 5 — Pipeline Development

When creating a procedural mesh in USD, which attributes must be kept synchronized to ensure the mesh remains valid? *(Select two options.)*

- **A.** `points` and `extent` — when vertex positions change
- **B.** `faceVertexIndices` and `faceVertexCounts` — when topology changes
- **C.** `normals` and `primvars:displayColor` — when shading changes
- **D.** `purpose` and `visibility` — when rendering properties change

---

## Question 6 — Customizing USD

You want to create a custom USD schema that adds physics properties to geometry prims. Which base class should your schema inherit from?

- **A.** `UsdSchemaBase`
- **B.** `UsdTyped`
- **C.** `UsdAPISchemaBase`
- **D.** `UsdPhysicsBase`

---

## Question 7 — Pipeline Development

What are some primary advantages of using USD's composition system in a production pipeline? *(Select two options.)*

- **A.** Layers enable version control and tracking of asset changes across the pipeline.
- **B.** Layers allow multiple users to collaborate on the same scene without conflicts.
- **C.** Layers support nondestructive editing and make it easy to revert or update changes.
- **D.** Layers improve scene performance by optimizing data organization and access.

---

## Question 8 — Pipeline Development

When designing a USD-based pipeline, which architectural decisions are most critical for long-term success? *(Select two options.)*

- **A.** Establishing clear and consistent asset naming conventions and directory structures.
- **B.** Implementing comprehensive error handling and validation at all pipeline boundaries.
- **C.** Defining an asset structure once that will meet all present and future requirements.
- **D.** Requiring that all content is always authored, managed, and encoded in USD format.

---

## Question 9 — Visualization

What is the primary purpose of the `UsdGeomImageable` schema in USD?

- **A.** It defines common geometric properties for 3D objects.
- **B.** It provides common properties for objects that can be rendered.
- **C.** It manages how materials are assigned to geometric primitives.
- **D.** It defines properties for texture image data for 3D objects.

---

## Question 10 — Visualization

You want to create a material that can be easily customized with different colors and textures. Which USD shading approach would be most appropriate?

- **A.** Create a single material with hardcoded shader parameters.
- **B.** Create a material with exposed parameters that can be overridden.
- **C.** Create separate materials for each color/texture combination.
- **D.** Use only the default material and modify it at render time.

---

## Question 11 — Content Aggregation

What happens when you reference a USD file with `metersPerUnit = 1.0` (meters) into a stage with `metersPerUnit = 0.01` (centimeters)?

- **A.** The USD runtime automatically rescales the referenced geometry to match the stage's unit system.
- **B.** The referenced geometry appears 100 times smaller than intended, as USD does not automatically convert units.
- **C.** The referenced geometry appears 100 times larger than intended, due to USD scaling it up automatically.
- **D.** The referenced geometry is not composed onto the stage, because the unit systems do not match.

---

## Question 12 — Data Exchange

The following is a snippet of a USDA layer output by a new digital content creation (DCC) application for an exported asset. What is the error with the material binding in this export?

```usda
#usda 1.0
(
    defaultPrim = "World"
    metersPerUnit = 0.01
    upAxis = "Z"
)

def Xform "World" (
    kind = "component"
)
{
    def Mesh "bolt" (
        prepend apiSchemas = ["MaterialBindingAPI"]
    )
    {
        rel material:binding = </Materials/metal> (
            bindMaterialAs = "weakerThanDescendants"
        )
        # Mesh definition...
    }
}

def Scope "Materials"
{
    def Material "metal" { # Material definition... }
}
```

- **A.** The use of `bindMaterialAs = "weakerThanDescendants"` is not valid in this context.
- **B.** The material binding is applied to the Mesh prim, but it should be applied to a Subset instead.
- **C.** The material binding is targeting a prim that is outside the hierarchy of the `defaultPrim`.
- **D.** The material binding data should go on the Material prim and target prims to bind to.

---

## Question 13 — Debugging and Troubleshooting

An artist reported that the MyBox asset in the `main.usda` stage does not change to blue even though they are setting the `loftedColor` variant set to the "blue" variant in that layer. Refer to the `main.usda`, `MyBox.usda`, and `contents.usda` below. Explain why the box is no longer changing to blue.

`main.usda`
```usda
#usda 1.0
(
    defaultPrim = "World"
)

def Xform "World"
{
    def Xform "MyBox" (
        prepend references = @./MyBox.usda@
        variants = {
            string loftedColor = "blue"
        }
    )
    {
        over "Cube" (
            variants = {
                string color = "red"
            }
        )
        {
        }
    }
}
```

`MyBox.usda`
```usda
#usda 1.0
(
    defaultPrim = "MyBox"
)

def Xform "MyBox" (
    prepend payload = @./contents.usda@
    prepend variantSets = "loftedColor"
    variants = {
        string loftedColor = "red"
    }
)
{
    variantSet "loftedColor" = {
        "red" {
            over "Cube" ( variants = { string color = "red" } ) {}
        }
        "blue" {
            over "Cube" ( variants = { string color = "blue" } ) {}
        }
    }
}
```

`contents.usda`
```usda
#usda 1.0
(
    defaultPrim = "MyBox"
)

def Xform "MyBox"
{
    def Cube "Cube" (
        prepend variantSets = "color"
        variants = {
            string color = "red"
        }
    )
    {
        variantSet "color" = {
            "red" {
                color3f[] primvars:displayColor = [(1, 0, 0)]
            }
            "blue" {
                color3f[] primvars:displayColor = [(0, 0, 1)]
            }
        }
    }
}
```

- **A.** The "red" variant selection in `contents.usda` is the strongest opinion and overrides any other opinions.
- **B.** The direct local opinion for the `color` variant selection in `main.usda` is stronger than the opinion of the `color` variant selection authored by `loftedColor`.
- **C.** The `loftedColor` variant set does not set `primvars:displayColor` for the Cube so there is no reason why changing `loftedColor` would change the Cube's color.
- **D.** Variant selections cannot be authored in `main.usda`. Variant selections must be made in `MyBox.usda` before it is referenced.

---

## Answer Key

| Q | Answer | Domain | Why |
|---|---|---|---|
| 1 | **A** | Composition | `defaultPrim` names the prim used as the root when the layer is referenced/payloaded without an explicit target path. |
| 2 | **C** | Composition | In **LIVRPS**, a **L**ocal opinion outranks **R**eferences. The root layer's local `(0, 1, 0)` wins over both referenced values. |
| 3 | **A, B, D** | Data Modeling | A valid `UsdGeomMesh` needs `points`, `faceVertexIndices` (+`faceVertexCounts`), and `extent`. `displayColor`/`normals` are optional. |
| 4 | **C** | Data Modeling | 500 triangles × 3 indices = **1,500**. `faceVertexIndices` length is the sum of `faceVertexCounts`, not the vertex count. |
| 5 | **A, B** | Pipeline Development | Geometry validity requires `points`↔`extent` (bounds follow positions) and `faceVertexIndices`↔`faceVertexCounts` (topology). |
| 6 | **C** | Customizing USD | Adding properties to *existing* prims = an **applied API schema**, which inherits from `UsdAPISchemaBase` (not a typed IsA schema). |
| 7 | **B, C** | Pipeline Development | Composition/layers enable concurrent collaboration and nondestructive editing. (A/D are workflow conventions, not intrinsic to composition.) |
| 8 | **A, B** | Pipeline Development | Naming/structure conventions and validation at boundaries scale well. C (one structure forever) and D (USD-only) are anti-patterns. |
| 9 | **B** | Visualization | `UsdGeomImageable` provides common properties for renderable objects (e.g. `visibility`, `purpose`). |
| 10 | **B** | Visualization | Expose/override material parameters — the reusable, data-driven approach vs. hardcoding or material explosion. |
| 11 | **B** | Content Aggregation | USD does **not** auto-convert units. A 1 m asset in a cm stage reads as 1 unit = 1 cm → appears **100× smaller**. |
| 12 | **C** | Data Exchange | `</Materials/metal>` lives outside the `defaultPrim` (`/World`), so it's dropped when the asset is referenced. Keep bound prims under the default prim. |
| 13 | **B** | Debugging & Troubleshooting | The **local** `color` variant selection (`red`) authored directly on the `over "Cube"` in `main.usda` is stronger than the selection set indirectly via the `loftedColor` variant. |

> **Source:** NVIDIA-Certified Professional: OpenUSD Development Exam Study Guide v1.1.0, "Certification Sample Questions" (pp. 10–18). The *Why* column is added explanatory context, not part of the official guide.
