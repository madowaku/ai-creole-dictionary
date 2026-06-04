# ChatGPT To Codex

Use when turning a ChatGPT discussion into an actionable Codex prompt.

```text
ROLE: Codex
MODE: codex_patch

TASK:
Implement the concrete next step from this discussion.

GOAL:

STATE:

CONTEXT:

INPUT:

TARGET:

DO:
- inspect local files before editing
- keep the first patch narrow
- report changed files and checks

KEEP:

NO:
- do not invent missing product decisions
- do not rewrite unrelated code

OUT:
- summary
- changed files
- checks
- next action

CHECK:

RISK:

NEXT:
```
