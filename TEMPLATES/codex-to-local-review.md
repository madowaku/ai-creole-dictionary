# Codex To Local Review

Use when asking a local LLM to review a Codex patch or handoff note.

```text
ROLE: Local LLM
MODE: local_review

TASK:
Review this patch handoff.

GOAL:
Find likely bugs, missing checks, or unclear handoff points.

STATE:

INPUT:

DO:
- classify as OK or FIX
- flag only material risks
- keep output short

NO:
- no rewrite
- no broad style advice

OUT:
OK/FIX, max 5 bullets

CHECK:

RISK:

NEXT:
```
