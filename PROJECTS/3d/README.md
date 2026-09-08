# 3D Dialects

Reusable AI Creole references for 3D generation, asset preparation, engine integration, and playable verification.

The universal Core remains unchanged. These files are examples and reusable project-local dialects.

## Files

- `3D_PROMPT_PATTERNS.md` — cross-tool production patterns and standard templates
- `tripo.md` — generation, segmentation, retopology, texturing, rigging, export, and downstream handoff
- `godot.md` — import recipes, reimport-safe integration, gameplay wrappers, collision/navigation, and playable checks

Planned candidates:

- `blender.md`
- `unity.md`
- `threejs.md`

## Recommended composition

Do not copy every term into every project. Pull only the dialect pieces that reduce ambiguity.

### Tripo → Godot asset line

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

### With Blender cleanup

```text
reference_or_idea
  ↓
Tripo generation
  ↓
Blender cleanup
  ↓
Godot import_recipe
  ↓
gameplay_wrapper
  ↓
playable_baseline
```

## Promotion rule

A 3D term should remain local until repeated real projects show that it reduces handoff ambiguity across tools or projects. Product-specific terms such as `smart_mesh` should remain product dialect terms rather than universal Core vocabulary.
