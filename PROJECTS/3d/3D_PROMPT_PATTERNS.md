# 3D Prompt Patterns

Reusable 3D production patterns for AI Creole.

This is not a collection of long “magic prompts.” It extracts durable instruction structures from public Tripo 3D prompt examples and maps them onto the existing AI Creole Core.

Canonical Core remains unchanged:

```text
ROLE:
MODE:
TASK:
GOAL:
STATE:
CONTEXT:
INPUT:
TARGET:
DO:
KEEP:
NO:
OUT:
CHECK:
RISK:
NEXT:
```

3D-specific vocabulary should stay project-local unless repeated use proves it deserves promotion.

## P01 — Outcome first

Define what “done” means before describing decoration.

```text
TASK: browser_3d_game
GOAL: playable_now
TARGET: browser
DO:
- build core_mechanic
- build objective
- build controls
CHECK:
- launch
- play
- restart
OUT: playable_project
```

## P02 — Preserve the core, replace the shell

Keep the proven loop and swap theme, environment, feedback, art, or audio.

```text
TASK: themed_variant
TARGET: existing_greybox
KEEP:
- core_loop
- controls
- rules
- progression
DO:
- replace environment
- replace art
- replace feedback
- replace audio
CHECK:
- same_playability
- theme_readability
OUT: themed_build
```

## P03 — Reference → structure → playable

Use a reference image as input to reconstruct structure, then add interaction and a playable loop.

```text
TASK: reference_to_playable
INPUT: reference_image
KEEP:
- composition
- recognizable_shapes
- visual_premise
DO:
- reconstruct scene
- add interaction
- add animation
- add camera
- add objective
CHECK:
- reference_similarity
- playable
- readable
```

## P04 — Editable, named, separate

Prefer downstream editability over a monolithic “finished-looking” asset.

```text
TASK: editable_asset
TARGET: blender
DO:
- separate logical_parts
- name objects
- keep transforms editable
- keep materials editable
NO:
- merged_monolith
- unnamed_parts
CHECK:
- object_separation
- naming
- editability
```

## P05 — Control the detail budget

Specify where detail matters instead of asking for “high quality” everywhere.

```text
TASK: detailed_model
DO:
- prioritize silhouette
- prioritize focal_parts
- use repeatable_detail
- keep detail_budget controllable
RISK:
- excessive_geometry
- invisible_detail
CHECK:
- silhouette
- focal_detail
- runtime_cost
```

## P06 — Performance is an acceptance check

Treat runtime performance as a completion condition, not cleanup work.

```text
TASK: realtime_scene
GOAL: smooth_runtime
DO:
- use efficient_geometry
- reuse repeated_assets
- optimize expensive_effects
CHECK:
- target_fps
- interaction_latency
- loading_behavior
RISK:
- geometry_explosion
- shader_cost
- asset_overload
```

## P07 — Close the test loop

Make run → inspect → fix → rerun explicit.

```text
TASK: playable_build
DO:
- build
- run
- inspect visible_failures
- fix highest_impact_failures
- rerun
CHECK:
- launch
- controls
- collisions
- layout
- fps
NEXT: repeat_until_checks_pass
```

## P08 — Stage large worlds

Break large spaces into regions or milestones and expand only after the current unit passes checks.

```text
TASK: large_world
DO:
- split into_regions
- build first_region
- evaluate
- refine
- expand_after_pass
CHECK:
- scale
- continuity
- landmarks
- navigation
RISK:
- scope_explosion
- inconsistency
```

## P09 — Preserve scale cues

Use familiar environmental cues, camera tuning, and movement speed so scale is perceptible.

```text
TASK: scale_readable_scene
DO:
- add familiar_scale_references
- preserve proportions
- tune camera
- tune movement_speed
CHECK:
- scale_is_readable
- silhouette_is_clear
```

## P10 — Separate simulation state from spectacle

For living systems, require persistent state and autonomous behavior rather than scripted visual events only.

```text
TASK: living_simulation
DO:
- define agent_needs
- define shared_goal
- define interactions
- define persistent_state
CHECK:
- agents_act_without_player
- state_changes_over_time
- system_remains_coherent
RISK: scripted_only_behavior
```

## P11 — Procedural first for repeated worlds

Use rules, instancing, reusable geometry, and procedural materials for forests, cities, crowds, oceans, and other repeated environments.

```text
TASK: dense_environment
DO:
- define generation_rules
- instance repeated_geometry
- vary placement
- use procedural_materials
CHECK:
- density
- variation
- performance
NO: unique_mesh_for_every_instance
```

## P12 — Explode for inspectability

Complex assemblies should support component inspection and, when useful, exploded and reassembled states.

```text
TASK: inspectable_assembly
DO:
- separate components
- add explode_state
- allow inspect
- add reassemble_state
CHECK:
- component_identity
- navigation
- reassembly
```

## P13 — Explicit uncertainty

Do not silently invent missing geometry. Surface assumptions and refine them against available references.

```text
TASK: reconstruct_from_partial_reference
DO:
- infer missing_geometry
- mark assumptions
- build first_pass
- compare against references
- refine mismatches
RISK: uncertain_geometry
OUT:
- reconstruction
- assumption_list
```

