# Unity 3D Dialect

Project-local AI Creole vocabulary and handoff patterns for Unity 6 model import, prefab integration, physics, animation, materials, reimport safety, and Play Mode verification.

This file does not change the universal Core. It defines reusable Unity-side production language that projects may copy into their own `AI_CREOLE.md` when useful.

## Design intent

Treat imported 3D files as upstream assets and Unity objects as a reproducible integration layer.

```text
DCC_source
  ↓
model_import_profile
  ↓
model_prefab
  ↓
gameplay_prefab
  ↓
playmode_baseline
  ↓
CHECK
```

Unity automatically imports supported model files placed under the project `Assets` folder and exposes model Import Settings in the Inspector. Prefabs then provide a reusable GameObject configuration layer whose instances can stay synchronized with the prefab asset.

## MODE: unity_3d_safe

Use for AI-assisted Unity 3D asset integration.

- inspect the existing asset, prefab, scene, and importer setup first
- preserve the upstream DCC source as the authored geometry source when practical
- express model interpretation through repeatable Import Settings rather than scene-local repair
- separate imported model structure from gameplay-specific components when practical
- preserve prefab references and working game behavior during visual asset replacement
- prefer simple or compound colliders before expensive mesh collision for moving gameplay objects
- keep animation import decisions separate from Animator Controller gameplay logic
- verify reimport and prefab-instance behavior after import-affecting changes
- run Edit Mode or Play Mode checks appropriate to the changed behavior
- report remaining editor-only visual checks

## Dialect terms

### `model_source`

The upstream model file Unity imports.

Typical sources include exported files from Blender or other DCC tools. Keep source geometry corrections upstream when that produces a cleaner and more reproducible pipeline.

### `model_import_profile`

The Unity Model Import Settings required to reproduce the intended imported asset.

May include decisions from the Model, Rig, Animation, and Materials tabs, such as:

```text
scale
mesh_options
normals_tangents
rig_type
avatar_setup
animation_clips
material_handling
```

### `model_prefab`

The imported model hierarchy Unity creates from the model asset.

Treat it primarily as the imported representation of upstream content, not as the ideal place for project-specific gameplay behavior.

### `gameplay_prefab`

A project-owned prefab that wraps or references the imported model and owns Unity-specific behavior.

Typical responsibilities:

```text
scripts
colliders
rigidbody
animator_controller
vfx
sfx
interaction_points
gameplay_state
```

### `prefab_contract`

The hierarchy, component, and reference expectations that must survive visual or imported-asset replacement.

Examples:

```text
required_child_names
required_components
serialized_references
attachment_points
animation_parameters
```

### `reimport_safe`

A state where reimporting the upstream model does not silently destroy required prefab behavior, references, materials, rig assumptions, or gameplay configuration.

### `collider_profile`

The intended physics representation for the asset.

Prefer the cheapest collider shape that satisfies gameplay.

Common values:

```text
primitive
compound
convex_mesh
static_mesh
custom_proxy
```

Unity 6 documentation describes primitive colliders as the most computationally efficient. Convex Mesh Colliders are more expensive and are suitable when primitives or compound colliders cannot approximate the required shape. Non-convex Mesh Colliders are the most expensive option and are intended for static geometry that needs precise collision surfaces.

### `collision_proxy`

A deliberately simplified collision representation that is independent from render topology.

Use when gameplay and performance benefit from a simpler collision shape than the visible model.

### `rig_profile`

The intended Rig import configuration.

Examples:

```text
none
generic
humanoid
```

Keep rig import configuration distinct from runtime animation-state logic.

### `animation_import_profile`

The set of animation clips and import decisions Unity should extract from the model source.

May include:

```text
clip_ranges
loop_flags
root_motion_expectation
avatar_expectation
```

### `animator_contract`

The runtime Animator Controller interface that gameplay expects.

Examples:

```text
controller_asset
parameters
states
transitions
layers
```

Unity's Animator component references an Animator Controller, which manages animation clips and transitions. Keep this gameplay-facing contract stable when swapping or reimporting model assets.

### `material_strategy`

The rule for how imported materials become project materials.

Examples:

```text
use_imported
remap_existing
extract_and_edit
project_owned_materials
```

Choose one strategy explicitly instead of allowing each asset to drift into a different manual workflow.

### `import_automation`

Scripted importer configuration used when many assets must follow the same import rules.

Unity supports managing importers with scripts. Use automation when repeated manual importer edits are becoming a source of inconsistency.

### `playmode_baseline`

The smallest runnable state that proves the asset is correctly integrated into gameplay.

Examples:

```text
scene_loads
prefab_spawns
controls_work
collisions_work
animation_plays
materials_render
```

## Boundary rule

Default ownership:

```text
Blender          owns authored geometry and DCC cleanup
Model Importer   owns Unity import interpretation
model_prefab     owns imported model representation
gameplay_prefab  owns Unity-specific reusable behavior
scene            owns placement and scene composition
```

Do not duplicate the same responsibility across all layers unless there is a clear reason.

## Standard handoff: Blender → Unity

