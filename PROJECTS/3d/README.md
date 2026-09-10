# 3D Dialects

Reusable AI Creole references for 3D generation, asset preparation, engine integration, and playable verification.

The universal Core remains unchanged. These files are examples and reusable project-local dialects.

## Files

- `3D_PROMPT_PATTERNS.md` — cross-tool production patterns and standard templates
- `tripo.md` — generation, segmentation, retopology, texturing, rigging, export, and downstream handoff
- `blender.md` — DCC cleanup, logical parts, transforms, origins, modifiers, materials, animation, and repeatable engine export
- `godot.md` — import recipes, reimport-safe integration, gameplay wrappers, collision/navigation, and playable checks
- `unity.md` — model import profiles, prefabs, collider strategy, rig/animation, materials, reimport safety, and Play Mode checks

Planned candidate:

- `threejs.md`

## Recommended composition

Do not copy every term into every project. Pull only the dialect pieces that reduce ambiguity.

### Fast line: Tripo → engine

Use when the generated export already satisfies the downstream contract and no DCC repair is needed.

```text
reference_or_idea
  ↓
tripo.md
  ↓
engine_ready_asset
  ↓
Godot OR Unity
  ↓
playable_baseline
  ↓
CHECK
```

### Standard line: Tripo → Blender → engine

Use Blender as the checkpoint when structure, topology, origins, transforms, materials, rigging, or export reproducibility need control.

```text
reference_or_idea
  ↓
Tripo generation
  ↓
upstream_asset
  ↓
Blender blender_engine_asset
  ↓
engine_ready_export
  ↓
┌──────────────────────┬──────────────────────┐
│ Godot                │ Unity                │
│ import_recipe        │ model_import_profile │
│ gameplay_wrapper     │ gameplay_prefab      │
└──────────────────────┴──────────────────────┘
  ↓
playable_baseline
  ↓
CHECK
```

## Boundary rule

Keep responsibility crisp:

```text
Tripo   owns generated starting geometry
Blender owns authored asset truth
Godot   owns import recipe + gameplay behavior
Unity   owns model import profile + prefab/gameplay behavior
```

For Unity specifically, keep imported representation and gameplay configuration separate when practical:

```text
model_source
  ↓
model_import_profile
  ↓
model_prefab
  ↓
gameplay_prefab
```

This is a default boundary, not a rigid law. Skip a stage when it adds no useful control.

## Engine choice does not change the upstream contract

Where possible, keep shared upstream requirements engine-neutral:

```text
scale_contract
origin_contract
logical_parts
material_portable
animation_export_set
engine_ready_export
```

Then express engine-specific interpretation downstream:

```text
Godot → import_recipe / gameplay_wrapper
Unity → model_import_profile / gameplay_prefab
```

This lets the same Blender-authored asset feed multiple engines without turning the DCC file into an engine-specific tangle.

## Promotion rule

A 3D term should remain local until repeated real projects show that it reduces handoff ambiguity across tools or projects. Product-specific terms such as `smart_mesh` should remain product dialect terms rather than universal Core vocabulary.
