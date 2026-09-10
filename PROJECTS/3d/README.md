# 3D Dialects

Reusable AI Creole references for 3D generation, asset preparation, engine integration, and playable verification.

The universal Core remains unchanged. These files are examples and reusable project-local dialects.

## Files

- `3D_PROMPT_PATTERNS.md` — cross-tool production patterns and standard templates
- `tripo.md` — generation, segmentation, retopology, texturing, rigging, export, and downstream handoff
- `blender.md` — DCC cleanup, logical parts, transforms, origins, modifiers, materials, animation, and repeatable engine export
- `godot.md` — import recipes, reimport-safe integration, gameplay wrappers, collision/navigation, and playable checks

Planned candidates:

- `unity.md`
- `threejs.md`

## Recommended composition

Do not copy every term into every project. Pull only the dialect pieces that reduce ambiguity.

### Fast line: Tripo → Godot

Use when the generated export already satisfies the downstream contract and no DCC repair is needed.

```text
reference_or_idea
  ↓
tripo.md
  ↓
engine_ready_asset
  ↓
godot.md
  ↓
playable_baseline
  ↓
CHECK
```

### Standard line: Tripo → Blender → Godot

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
Godot import_recipe
  ↓
gameplay_wrapper
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
```

This is a default boundary, not a rigid law. Skip a stage when it adds no useful control.

## Promotion rule

A 3D term should remain local until repeated real projects show that it reduces handoff ambiguity across tools or projects. Product-specific terms such as `smart_mesh` should remain product dialect terms rather than universal Core vocabulary.