```text
ROLE: Unity integration agent
MODE: unity_3d_safe
TASK: integrate_engine_ready_asset
GOAL: reimport_safe_gameplay_prefab
INPUT:
- blender_engine_ready_export
TARGET:
- Assets/<asset_path>
- model_import_profile
- gameplay_prefab
DO:
- import model_source
- configure model_import_profile
- inspect model_prefab hierarchy
- create_or_update gameplay_prefab
- configure collider_profile
- configure material_strategy
- configure rig_profile_if_needed
- configure animation_import_profile_if_needed
- run playmode_baseline
KEEP:
- prefab_contract
- working_gameplay
NO:
- scene_only_fix_for_source_geometry
- duplicate_manual_import_settings_without_reason
CHECK:
- scale
- orientation
- hierarchy
- materials
- colliders
- rig
- animation
- prefab_references
- reimport_safe
- playmode_baseline
RISK:
- broken_serialized_reference
- wrong_scale
- expensive_mesh_collider
- material_remap_drift
- rig_mismatch
OUT:
- gameplay_prefab
- importer_notes
- remaining_manual_checks
NEXT: test_in_target_scene
```

## Standard handoff: imported model → gameplay prefab

```text
ROLE: Unity integration agent
MODE: unity_3d_safe
TASK: wrap_imported_model
STATE: model_prefab_imported
GOAL: reusable_gameplay_prefab
DO:
- preserve imported hierarchy where required
- add project_owned wrapper
- add scripts
- add colliders
- add rigidbody_if_needed
- add interaction_points
- connect animator_contract_if_needed
KEEP:
- model_prefab_as_imported_representation
- prefab_contract
NO:
- bury_source_repairs_in_scene_instances
CHECK:
- prefab_instance_updates
- serialized_references
- spawn
- interaction
- collision
OUT: gameplay_prefab
```

## Standard handoff: animated character

```text
ROLE: Unity animation integration agent
MODE: unity_3d_safe
TASK: integrate_animated_character
INPUT:
- model_source
- animation_clips
GOAL: stable_animator_contract
DO:
- configure rig_profile
- configure animation_import_profile
- verify avatar_if_humanoid
- connect animator_controller
- map required parameters
- test state transitions
KEEP:
- gameplay_parameter_names
- required_states
CHECK:
- rig_import
- avatar
- clip_ranges
- loop_behavior
- root_motion_expectation
- animator_parameters
- transitions
- playmode_animation
RISK:
- rig_mismatch
- changed_bone_hierarchy
- missing_clip
- parameter_drift
OUT:
- animated_gameplay_prefab
- animator_check_result
```

## Reimport regression check

```text
ROLE: Unity verification agent
MODE: unity_3d_safe
TASK: verify_asset_reimport
STATE: upstream_model_changed
DO:
- reimport model_source
- inspect importer_result
- inspect gameplay_prefab
- open target_test_scene
- enter Play Mode
CHECK:
- no_missing_references
- scale_unchanged_or_intended
- materials_resolve
- colliders_match_contract
- rig_valid
- animation_valid
- prefab_instances_valid
- gameplay_loop_still_works
OUT:
- reimport_result
- regressions
- manual_visual_checks
```

## Collider decision rule

Use the simplest physics shape that preserves intended gameplay.

```text
IF simple_shape
  → primitive
ELSE IF dynamic_complex_shape
  → compound OR convex_mesh
ELSE IF static_precise_environment
  → static_mesh
ELSE
  → custom_proxy
```

Do not use render-mesh fidelity as the default collider requirement.

## Acceptance checks

Pick only what applies.

```text
CHECK:
- imports_without_error
- scale
- orientation
- hierarchy
- model_import_profile
- materials
- prefab_contract
- serialized_references
- collider_profile
- rigidbody_behavior
- rig_profile
- animation_import_profile
- animator_contract
- prefab_instance_updates
- reimport_safe
- edit_mode_tests
- play_mode_tests
- target_fps
- visible_errors
```

## Source boundary

Unity-specific behavior summarized here is based on current Unity 6.0 documentation. AI Creole terms such as `model_import_profile`, `gameplay_prefab`, `prefab_contract`, `collider_profile`, `animator_contract`, `reimport_safe`, and `playmode_baseline` are madowaku-derived workflow vocabulary, not Unity product terminology unless explicitly noted.

Primary references:

- https://docs.unity3d.com/ja/current/Manual/models-importing.html
- https://docs.unity3d.com/ja/current/Manual/class-FBXImporter.html
- https://docs.unity3d.com/jp/current/Manual/prefabs-introduction.html
- https://docs.unity3d.com/ja/6000.0/Manual/primitive-colliders-introduction.html
- https://docs.unity3d.com/kr/6000.0/Manual/physics-optimization-cpu-collider-types.html
- https://docs.unity3d.com/ja/current/Manual/class-Animator.html
- https://docs.unity3d.com/ja/current/Manual/class-AnimatorController.html
- https://docs.unity3d.com/ja/current/Manual/com.unity.test-framework.html
- https://docs.unity3d.com/ja/current/Manual/import-assets.html
