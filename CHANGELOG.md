# Changelog

## 0.2.0 — 2026-06-01

- **Renamed** the plugin `srix-scaffold` → `specialist-agents` — the plugin id, the
  `/specialist-agents` command, and the marketplace name. (Breaking for anyone who
  installed `0.1.0` under the old name; reinstall under the new name.)
- **New agent: `designer`** — UX/UI thinking for user-facing specs. Sits between
  `spec-creator` and `plan-creator`, runs only on UI/UX tasks, and produces
  `specs/<task>-design.md` plus self-contained HTML mockups under `design/mockups/`
  (tool-independent — plain HTML + inline CSS, no external design tool required). Also
  maintains the `design-system.md` decision log.
- New scaffolded dir: `design/mockups/`.
- **Agents now compose with community skills.** Each agent has a *Leverage available
  skills* section nudging it toward relevant packs when installed (e.g. `superpowers`,
  `frontend-design`, and code-craft packs like `impeccable` / `mattpocock`), while
  staying functional without them. CLAUDE.md and README document the pairing.
- Sharpened positioning: a one-shot roster of specialist agents you field per project,
  not an always-on skill layer.

## 0.1.0 — 2026-05-13

Initial extraction from `lived-app`.

- 5 agents: spec-creator, plan-creator, architect, developer, tester.
- Templates for `CLAUDE.md`, `architecture/{architecture,local-development}.md`, `design/{README,design-system}.md`, `specs/{REQUIREMENTS,ROADMAP,STATUS,ui-design-brief}.md`.
- `/srix-scaffold` skill (skill name matches plugin name, so it's invoked as `/srix-scaffold` with no colon) — interactive prompts for project name, stack, build commands; substitutes `{{PROJECT_NAME}}` / `{{VERIFY_COMMAND}}` / `{{TODAY}}` placeholders.
- Genericized from Android/Compose/Hilt specifics — stack-neutral.
- No `.reviewer/` config included (kept as personal config in `~/.claude/`).
