# Installing the srix-scaffold workflow

## 1. Make sure you have access

This is a **private** repo. Ask Srix to add you as a collaborator on `srix/srix-scaffold`,
and confirm you can reach GitHub (`gh auth status` or a test `git clone` works).

## 2. Register the marketplace and install the plugin

Run these two slash commands inside Claude Code:

```
/plugin marketplace add srix/srix-scaffold
/plugin install srix-scaffold@srix-scaffold
```

The first registers the repo as a plugin marketplace; the second installs the
`srix-scaffold` plugin *from* that marketplace (the `plugin@marketplace` syntax).
Restart Claude Code if prompted.

## 3. Create the scaffold in a project

`cd` into the project you want to scaffold (new or existing), open Claude Code, and run:

```
/srix-scaffold
```

It inflates into the **current directory**, so make sure you're in the right folder. It will:

- confirm your `pwd` and check for file collisions (it won't silently overwrite),
- ask a few questions (project name, one-liner, stack, build/test/lint commands, verify command),
- then write `CLAUDE.md`, `agents/`, `architecture/`, `design/`, and `specs/`.

## 4. After inflation

Fill in the `<!-- TODO -->` placeholders it leaves in `architecture/architecture.md`,
`specs/REQUIREMENTS.md`, and `CLAUDE.md`.

The normal per-task flow is then:

```
spec-creator -> plan-creator -> architect (review) -> developer -> tester -> architect (update)
```
