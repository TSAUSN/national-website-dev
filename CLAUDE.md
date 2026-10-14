# The Salvation Army National Website

<!-- Status: Draft — pending review (generated 2026-10-14 by zesty-ai:project-discovery) -->

## About

Zesty.io WebEngine website for The Salvation Army USA national site: Parsley views, SCSS
and JavaScript stored in this repository and synced to Zesty instance `8-b28282a4fe-dszqwn`.
Project type: `website`.

Machine-readable configuration (commands, quality gates, environments, integrations):
`.claude/project.yaml`.

## Standards

This project follows the zesty-ai shared standards (load the
`zesty-ai:engineering-standards` skill). Project rules below add to them.

## Architecture

- `webengine/views/`: Parsley views (1,317 files). The file path is the Zesty view name.
  - Top level: page templates (`pageset` = one view per item of the model of the same name,
    for example `locations`, `services`, `stories`; `templateset` = parent URL view, for example
    `homepage`), `loader` (the global layout wrapping every page), `custom_head`, and JSON endpoints (`*.json`).
  - `modules/`: page sections pulled in with `{{ include modules/<name> }}`.
  - `components/`: smaller shared pieces (header, footer, cards, modals).
  - `-/block/`: block library (`*.html`) rendered by Zesty layouts via `block('/-/block/<name>.html')`.
  - `datasets/mobile_editor/`, `api/`: JSON endpoints consumed by other apps.
  - `z/`: **platform-owned** Zesty Layout-editor output. See "Project-specific restrictions".
- `webengine/styles/`: SCSS/CSS stylesheets (33). `webengine/scripts/`: JavaScript (23), including
  `data-layer/*-tracking.js` analytics.
- `zesty.config.json`: maps every file to its Zesty resource ZUID and type. The sync script
  and the CI bot write it. See `docs/custom-patterns.md`.
- `scripts/sync-to-zesty.js`: the deploy script (repo → Zesty instance).
- `.github/workflows/`: `zesty-deploy.yml` (deploy on push to `stage`/`production`) and
  `zesty-pr-preview.yml` (read-only sync preview comment on PRs into `stage`/`production`).
- External systems: Zesty.io instance (Instances API), Tealium (`utag.js`, set up in `loader`),
  Microsoft Clarity (dev only), Zesty media CDNs (`8hxvw8tw.media.zestyio.com`, `mgfrnv8q.media.zestyio.com`).

## Conventions

- Parsley: reuse existing `modules/` and `components/` through `{{ include }}` before writing new
  markup. Look up related items with `<model>.filter({zuid}).field` (see `webengine/views/components/header`).
  JSON endpoints read params with `request.queryParam()` (see `webengine/views/get-informational-page.json`).
- Markup uses Bootstrap 5 utility classes (`d-flex`, `px-lg-8`, `data-bs-*`). Do not add new CSS for
  what utilities already cover.
- New files: extension-less views (snippet/pageset/templateset) **cannot be auto-created** by the
  sync script. A human creates them in the Zesty admin and adds the ZUID to `zesty.config.json`.
  Views with an extension (`.json`, `.html`) and new styles/scripts are created automatically on push to `stage`.
- Branches: one per Coda ticket, named `coda-<number>` (ticket `CODA-1234` → branch `coda-1234`),
  cut from `stage` without tracking it: `git switch -c coda-1234 origin/stage --no-track`.
- Commits: short imperative subject (existing history: "Fix clone relationship counts").
  Reference the ticket ID in commits and PRs, never in shipped files.
- PRs: feature branch → `stage` → `production`. Reviewing the sync preview comment on each PR is a
  required gate before merging.

## Project docs

Working reference for agents and people, in `docs/`:

- `docs/agents.md`: agent workflow and project overrides
- `docs/content-models.md`, `docs/templates.md`, `docs/custom-patterns.md`: how the site is built
- `docs/api-tools.md`: API and MCP tools available to agents
- `docs/qa-test-plan.md`: what QA tests, by ID
- `docs/project-tracking.md`: Coda tickets and SOPs
- `docs/changelog.md`: what changed

If a doc disagrees with the code, the code wins; flag the doc.

## Running and testing

There is no install, lint, test or build step. Views compile on the Zesty platform.

- `.env` (gitignored) holds `ZESTY_INSTANCE_ZUID` and `ZESTY_TOKEN`. Both `scripts/sync-to-zesty.js`
  and the plugin's read-only API helper read them.
- Preview a change on the branch preview, then on stage, after a human merges it. Never test on production.
  Previews and stage are password-gated (`?zpw=`; ask the team, never commit the value). The branch preview
  serves the instance's dev `site.css`, so **stylesheet-only changes can only be verified on stage**.
- The only local check is a read-only sync preview (`commands.sync_report` in `.claude/project.yaml`).
  It never writes to the instance.

## Environments and release

| Environment    | Branch       | URL                                            | What a push does                                                                |
| -------------- | ------------ | ---------------------------------------------- | ------------------------------------------------------------------------------- |
| Branch preview | `coda-####`  | `https://hr1cm7dt-{branch-name}.webengine.zesty.io/` (e.g. `hr1cm7dt-coda-1234`) | Renders the branch from GitHub; no instance write                               |
| Stage          | `stage`      | https://hr1cm7dt-dev.webengine.zesty.io/       | Creates new resources and saves changed files to the instance's **dev** version |
| Production     | `production` | https://thesalvationarmyinternaldev.zesty.dev/ | **Publishes** dev → live for changed resources; never writes repo content       |

`main` is retired. It isn't part of the promotion flow; never branch from it or merge into it.

- Deploys are GitHub Actions (`.github/workflows/zesty-deploy.yml`). Humans merge PRs. A manual
  `workflow_dispatch` can force a publish (`publish`) or a full sync (`full_sync`): human-only.
- Code review and production release approval: the lead and the programmer (`release.approvers` in `.claude/project.yaml`).
- Production smoke checks: the read-only subset in `docs/qa-test-plan.md` §7.

## Project-specific restrictions

- **Never edit, stage or commit anything under `webengine/views/z/`.** These are Zesty layouts
  edited in the CMS (they contain page content). Today they are still tracked and mapped in
  `zesty.config.json`, so a push that touches them overwrites CMS edits on the instance.
- **Never hand-edit `zesty.config.json`.** The sync script and the CI bot (`chore: map new Zesty resource
ZUIDs [skip ci]`) write it. Adding a ZUID for an admin-created view is a human task.
- **Never run `scripts/sync-to-zesty.js` without `--report`/`--dry-run`**, and never set
  `MANUAL_PUBLISH`, `FULL_SYNC` or `BRANCH=production` locally. Writes happen only through CI after a human merge.
- **Never push to `stage`, `production` or `main`**, and never trigger the deploy workflow.
- `.github/workflows/` and `scripts/sync-to-zesty.js` are human-only. Describe the change instead.
- Don't copy the test views (`gb_test`, `api_test`, `blocks_test`, `test.html`, `gisele-test.json`,
  `hero-content-test.json`, `angel-tree-dummy-data.json`) as patterns. They're not production code.
