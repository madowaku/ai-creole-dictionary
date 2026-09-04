# madowaku Standard AGENTS.md v1

Use this file as a lightweight execution adapter. Keep project knowledge and project-specific vocabulary in `AI_CREOLE.md` instead of duplicating it here.

## Instruction Priority

Follow, in order:

1. explicit user or task instructions
2. repository-local `AGENTS.md`
3. repository-local `AI_CREOLE.md`
4. existing project conventions, tests, and documented workflows
5. the defaults below

When instructions conflict, follow the higher-priority instruction and preserve the lower-priority intent where possible.

## AI Creole

- read repository-local `AI_CREOLE.md` when present
- for implementation work, default to `MODE: codex_patch` unless the task specifies another mode
- use AI Creole tags for handoff when they reduce ambiguity
- do not copy the full AI Creole dictionary into this file

## Default Execution

- inspect existing files before editing
- infer routine intent from task and context, then carry safe work to completion
- ask only when missing input could materially change the result or when an action is destructive, irreversible, or externally consequential
- prefer reuse and minimal diffs over broad rewrites
- avoid unrelated refactors and unrelated file changes
- preserve existing architecture, naming, data flow, and user-visible behavior unless the task requires changing them

## Parallel Work

- parallelize only independent workstreams that clearly save time or improve quality
- avoid subagent or coordination overhead for tiny patches
- reconcile parallel findings before editing overlapping areas

## Verification

- calibrate checks and tests to the size and risk of the change
- run the smallest sufficient checks first
- do not broaden or repeat passed checks without a new failure, code change, or unresolved concern
- never claim a test, build, command, or manual check passed unless it actually ran successfully
- if a check cannot run, report the reason and the next useful check

## Boundaries

- do not expose secrets or credentials
- preserve auth, billing, persistence, deployment, and external integration behavior unless explicitly targeted
- do not delete data, rewrite history, publish, deploy, or perform other destructive or irreversible actions without explicit authorization

## Completion

A task is complete when the requested change is implemented, proportionate checks have passed or been attempted, and any remaining manual check or blocker is clearly reported.

Report:

1. changed files
2. checks performed
3. remaining manual check or blocker, if any
