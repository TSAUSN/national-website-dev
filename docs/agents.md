# Agent Roster & Workflow

Status: Draft — pending review
Last generated: 2026-10-14 by Claude (zesty-ai:project-discovery, automated codebase scan)

How AI agents work on this project: which plugin agents are used, where the human gates are, and what this project changes. Shared rules live in the zesty-ai plugin (`standards/`, `workflows/qa-to-release.md`); this doc records only what is specific to this project.

## Quick reference

| Step | Agent | Does | Never does | Learnings file |
| --- | --- | --- | --- | --- |
| Plan | `zesty-ai:task-manager` | Breaks the Coda ticket down, sequences work, tracks status | Writes code; assumes merges | `.claude/learnings/task-manager.md` |
| Build | `zesty-ai:developer` | Implements on the `coda-####` branch, lists QA targets | Pushes `stage`/`production`/`main`; opens/merges PRs | `.claude/learnings/developer.md` |
| Spec audit | `zesty-ai:spec-auditor` | Checks the real diff against the ticket and `CLAUDE.md` | Edits files | `.claude/learnings/spec-auditor.md` |
| Code review | `zesty-ai:sanity-check` | Reviews the diff for correctness and security | Edits files | `.claude/learnings/sanity-check.md` |
| 🔒 Human review | Lead and programmer | Review the PR and its sync-preview comment (required gate) before merge | | |
| QA | `zesty-ai:qa` | Tests the branch preview / stage in a real browser | Tests production | `.claude/learnings/qa.md` |
| Reality check | `zesty-ai:reality-check` | Confirms "QA passed" is really true on disk and live | Runs scripts | `.claude/learnings/reality-check.md` |
| Docs & release | `zesty-ai:documentation` | Updates `docs/`, changelog, release notes | Publishes externally | `.claude/learnings/documentation.md` |
| Platform questions | `zesty-ai:zesty-expert` | Answers `PLATFORM QUESTION`s from docs and read-only lookups | Implements or deploys | `.claude/learnings/zesty-expert.md` |

## The main chain

1. Coda ticket `CODA-####` → task-manager plan
2. developer builds on branch `coda-####`, cut from `origin/stage` with `--no-track`
3. spec-auditor → sanity-check (Critical/High findings loop back to the developer; max 3 rounds)
4. 🔒 Human code review (lead and programmer) of the PR into `stage`, including the "Stage sync preview" comment posted by `zesty-pr-preview.yml`
5. 🔒 Preview: the branch preview renders the branch from GitHub (no deploy step). Stage deploy happens when a human merges into `stage` (`zesty-deploy.yml` saves to the instance's dev version)
6. qa on the branch preview (`https://hr1cm7dt-coda-####.webengine.zesty.io/`), then on stage (https://hr1cm7dt-dev.webengine.zesty.io/) after merge; then reality-check
7. 🔒 Merges: `coda-####` → `stage` → `production`, approved by the lead and the programmer. The production PR's comment lists what will be published dev → live. `main` is retired
8. documentation updates `docs/` and `docs/changelog.md`

## Project-specific agents

None. The repository has no `.claude/agents/`.

## Project overrides

- **Never touch `webengine/views/z/`** (Zesty Layout-editor output, platform-owned). See `CLAUDE.md`.
- **Never hand-edit `zesty.config.json`.** The sync script and the CI bot write it.
- **Human-only:** `.github/workflows/`, `scripts/sync-to-zesty.js`, the `workflow_dispatch` inputs `publish` and `full_sync`.
- **New extension-less views** need a human to create them in the Zesty admin first (the sync script skips them). The developer must flag this in the hand-off instead of expecting CI to create them.
- **The only local "check" is the read-only sync report** (`commands.sync_report`). There are no lint, test or build gates.
- QA never runs on production. The §7 smoke subset in `docs/qa-test-plan.md` is human-initiated.

## Open Questions / Flags

- Does the branch preview need access credentials?

## Review status

Not yet reviewed.
