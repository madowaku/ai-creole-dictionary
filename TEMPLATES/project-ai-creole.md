# AI_CREOLE.md

## Purpose

Compact shared work language for this project.
Used for human-to-AI and AI-to-AI handoff.

## Core Tags

ROLE = target agent or responsibility
MODE = reusable working style
TASK = work to perform
GOAL = desired end state
STATE = current known state
CONTEXT = background
INPUT = supplied material
TARGET = files, scenes, objects, docs, or areas to touch
DO = required actions
KEEP = things to preserve
NO = forbidden actions
OUT = expected output format
CHECK = verification items
RISK = likely failure points
NEXT = next action

## Modes

### MODE: codex_patch

- inspect existing files first
- make minimal diff
- avoid broad refactor
- report changed files
- include checks

### MODE: local_review

- review only
- short output
- OK/FIX format
- max 5 bullets unless asked

## Terms

Add project-specific terms here.

## Prompt Examples

```text
ROLE: Codex
MODE: codex_patch

TASK:
GOAL:
STATE:
TARGET:
DO:
KEEP:
NO:
OUT:
CHECK:
RISK:
NEXT:
```
