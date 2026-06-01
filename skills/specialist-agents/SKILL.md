---
name: specialist-agents
description: Inflate the specialist-agents spec-driven workflow (agents, architecture/, design/, specs/, CLAUDE.md) into the current working directory. Use when the user wants to bootstrap a new project — or add the scaffold to an existing one — with the spec → designer (UI/UX) → plan → architect → developer → tester loop.
---

# specialist-agents

You are inflating the specialist-agents scaffold into the user's current working directory. It drops a roster of sharp, single-responsibility specialist agents into one project: agents under `agents/`, a living technical doc under `architecture/`, a design system under `design/`, specs and roadmap under `specs/`, and a `CLAUDE.md` tying them together. It is a one-shot inflation, not an always-on skill layer — the agents live in the project and you invoke a specialist when a task calls for it.

## Plugin layout (where you read from)

The plugin's files are at `${CLAUDE_PLUGIN_ROOT}` — typically `~/.claude/plugins/<install-id>/specialist-agents/`. The layout you care about:

```
${CLAUDE_PLUGIN_ROOT}/
├── agents/         ← copied verbatim into <project>/agents/
└── templates/      ← inflated with placeholder substitution
    ├── CLAUDE.md.tmpl
    ├── architecture/
    │   ├── architecture.md.tmpl
    │   └── local-development.md.tmpl
    ├── design/
    │   ├── README.md.tmpl
    │   └── design-system.md.tmpl
    ├── sdlc/
    │   └── flow.md.tmpl
    └── specs/
        ├── REQUIREMENTS.md.tmpl
        ├── ROADMAP.md.tmpl
        ├── STATUS.md.tmpl
        └── ui-design-brief.md.tmpl
```

If `${CLAUDE_PLUGIN_ROOT}` is not set, ask the user for the plugin's install path (or fall back to the repo path they cloned it from).

## Process

### Step 1 — Sanity-check the cwd

Before asking anything else:

1. Confirm `pwd` with the user. The scaffold installs into the current directory.
2. Check for collisions: `ls agents/ architecture/ design/ specs/ CLAUDE.md 2>/dev/null`. If anything already exists, **stop and ask the user** how to proceed (overwrite, skip-existing, abort). Default to skip-existing — never silently overwrite.

### Step 2 — Gather project info (interactive)

Ask one focused question at a time using `AskUserQuestion`. Don't batch into a giant form. Capture:

1. **Project name** — display name used in headers (e.g. "Lived"). Free text.
2. **One-liner** — single sentence describing what the project does. Used in `CLAUDE.md` and `architecture.md` and `REQUIREMENTS.md`.
3. **Stack** — pick one or describe:
   - Android (Kotlin + Compose + Gradle)
   - iOS (Swift + SwiftUI + Xcode)
   - Web frontend (TypeScript + framework)
   - Backend service (language + framework)
   - CLI tool (language)
   - Other (free text)
4. **Build / test / lint commands** — three short shell commands. If the project doesn't have them yet, accept the user's best guess and leave a TODO.
5. **Single verification command** — the chained pre-merge command (e.g. `./gradlew verifyAll`, `npm run verify`). If none, suggest creating one and leave it blank for now.

Defer these to a follow-up — don't block the inflation on them:
- Vision paragraph (longer than the one-liner; goes into `REQUIREMENTS.md` and `architecture.md`).
- Layer/module structure.
- Tech-stack version numbers.
- Project-structure notes for `CLAUDE.md`.

After inflation, tell the user which sections still have placeholders and which ones to fill in next.

### Step 3 — Inflate

1. Compute `${TODAY}` as `YYYY-MM-DD` from the system date.
2. Build a substitution map:
   - `{{PROJECT_NAME}}` → step 2.1
   - `{{PROJECT_ONE_LINER}}` → step 2.2
   - `{{PROJECT_VISION}}` → leave as placeholder with a `<!-- TODO -->` marker, OR ask step 2 follow-up
   - `{{BUILD_COMMAND}}`, `{{TEST_COMMAND}}`, `{{INTEGRATION_TEST_COMMAND}}`, `{{LINT_COMMAND}}`, `{{VERIFY_COMMAND}}` → step 2.4 / 2.5
   - `{{PROJECT_STRUCTURE_NOTES}}`, `{{BUILD_CONFIG_NOTES}}`, `{{KEY_DEPENDENCIES}}` → leave as placeholders for the user to fill in later
   - `{{TODAY}}` → today's date
3. For each `.tmpl` file under `${CLAUDE_PLUGIN_ROOT}/templates/`:
   - Strip the `.tmpl` suffix.
   - Apply substitutions.
   - Write to the corresponding path in cwd, preserving subdirectory layout.
   - Honor the collision policy from step 1.
4. Copy `${CLAUDE_PLUGIN_ROOT}/agents/*.md` verbatim into `<cwd>/agents/`, applying `{{PROJECT_NAME}}` and `{{VERIFY_COMMAND}}` substitutions but otherwise unchanged. (All agent files are copied, so the count grows automatically as agents are added.)
5. Create empty placeholder dirs: `design/exports/` and `design/mockups/` (the designer agent writes self-contained HTML mockups under `design/mockups/`).

### Step 4 — Report

Output a short summary:

```
Scaffold initialized.

Created:
- CLAUDE.md
- agents/ (6 files)
- architecture/architecture.md
- architecture/local-development.md
- design/README.md
- design/design-system.md
- design/exports/  (empty)
- design/mockups/  (empty)
- specs/REQUIREMENTS.md
- specs/ROADMAP.md
- specs/STATUS.md
- specs/ui-design-brief.md
- sdlc/flow.md

Next steps:
1. Edit `architecture/architecture.md` — fill in the Vision, Layers, Tech stack, and Data flow sections.
2. Edit `specs/REQUIREMENTS.md` — replace the FR1/FR2 placeholders with your real functional requirements.
3. Edit `CLAUDE.md` — fill the {{PROJECT_STRUCTURE_NOTES}}, {{BUILD_CONFIG_NOTES}}, {{KEY_DEPENDENCIES}} placeholders.
4. Write your first task spec via the spec-creator agent.

Open `agents/` for role definitions; the normal flow for a new task is
spec-creator → designer (UI/UX tasks only) → plan-creator → architect (review) → developer → tester → architect (update).
```

## Stop conditions

- The user wants the scaffold somewhere other than the current cwd → confirm the target path before doing anything.
- A file collision the user hasn't decided on → never silently overwrite.
- `${CLAUDE_PLUGIN_ROOT}` (or the template source) is missing → ask the user where the plugin is installed.

## Style

- Don't try to write a "complete" architecture.md or REQUIREMENTS.md during init. The scaffold gives shape; content lands as the project develops.
- Leave `<!-- TODO -->` markers where the user must fill in domain-specific content. Better an obvious TODO than a fabricated paragraph.
- Don't run any build commands — the project may not be set up yet. Just write files.
