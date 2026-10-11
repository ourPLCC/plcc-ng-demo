# Contributing to plcc-ng-demo

This builds on the [ourPLCC developer guide](https://github.com/ourPLCC/dev/blob/main/CONTRIBUTING.md); read that first. Below is what's specific to this repo, and where it differs.

For the demo's design, see [dev-docs/specs/2026-07-21-plcc-ng-demo-design.md](dev-docs/specs/2026-07-21-plcc-ng-demo-design.md).

## Where this repo differs from the org guide

Nowhere at present. One leftover from before the org guide is history, not a
deviation: [dev-docs/plans/](dev-docs/plans/) holds a kept plan. Merged plans
are frozen, so it stays; add no new ones.

## Development environment

Open the repo in its devcontainer. Before the container is created, it clones
the [ourPLCC/dev](https://github.com/ourPLCC/dev) repo beside this one, as
`../dev`, unless that folder already exists. The container mounts it at
`/workspaces/dev`, points the `backlog` CLI at it through `BACKLOG_CWD`, and
installs the pinned `backlog` version. In the tracker, this repo's `project`
value is `plcc-ng-demo`. Old plcc-ng-demo issue numbers still resolve at
+800: `#3` is `CR-803`.

[AGENTS.md](AGENTS.md) imports files from `../dev`. Claude Code loads imports
from outside the repo only once they are approved, and until then skips them
without an error, so agents never load the org guide. It does not reliably ask
for that approval, and the approval lives in `~/.claude.json`, which a rebuild
discards. So the container approves them when it is created, by running
[bin/install/claude-imports.bash](bin/install/claude-imports.bash). To check,
run `/memory` in a session and confirm it lists `ORG-AGENTS.md` and both
`CONTRIBUTING.md` files. If they are missing, quit every Claude Code session,
since a running one can overwrite `~/.claude.json`, then run the script and
start a new session.

Keep each import in `AGENTS.md` on its own line with nothing after the path:
Claude Code reads an import up to the next whitespace, so `@file.md,` looks
for a file named `file.md,` and silently skips it.

## Common commands

| Command | What it does |
|---|---|
| [bin/install/claude-imports.bash](bin/install/claude-imports.bash) | Approve [AGENTS.md](AGENTS.md)'s imports from `../dev` in `~/.claude.json`. Run by the devcontainer on creation. Idempotent. |
