# Tripo 3D Dialect

Project-local AI Creole vocabulary and handoff patterns for generating 3D assets with Tripo and preparing them for downstream DCC and game-engine use.

This file does not change the universal Core. It defines reusable production language around generation, editability, topology, texturing, rigging, and export.

## Design intent

Do not ask only for a model that looks good in Tripo. Ask for an asset that survives the next tool.

```text
idea_or_reference
  ↓
generation_source
  ↓
model_generation
  ↓
segmentation / retopology / texture / rigging
  ↓
export
  ↓
DCC_or_engine
  ↓
CHECK
```

Tripo's current product pipeline supports model generation followed by optional segmentation, retopology, texturing, rigging/animation, and export. These stages can be combined as needed rather than treated as one mandatory sequence.

## MODE: tripo_game_asset

Use when the output is intended for a real-time engine or interactive scene.

- define downstream engine or DCC target before generation
- prioritize silhouette and structure before decorative detail
- use image or multi-view input when shape control matters more than free exploration
- request logical part separation when editability matters
- choose topology for runtime needs, not maximum geometric detail by default
- add texture and rigging only when downstream use needs them
- choose export format for the receiving tool
- test the exported asset in the receiving tool before calling it done

## Dialect terms

### `generation_source`

The source used to create the model.

Common values:

```text
text
single_image
multi_view
```

Tripo's help guidance recommends image-based generation when tighter shape control is needed, and suggests multi-view references for more accurate 3D structure.

### `structure_priority`

Instruction that recognizable shape, proportion, and part arrangement outrank tiny surface detail.

### `silhouette_priority`

Instruction that the asset should read clearly from its major outline before dense detail is added.

### `logical_parts`

Parts that should remain independently editable, replaceable, or riggable.

### `segmented_asset`

A generated model that has been separated into editable components using Tripo segmentation or equivalent downstream cleanup.

### `retopo_target`

The intended topology budget or topology style for downstream use.

Examples:

```text
realtime_lowpoly
quad_mesh
custom_polygon_budget
```

### `smart_mesh`

Tripo's product term for an optimized real-time-oriented mesh workflow. Current Tripo documentation describes Smart Mesh as producing structured topology with lightweight geometry for game engines and other performance-sensitive use cases.

Use this as a Tripo-specific term, not as a universal AI Creole concept.

### `texture_ready`

State where texture output is suitable for the next renderer or engine and required material maps have been checked.

### `rig_ready`

State where skeleton and skinning exist and can be tested in the downstream animation workflow.

### `export_format`

Explicit receiving-file format.

Tripo currently documents six export formats:

```text
GLB
USD
FBX
OBJ
STL
3MF
```

For game-engine handoff, GLB and FBX are common candidates. Choose based on the receiving tool and actual animation/material requirements.

### `engine_target`

The destination runtime that determines topology, scale, materials, animation, and export choices.

Examples:

```text
godot
unity
unreal
threejs
```

### `variation_batch`

Several generations using the same structural brief to explore stochastic alternatives before committing to cleanup.

### `assumption_list`

Explicit list of geometry or structure inferred from incomplete references.

## Generation policy

### Text for exploration, image for control

Tripo's Text-to-3D guidance recommends clear descriptions of style, material, and structure. It also recommends switching to image-driven generation when more output control is needed, with multi-view references as a further aid to structural accuracy.

```text
TASK: generate_asset
GOAL: controllable_shape
DO:
- define structure
- define material
- define style
- prefer image_or_multiview when exact_shape_matters
CHECK:
- silhouette
- proportions
- recognizable_parts
```

### Generate for the downstream target

Do not maximize fidelity automatically.

```text
TASK: game_asset
TARGET: <engine_target>
DO:
- generate recognizable form
- separate logical_parts if needed
- retopologize for runtime
- texture for target renderer
- rig only if animation is required
CHECK:
- polygon_budget
- editability
- materials
- export
- runtime_import
```

### Keep editability intentional

Tripo currently supports model segmentation into editable parts. Use it when the next stage needs selective material edits, part replacement, mechanical motion, or easier rigging.

