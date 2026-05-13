# Changelog

## 0.1.0 — 2026-05-13

Initial extraction from `lived-app`.

- 5 agents: spec-creator, plan-creator, architect, developer, tester.
- Templates for `CLAUDE.md`, `architecture/{architecture,local-development}.md`, `design/{README,design-system}.md`, `specs/{REQUIREMENTS,ROADMAP,STATUS,ui-design-brief}.md`.
- `/scaffold-init` skill — interactive prompts for project name, stack, build commands; substitutes `{{PROJECT_NAME}}` / `{{VERIFY_COMMAND}}` / `{{TODAY}}` placeholders.
- Genericized from Android/Compose/Hilt specifics — stack-neutral.
- No `.reviewer/` config included (kept as personal config in `~/.claude/`).
