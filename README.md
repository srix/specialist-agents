# srix-scaffold

A Claude Code plugin that drops a spec-driven workflow into any new (or existing) project. Inflates five specialist agents, an architecture doc, a design folder, and a specs / roadmap / status set — all wired together by a generated `CLAUDE.md`.

Reference implementation: [`lived-app`](https://github.com/srix/lived-app) — the project this scaffold was extracted from.

## What you get

Run `/srix-scaffold` inside a project directory and the following lands in `cwd`:

```
.
├── CLAUDE.md                ← project guide for Claude Code, points at agents/ + architecture/
├── agents/
│   ├── spec-creator.md      ← writes specs/<task>-spec.md (the WHAT)
│   ├── plan-creator.md      ← writes specs/<task>-plan.md (the HOW)
│   ├── architect.md         ← owns architecture/architecture.md; reviews plans, updates post-merge
│   ├── developer.md         ← implements an approved plan in a git worktree
│   └── tester.md            ← authors / runs / audits tests 1:1 against acceptance criteria
├── architecture/
│   ├── architecture.md      ← living technical doc with ADR-style decision log
│   └── local-development.md ← setup notes, commands, gotchas
├── design/
│   ├── README.md            ← where things live; engineer-facing
│   ├── design-system.md     ← tokens, type, spacing, motion, components, decisions
│   └── exports/             ← dated snapshots from the design tool
└── specs/
    ├── REQUIREMENTS.md      ← product-level FRs / NFRs
    ├── ROADMAP.md           ← phased plan with entry/exit criteria
    ├── STATUS.md            ← live phase + task-spec inventory
    └── ui-design-brief.md   ← input to the design tool
```

## The workflow

For each task:

```
spec-creator   →   plan-creator   →   architect (review)
                                              ↓
                              developer (in worktree)
                                              ↓
                                          tester
                                              ↓
                                   architect (update doc)
```

Each agent has a sharp role:

- **spec-creator** writes the WHAT/WHY in user-observable terms; no implementation talk.
- **plan-creator** answers HOW: file-level changes, reusable code to leverage, verification steps.
- **architect** gates plans against `architecture.md` and updates the doc after merges.
- **developer** executes the plan in an isolated git worktree, reports back with a branch.
- **tester** maps every acceptance criterion to a test (or a manual-deferred procedure), and runs the full audit on demand.

## Install (local, for yourself)

While iterating on the scaffold:

```bash
# point Claude Code at the local plugin
/plugin install file:///media/workdir/workspace/srix-scaffold
```

Then in any project:

```bash
/srix-scaffold
```

The skill prompts for project name, stack, build commands, then inflates the templates into `cwd`. It refuses to overwrite existing files by default.

## Install (for colleagues, from GitHub)

Once published:

```bash
/plugin marketplace add github:srix/srix-scaffold
/plugin install srix-scaffold
```

Tag stable versions so colleagues can pin (`v0.1.0`, `v0.2.0`) while you keep iterating on `main`.

## Maintenance — the harvest loop

You'll notice improvements while *using* the agents in a real project — that's where friction surfaces. The discipline:

1. Fix it in the real project first.
2. Once the change has been used a few days, port the diff into this plugin repo.
3. Bump the version (`plugin.json`), write a one-line `CHANGELOG.md` entry.
4. Tag.

Don't edit the plugin from inside a real project's directory — it muddles the source of truth. Open `srix-scaffold/` in its own editor window for plugin work.

The opposite direction (plugin → real project) is harder: existing projects don't auto-upgrade. Treat the scaffold as a one-shot inflation, not a runtime dependency. When you want new patterns in an old project, manually port what you want from the plugin.

## Versioning

Semver, loosely:
- **Patch** — content tweaks inside an existing file (wording, examples).
- **Minor** — new agent, new template file, new skill, new placeholder.
- **Major** — folder shape change, breaking placeholder rename, removed agent.

Existing inflated projects only see plugin upgrades if the user re-runs `/srix-scaffold` — and even then only for new files (existing files are skipped by collision policy).

## Files

- `.claude-plugin/plugin.json` — plugin metadata.
- `skills/srix-scaffold/SKILL.md` — the `/srix-scaffold` skill.
- `agents/*.md` — copied verbatim into the user's project (with `{{PROJECT_NAME}}` / `{{VERIFY_COMMAND}}` substituted).
- `templates/**/*.tmpl` — inflated with placeholder substitution; `.tmpl` suffix stripped on copy.
- `CHANGELOG.md` — one line per release.
