# Godot 3D Dialect

Project-local AI Creole vocabulary and handoff patterns for Godot 3D asset import, scene integration, and playable verification.

This file does not change the universal Core. It defines a reusable 3D dialect that projects may copy into their own `AI_CREOLE.md` when useful.

## Design intent

Treat a 3D asset as a source-controlled pipeline, not as a one-time editor result.

```text
DCC_source
  ↓
import_recipe
  ↓
Godot_imported_scene
  ↓
gameplay_wrapper
  ↓
playable_baseline
  ↓
CHECK
```

Godot's documentation recommends changing original 3D source data when practical rather than accumulating avoidable post-import differences. Imported scene configuration can then be expressed through the Import dock, Advanced Import Settings, and import hints.

## MODE: godot_3d_safe

Use for AI-assisted Godot 3D asset integration.

- inspect the existing scene and asset pipeline first
- preserve DCC source as the upstream source of truth
- prefer reproducible import settings over manual one-off editor fixes
- keep imported asset structure separate from gameplay-specific wrapping when practical
- preserve working controls and game loop during visual asset replacement
- run after import-affecting changes
- verify reimport does not silently break materials, collisions, skeletons, or node references
- report remaining editor-only checks

## Dialect terms

### `dcc_source`

Original Blender, glTF/GLB, FBX, or other authored/generated source used to produce the Godot import.

### `import_recipe`

The settings and naming conventions required to reproduce the intended Godot import.

May include:

- Import dock settings
- Advanced Import Settings
- name suffix import hints
- material extraction or overrides
- collision generation
- navigation generation

### `reimport_safe`

A state where changing and reimporting the upstream asset does not destroy required Godot-side behavior or references.

### `gameplay_wrapper`

A Godot scene that instances or contains the imported asset and owns game-specific nodes, scripts, interactions, effects, or state.

### `collision_proxy`

Collision geometry intentionally chosen for gameplay and runtime cost instead of blindly matching render geometry.

### `navmesh_source`

Geometry or import configuration used to generate navigation data.

### `import_hint`

A naming convention embedded in the DCC source that asks Godot's importer to perform a known action.

Godot supports name suffix workflows for tasks such as collisions, navigation, removing nodes, rigid bodies, and animation looping. Use only suffixes supported by the target Godot version.

### `playable_baseline`

The smallest imported scene that launches and supports the required input → response → game-loop path.

### `visible_failures`

Failures observable in a run or editor preview, such as:

- wrong scale or orientation
- missing materials
- broken skeleton or animation
- bad collision
- camera clipping
- unreachable navigation
- obvious frame-rate regression

## Import policy

### Prefer glTF/GLB for portable handoff

Godot's stable documentation describes glTF as the underlying path for `.blend` import as well. Direct `.blend` import is useful for fast iteration when Blender is available, but it adds a team/tooling dependency and is unavailable in Android and web editors. For portable handoff, exported glTF/GLB is the safer default.

```text
GOAL: portable_3d_handoff
TARGET: godot
DO:
- prefer gltf_or_glb
- preserve source_asset
- document import_recipe
CHECK:
- imports_without_local_only_dependency
```

### Prefer source fixes over import hacks

When the same issue can be fixed cleanly in the DCC source, prefer that upstream fix.

```text
STATE: import_mismatch
DO:
- identify source_vs_import cause
- fix dcc_source when practical
- reimport
CHECK:
- mismatch_resolved
- reimport_safe
```

### Use per-object import configuration deliberately

Godot's Advanced Import Settings can configure individual nodes, meshes, materials, animation, generated physics, and navigation-related behavior. Do not generate expensive collision or navigation blindly.

```text
TASK: configure_import
DO:
- inspect per_object needs
- generate collision only where required
- generate navmesh only where required
- externalize materials when Godot-side editing is required
CHECK:
- collision_cost
- navigation
- material_override_survives_reimport
```

## Naming hints

Godot supports import behavior triggered by source names. Examples in current Godot 4.x documentation include conventions for collision, navigation, removing nodes, rigid bodies, and looping animations.

Use them as a reproducible pipeline tool, not as hidden magic.

```text
TASK: encode_import_intent
TARGET: dcc_source
DO:
- use supported name_suffixes
- document suffix meaning
CHECK:
- expected node_type_after_import
- reimport_same_result
RISK:
- version_specific_suffix_behavior
```

## Standard pattern: generated asset → Godot

```text
ROLE: Codex
MODE: godot_3d_safe
TASK: integrate_generated_asset
GOAL: playable_reimport_safe_asset
CONTEXT:
- asset comes from Tripo or Blender cleanup
INPUT:
- engine_ready_glb
TARGET:
- Godot project
DO:
- inspect existing asset conventions
- import asset
- verify scale and orientation
- configure materials
- configure collision_proxy
- instance asset in gameplay_wrapper
- run project
- inspect visible_failures
- fix highest_impact failures
KEEP:
- existing core_loop
- existing controls
- upstream source_asset
NO:
- destructive edits to generated import cache
- broad gameplay refactor
CHECK:
- import
- scale
- orientation
- materials
- collisions
- node_references
- playable_baseline
- reimport_safe
OUT:
- changed_files
- import_recipe
- checks
- remaining_editor_check
NEXT: short_revision_loop
```

## Standard pattern: replace greybox art without changing play

```text
MODE: godot_3d_safe
TASK: replace_greybox_art
GOAL: themed_playable_build
TARGET: existing_scene
KEEP:
- core_loop
- controls
- rules
- collision_intent
DO:
- swap visual assets
- preserve gameplay_wrapper
- retune collision only if silhouette requires it
- run
- fix visible_failures
CHECK:
- no_core_regression
- scale
- collisions
- camera
- target_fps
```

## Acceptance checks

Pick only what the task needs.

```text
CHECK:
- project_launches
- imported_scene_exists
- scale
- orientation
- materials
- skeleton
- animation
- collisions
- navigation
- camera
- controls
- node_references
- reimport_safe
- target_fps
- restart
```

## Source notes

Godot-derived behavior in this file is based on current official documentation for importing 3D scenes, import configuration, Advanced Import Settings, available formats, and node-type customization. The dialect names and AI Creole mappings are madowaku-derived organization.

References:

- https://docs.godotengine.org/en/stable/tutorials/assets_pipeline/importing_3d_scenes/index.html
- https://docs.godotengine.org/en/stable/tutorials/assets_pipeline/importing_3d_scenes/available_formats.html
- https://docs.godotengine.org/en/stable/tutorials/assets_pipeline/importing_3d_scenes/import_configuration.html
- https://docs.godotengine.org/en/stable/tutorials/assets_pipeline/importing_3d_scenes/advanced_import_settings.html
- https://docs.godotengine.org/en/4.6/tutorials/assets_pipeline/importing_3d_scenes/node_type_customization.html