```text
TASK: editable_generated_asset
DO:
- identify logical_parts
- segment where useful
- preserve part identity
NO:
- arbitrary fragmentation
CHECK:
- part_editability
- assembly_integrity
```

### Treat topology as a target, not cleanup debt

Tripo currently supports retopology options including custom polygon counts and quad conversion, and documents Smart Mesh for real-time-oriented optimized topology.

```text
TASK: realtime_topology
GOAL: engine_ready_geometry
DO:
- choose retopo_target
- preserve silhouette
- reduce invisible_detail
CHECK:
- polygon_budget
- deformation_if_rigged
- silhouette
- runtime_cost
```

## Standard pattern: Tripo → Blender → Godot

```text
ROLE: asset_agent
MODE: tripo_game_asset
TASK: generate_game_asset
GOAL: editable_engine_ready_asset
CONTEXT:
- final runtime is Godot
INPUT:
- reference_or_prompt
TARGET:
- Tripo
- Blender
- Godot
DO:
- choose generation_source
- generate several useful variations if needed
- select best structure
- segment logical_parts if editing requires it
- retopologize for realtime
- create required textures
- rig only if animation is required
- export GLB unless pipeline requires another supported format
- clean pivots scale naming and materials in Blender if needed
- import into Godot
- run and inspect
KEEP:
- silhouette
- required proportions
- recognizable_parts
NO:
- maximum geometry without downstream reason
- hidden assumptions about missing geometry
CHECK:
- structure
- logical_parts
- polygon_budget
- texture_ready
- rig_ready_if_required
- scale
- orientation
- Godot_import
- playable_baseline
OUT:
- source_reference
- selected_generation
- engine_ready_asset
- assumption_list_if_needed
NEXT: short_revision_loop
```

## Standard pattern: reference image → prop

```text
MODE: tripo_game_asset
TASK: reference_to_prop
GOAL: recognizable_editable_prop
INPUT: reference_image
DO:
- use image_to_3d
- preserve silhouette
- preserve major proportions
- separate functional_parts if required
- retopologize for engine_target
- texture
CHECK:
- reference_similarity
- silhouette
- editability
- polygon_budget
OUT: engine_ready_asset
```

## Standard pattern: stylized character

```text
MODE: tripo_game_asset
TASK: stylized_character
GOAL: animation_ready_character
DO:
- lock silhouette_and_proportions
- generate character
- retopologize for deformation
- texture
- rig
- export supported animated format
CHECK:
- joint_placement
- skin_deformation
- material_integrity
- engine_import
RISK:
- topology_breaks_deformation
- accessories_merge_into_body
```

## Acceptance checks

Pick only what the task needs.

```text
CHECK:
- silhouette
- proportions
- structure
- assumption_list
- logical_parts
- polygon_budget
- topology
- normals
- uv
- texture_ready
- rig_ready
- scale
- orientation
- export_format
- downstream_import
- runtime_cost
```

## Godot handoff note

For the Godot-specific receiving side, pair this dialect with `godot.md`.

A compact cross-tool handoff can be:

```text
MODE: tripo_game_asset
TASK: make_godot_prop
GOAL: playable_reimport_safe_asset
TARGET: tripo -> blender -> godot
KEEP:
- silhouette
- proportions
DO:
- image_to_3d
- retopo realtime
- export GLB
- cleanup Blender
- import Godot
- configure collision_proxy
- run
CHECK:
- polygon_budget
- scale
- materials
- collision
- reimport_safe
```

## Source notes

Tripo-derived behavior and product capabilities in this file are based on current official Tripo Help Center and feature documentation. The dialect names, acceptance language, and AI Creole mappings are madowaku-derived organization.

References:

- https://www.tripo3d.ai/help/features/how-to-use-the-text-to-3d-feature
- https://www.tripo3d.ai/help/features/how-to-use-the-image-to-3d-feature
- https://www.tripo3d.ai/help/getting-started/what-features-does-tripo-have
- https://www.tripo3d.ai/help/features/what-is-smart-mesh
- https://www.tripo3d.ai/help/features/can-tripo-create-animation-ready-3d-models
- https://www.tripo3d.ai/help/getting-started/what-3d-file-formats-do-you-support
