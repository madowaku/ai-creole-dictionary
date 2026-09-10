# Blender 3D Dialect

Project-local AI Creole vocabulary and handoff patterns for cleaning, structuring, and exporting 3D assets through Blender for downstream engines and tools.

This file does not change the universal Core. It defines a reusable DCC-side dialect for work between generation tools such as Tripo and runtime targets such as Godot, Unity, or Three.js.

## Design intent

Treat Blender as the pipeline checkpoint where a visually plausible model becomes a reproducible downstream asset.

```text
upstream_asset
  ↓
Blender inspection
  ↓
structure + transforms + origins
  ↓
materials + topology + animation
  ↓
export_profile
  ↓
downstream tool
  ↓
CHECK
```

Do not normalize everything blindly. Preserve working hierarchy, rigging, modifiers, and source editability when they matter. Apply or bake only what the downstream contract needs.

## MODE: blender_engine_asset

Use when Blender is preparing an asset for a real-time engine or interactive 3D target.

- inspect the incoming asset before destructive cleanup
- define the downstream target and export format first
- preserve source proportions and recognizable silhouette
- keep logical parts separate when downstream editing, collision, rigging, or replacement needs them
- make object names and collection structure readable
- verify object origins and transform state deliberately
- apply rotation or scale only when required by the pipeline; do not blanket-apply transforms to rigs or animated assets
- resolve modifier behavior before export: keep editable when useful, bake only when the receiving format or tool requires evaluated geometry
- prefer portable material setups for engine-bound assets
- export only the intended asset set
- reopen or import the exported file in the receiving tool before calling the asset done

## Dialect terms

### `upstream_asset`

The model received from a generator, marketplace, scan, another DCC, or an earlier Blender stage.

Examples:

```text
tripo_glb
photogrammetry_mesh
artist_blend
procedural_asset
```

### `logical_parts`

Parts that should remain independently selectable, replaceable, riggable, or assignable downstream.

Use object separation because the downstream task needs it, not merely to increase object count.

### `transform_contract`

The expected location, rotation, scale, and axis state at handoff.

Typical engine-bound intent:

```text
scale_verified
rotation_verified
no_accidental_nonuniform_scale
axis_conversion_understood
```

Blender's Apply operations transfer transform values into object data while preserving the visible result. This can affect modifiers, constraints, children, rigs, and animation, so transform application is a deliberate pipeline operation rather than universal cleanup.

### `origin_contract`

The intended object-origin placement for downstream transforms, snapping, animation, attachment, or spawning.

Examples:

```text
base_center
center_of_mass
hinge_point
wheel_axle
character_root
```

A visually correct mesh with a bad origin can still be a bad game asset.

### `modifier_policy`

Whether modifiers must remain editable in the `.blend` source or be evaluated into export geometry.

Common values:

```text
keep_editable
apply_selected
export_evaluated
```

The Blender glTF exporter can export the evaluated result of modifiers. Decide this explicitly so the source file and exported file do not drift by accident.

### `material_portable`

A material setup intentionally limited to features that survive the chosen export format and downstream renderer.

For glTF-bound work, prefer material and texture choices that map cleanly to glTF/PBR behavior. Validate the exported result rather than assuming the Blender viewport is authoritative.

### `export_set`

The exact objects or collection intended for export.

```text
character_only
vehicle_collection
selected_environment_props
```

Avoid exporting cameras, lights, helpers, hidden experiments, or unrelated scene objects unless the downstream target explicitly needs them.

### `export_profile`

A repeatable bundle of exporter choices for a specific destination.

May include:

```text
format: glb
selection_scope: export_set
modifiers: evaluated
uvs: yes
normals: yes
animation: named_actions
axis: gltf_y_up
```

Store the intent in project documentation or automation when repeatability matters.

### `animation_export_set`

The actions or animation tracks intended to cross the handoff boundary.

Name actions clearly and verify the exporter is actually including them. Blender's glTF workflow can derive exported animations from active actions and NLA tracks, so the intended set should be explicit.

### `engine_ready_export`

An exported asset that has passed both DCC-side checks and a downstream import smoke test.

It is not synonymous with “export command succeeded.”

## Standard checks

### Static asset

```text
CHECK:
- silhouette_matches_source
- dimensions_verified
- object_names_readable
- logical_parts_preserved
- origins_intentional
- transforms_understood
- normals_clean
- uv_state_valid
- material_export_valid
- export_set_contains_only_intended_objects
- downstream_import_passes
```

### Animated asset

```text
CHECK:
- skeleton_hierarchy
- rest_pose
- mesh_skin_binding
- transform_application_did_not_break_rig
- animation_export_set
- clip_names
- looping_intent
- downstream_animation_playback
```

### Runtime-sensitive asset

```text
CHECK:
- polygon_budget
- material_count
- texture_sizes
- draw_call_risk
- collision_proxy_plan
- lod_plan_if_needed
- downstream_runtime_smoke
```

## Standard handoff: Tripo → Blender

```text
ROLE: 3d_asset_agent
MODE: blender_engine_asset

TASK: prepare_generated_asset
GOAL: editable_engine_ready_asset
STATE: upstream_asset_from_tripo
INPUT: tripo_export
TARGET: blender_scene

KEEP:
- recognizable_silhouette
- required_logical_parts
- useful_material_information

DO:
- inspect geometry
- verify dimensions
- repair obvious mesh_failures
- organize logical_parts
- rename objects
- set origin_contract
- define transform_contract
- choose modifier_policy
- simplify only where downstream use benefits
- define export_set
- define export_profile

NO:
- blind_transform_apply
- merge_everything
- destructive_detail_loss_without_reason

CHECK:
- topology
- normals
- transforms
- origins
- materials
- export

OUT:
- blender_source
- engine_ready_export

NEXT: downstream_import_test
```

## Standard handoff: Blender → Godot

```text
ROLE: asset_pipeline_agent
MODE: blender_engine_asset

TASK: export_for_godot
GOAL: reimport_safe_engine_asset
TARGET: godot

KEEP:
- logical_parts_needed_by_gameplay
- skeleton_and_animation_names
- agreed_scale

DO:
- verify transform_contract
- verify origin_contract
- select export_set
- export glb_or_gltf
- import into Godot
- create or update import_recipe
- smoke_test reimport

CHECK:
- scale
- orientation
- materials
- node_hierarchy
- skeleton
- animation
- reimport_safe

OUT:
- blender_source
- engine_ready_export
- godot_import_recipe
```

## Repair rule

When a downstream problem originates in geometry, hierarchy, origin, UVs, normals, rigging, or material source data, prefer fixing it in the Blender source and re-exporting rather than stacking fragile engine-side corrections.

When a problem is genuinely runtime-specific, keep it in the engine wrapper instead of baking game logic into the DCC asset.

This creates a clean boundary:

```text
Blender owns authored asset truth
Godot owns gameplay behavior
```

## Source notes

This dialect's Blender behavior is based on Blender 4.5 LTS documentation for object transforms/origins and the glTF 2.0 importer/exporter. The AI Creole terms, modes, checks, and pipeline boundaries are madowaku-derived organization rather than Blender terminology.

Primary references:

- https://docs.blender.org/manual/en/4.5/scene_layout/object/editing/apply.html
- https://docs.blender.org/manual/en/4.5/scene_layout/object/origin.html
- https://docs.blender.org/manual/en/4.5/addons/import_export/scene_gltf2.html
