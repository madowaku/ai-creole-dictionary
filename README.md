# AI Creole Dictionary

Canonical v0.1 dictionary for AI Creole.

AI Creole is a compact shared work language for human-to-AI and AI-to-AI handoff. It is not a universal rigid language. It is a small, structured, low-ambiguity work protocol with a stable core and project-local dialects.

## Structure

- `AI_CREOLE_CORE.md`: stable core tags
- `MODES.md`: reusable work-style aliases
- `AGENT_ROLES.md`: common target agent roles
- `TEMPLATES/`: prompt templates
- `PROJECTS/`: project-local dialect notes
- `GOOGLE_DRIVE_SUMMARY_DRAFT.md`: ChatGPT-readable entrance draft

## Operating Model

- GitHub is the canonical source.
- Each project may keep its own `AI_CREOLE.md`.
- Google Drive may summarize this repo for ChatGPT and humans.
- Codex updates this repo and project-local markdown.
- Local LLMs read project-local markdown for review and handoff.

Keep v0.1 small. Add terms only when they reduce real handoff ambiguity.