## P14 — One source of dimensions

When 2D and 3D views describe the same space, use one shared dimension source.

```text
TASK: linked_2d_3d_space
KEEP: shared_dimensions
DO:
- build floor_plan
- build 3d_walkthrough
- sync current_position
- label assumptions
CHECK:
- dimension_consistency
- position_sync
```

## P15 — Cross-tool asset pipeline

Describe the whole handoff: generation → cleanup → export → engine import → playable assembly.

```text
TASK: game_asset_pipeline
DO:
- generate asset
- prepare in_blender
- export engine_ready
- import game_engine
- assemble playable_scene
CHECK:
- scale
- orientation
- materials
- animation
- runtime
OUT:
- engine_ready_asset
- playable_scene
```

## P16 — Interaction before decoration

Get input, response, and feedback working before visual polish.

```text
TASK: interactive_3d
DO:
- build interaction_first
- confirm feedback
- then refine visuals
CHECK:
- input
- response
- clarity
- polish
NO: decoration_before_working_interaction
```

## P17 — Design for the target surface

Specify device, aspect, input mode, and startup constraints.

```text
TASK: realtime_experience
TARGET: mobile_browser
DO:
- design for_touch
- design vertical_layout
- minimize startup_cost
CHECK:
- touch_controls
- loading
- readable_ui
- target_fps
```

## P18 — Short revision loop

Once a playable baseline exists, prefer small explicit changes that preserve working parts.

```text
TASK: iterative_3d_builder
STATE: playable_baseline_exists
DO:
- change one_clear_thing
- rerun
- inspect
- preserve working_parts
KEEP: playable_baseline
CHECK:
- requested_change
- regressions
NEXT: next_short_revision
```

# Standard templates

## Blender asset

```text
TASK: <asset>
GOAL: editable_engine_ready_asset
TARGET: blender
KEEP:
- reference_proportions
DO:
- build silhouette_first
- separate logical_parts
- name objects
- create materials
- set sensible_pivots
- prepare export
NO:
- merged_monolith
- hidden_required_parts
CHECK:
- silhouette
- scale
- object_names
- editability
- materials
- export
RISK:
- excessive_geometry
- uncertain_geometry
OUT:
- blender_scene
- engine_ready_export
NEXT: import_and_test
```

## Reference image → playable scene

```text
TASK: reference_to_playable
GOAL: recognizable_and_playable
TARGET: <engine>
KEEP:
- composition
- visual_premise
- important_landmarks
DO:
- reconstruct scene
- add collision
- add controls
- add camera
- add core_interaction
- add feedback
- add restart
CHECK:
- reference_similarity
- controls
- collisions
- objective
- fps
OUT: playable_build
```

## Greybox → themed game

```text
TASK: theme_existing_game
TARGET: existing_greybox
KEEP:
- core_loop
- controls
- rules
- progression
DO:
- replace environment
- replace character_assets
- replace vfx
- replace audio
- adjust ui
- play_test
- fix visible_bugs
CHECK:
- no_core_regression
- theme_readability
- fps
- complete_loop
OUT: themed_playable_build
```

## Large world

```text
TASK: build_large_world
GOAL: coherent_explorable_world
DO:
- define regions
- define shared_scale
- build one_region
- evaluate
- refine
- expand
KEEP:
- scale_rules
- navigation_rules
- art_rules
CHECK:
- landmarks
- continuity
- navigation
- performance
RISK:
- scope_explosion
- inconsistent_regions
NEXT: next_region_after_pass
```

# Suggested 3D dialect terms

Keep these local until repeated use proves they are broadly useful:

```text
greybox
playable_baseline
engine_ready
editable_scene
logical_parts
detail_budget
target_fps
reference_similarity
uncertain_geometry
visible_failures
```

# Composition rule

Do not copy every pattern into every prompt. Compose only what the task needs.

Example: “turn a reference image into a playable Godot mini-game” might combine P03, P04, P06, P07, and P15.

# Production line

```text
reference_or_idea
  ↓
GOAL + TARGET + KEEP
  ↓
prototype / greybox
  ↓
3d_asset_generation
  ↓
Blender cleanup
  ↓
engine import
  ↓
playable baseline
  ↓
CHECK
  ↓
visible failure fix
  ↓
short revision loop
```

The operating idea is simple: do not merely ask an AI to “make it.” Give it a production line that is easier to verify, repair, and hand off.

# Sources and boundary

Source-derived ideas were extracted and summarized from public Tripo prompt examples and related Tripo 3D prompt guidance. The pattern names, AI Creole mappings, templates, and production-line framing are madowaku-derived organization, not Tripo terminology.

Primary references:

- https://www.tripo3d.ai/3d-prompts
- https://www.tripo3d.ai/3d-prompts/models/gpt-6-astra
- https://www.tripo3d.ai/ja/3d-prompts/three-themed-kart-games-from-one-greybox-2095580402505400369

Future split candidates:

```text
PROJECTS/3d/
├─ 3D_PROMPT_PATTERNS.md
├─ blender.md
├─ unity.md
├─ godot.md
├─ threejs.md
└─ tripo.md
```
