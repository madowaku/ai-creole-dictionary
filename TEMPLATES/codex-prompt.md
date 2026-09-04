# Codex Prompt Template

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

Use exact file paths when known. Prefer small diffs and clear verification.
Carry routine work to completion when intent is clear. Ask only when missing input could materially change the result or the action is destructive or irreversible. Calibrate checks to change size and risk, and do not repeat passed checks without a new reason. Parallelize independent work only when it clearly saves time or improves quality.
