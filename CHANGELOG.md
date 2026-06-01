# Changelog

## 0.4.0 — 2026-06-01

- **Renamed the inflate skill `specialist-agents` → `setup`.** It is now invoked as
  **`/specialist-agents:setup`**. Claude Code always namespaces a plugin's skills as
  `/<plugin>:<skill>` by the skill's *directory* name (no collapse when names match), so
  the old layout produced the redundant `/specialist-agents:specialist-agents`; `setup`
  fixes that and reads as the action it performs.
- Made the setup skill **user-invoke-only** (`disable-model-invocation: true`) — a
  file-writing scaffold should be run deliberately, not auto-triggered.
- README: added a **"Why a plugin (not just a skill)?"** section (plugin-vs-skill
  rationale).
- Plugin name and install command (`specialist-agents@specialist-agents`) unchanged.

## 0.3.0 — 2026-06-01

- **New doc: `sdlc/flow.md`** (template) — a dedicated, visual SDLC document inflated into
  every scaffolded project. Left-to-right Mermaid diagram of the lifecycle: the spec is
  authored in `main`, the rest of the pipeline (designer → plan → architect review →
  developer → tester → architect doc-update → final commit) runs **autonomously inside a
  sandbox**, with a single human review at merge. Documents the `main`↔sandbox boundary and
  the sandbox lifecycle (recommends `agent-sandbox`, but the sandbox is not required). New
  top-level `sdlc/` folder, room for future flow variants.
- **Developer drops git-worktree gymnastics.** It now works on the current branch (the
  sandbox branch already exists); as a fallback, if it runs on `main` it creates a
  `task/<slug>` branch first. README/CLAUDE.md updated; the README workflow is now the same
  Mermaid diagram.

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
