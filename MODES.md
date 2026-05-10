# AI Creole Modes

Modes are reusable aliases for work style. Let modes evolve when repeated patterns appear.

## MODE: codex_patch

- inspect existing files first
- make minimal diff
- avoid broad refactor
- report changed files
- include checks or test commands when possible

## MODE: local_review

- review only
- short output
- OK/FIX format
- max 5 bullets unless asked
- flag risks clearly
- no large rewrites

## MODE: unity_safe

- beginner friendly
- exact file paths
- hierarchy/object names
- Inspector steps
- compile-risk notes
- MVP first
- no large refactor

## MODE: web_safe

- no secret exposure
- minimal diff
- preserve existing auth/data flow
- report changed files and checks

## MODE: game_idea

- extract playable core
- identify MVP
- avoid scope creep
- keep weird worldbuilding if useful
- output next concrete steps
