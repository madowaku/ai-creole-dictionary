# AI Creole Modes

Modes are reusable aliases for work style. Let modes evolve when repeated patterns appear.

## MODE: codex_patch

- inspect existing files first
- make minimal diff
- avoid broad refactor
- infer routine intent from task and context, then continue to completion when safe
- ask only when missing input could materially change the result or the action is destructive or irreversible
- calibrate checks and tests to change size and risk
- do not broaden or repeat passed checks without a new failure, change, or unresolved concern
- parallelize independent work when it clearly saves time or improves quality; avoid coordination overhead for tiny patches
- explicit user or task instructions override mode defaults
- report changed files
- include checks or test commands when possible
- use when: Codex should edit files or prepare a small repo patch
- do not use when: the user only wants brainstorming or high-level explanation

## MODE: local_review

- review only
- short output
- OK/FIX format
- max 5 bullets unless asked
- flag risks clearly
- no large rewrites
- use when: another model should check a patch, note, or handoff
- do not use when: implementation or large rewriting is needed

## MODE: unity_safe

- beginner friendly
- exact file paths
- hierarchy/object names
- Inspector steps
- compile-risk notes
- MVP first
- no large refactor
- use when: Unity scenes, prefabs, scripts, or Inspector steps are involved
- do not use when: the task is not Unity-specific

## MODE: web_safe

- no secret exposure
- minimal diff
- preserve existing auth/data flow
- report changed files and checks
- use when: web app code touches auth, data, APIs, billing, or deployment
- do not use when: pure content or ideation work has no app risk

## MODE: game_idea

- extract playable core
- identify MVP
- avoid scope creep
- keep weird worldbuilding if useful
- output next concrete steps
- use when: turning creative fragments into game concepts or prototype steps
- do not use when: the task is already a concrete implementation patch

## MODE: terse_handoff

- keep the AI Creole structure
- compress wording hard
- prefer fragments over filler
- preserve constraints and risk notes
- use when short handoff matters more than prose
- use when: another AI already has enough context
- do not use when: teaching, safety reasoning, or high-uncertainty decisions need detail
