# AI_CREOLE.md

## Purpose

Compact shared work language for App Gardenium.
Used for human-to-AI and AI-to-AI handoff.

Canonical source: https://github.com/madowaku/ai-creole-dictionary

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

### MODE: web_safe

- no secret exposure
- minimal diff
- preserve existing auth/data flow
- report changed files and checks

### MODE: codex_patch

- inspect existing files first
- make minimal diff
- avoid broad refactor
- infer routine intent and continue to completion when safe
- ask only when missing input materially changes the result or the action is destructive or irreversible
- calibrate checks to change size and risk
- do not repeat passed checks without a new reason
- parallelize independent work when it clearly saves time or improves quality
- explicit user or task instructions override mode defaults
- report changed files
- include checks

## Terms

Growth Agent = idea diagnosis, MVP planning, tester strategy, and suggestion workflow
idea = user-submitted app idea
salon = community discussion area
membership = billing and access feature area
store_review_readiness = lightweight pre-launch store submission risk check
security_readiness = lightweight pre-launch security risk check, not an audit
compact_receipt = final response format with only changed files, checks, and remaining manual check

## Output Conventions

### OUT: compact_receipt

- Return compact receipt
- Include only:
  1. changed files
  2. checks
  3. remaining manual check
- Max 1200 characters

## Prompt Examples

```text
ROLE: Codex
MODE: web_safe

TASK:
GOAL:
STATE:
TARGET:
DO:
KEEP:
NO:
OUT: compact_receipt
CHECK:
RISK:
NEXT:
```
