# Custom Patterns & Shared Conventions

Status: Draft — pending review
Last generated: 2026-10-14 by Claude (zesty-ai:project-discovery, automated codebase scan of `production` @ `cd6ce7c`)

Cross-cutting conventions to **reuse** when building, and known debt **not to copy**. Agents check new work against this list.

## Patterns to reuse

### P-01: Compose pages from modules and components

- What: page templates are thin lists of `{{ include modules/<name> }}`; modules include `components/<name>`.
- Where: `webengine/views/homepage` (5 includes, nothing else), `webengine/views/contact_us`, `webengine/views/components/header`. The full include graph is in `docs/templates.md`.
- Use it when: adding a section to a page. Check `modules/` for an existing section (there are 13 `hero-full*` variants) before writing a new one.
- Gotchas: include paths have no leading slash (`modules/hero`). `stories` uses `/components/socials-share`, the only exception.

### P-02: Resolve related items with `.filter()`

- What: look up a linked item and read one field: `{{ cities.filter({$cityZUID}).title }}`.
- Where: `webengine/views/components/header:1-6`, `webengine/views/get-informational-page.json:7-24`.
- Use it when: a model links to `locations`, `divisions`, `territories`, `cities` or `states` (the organizational hierarchy).
- Gotchas: an item can belong to a location, a division *or* a territory. Templates branch on whichever field is set (see `get-informational-page.json`).

### P-03: JSON endpoint views

- What: `*.json` views (type `ajax-json`) that read `request.queryParam()` and print JSON with Parsley loops; `.toJSON()` serializes a whole item.
- Where: `webengine/views/get-informational-page.json`, `webengine/views/locations.json`, `webengine/views/datasets/mobile_editor/**`.
- Use it when: front-end scripts or other apps need data. New `.json` views are auto-created on the instance by the stage sync.
- Gotchas: commas between `{{each}}` items need care (models expose `_arraycomma`); keep output valid JSON when fields are empty.

### P-04: Skeleton loader reveal

- What: `loader` renders placeholder skeletons and hides `.main-content` (`d-none`) until JS calls `hideMain()` / `hideSlider()` / `hideCardLoader1/2()`.
- Where: `webengine/views/loader:281-349`. `modules/hero:674,717` calls the reveal functions; `loader:347` calls `hideMain()` when there's no `#hero`.
- Use it when: building a page with a hero. The hero must call `hideMain()`, or the page body stays hidden.
- Gotchas: a JS error before the reveal leaves the page blank. QA check X-04 covers this.

### P-05: Analytics through Tealium `utag`

- What: `loader` loads Tealium `utag.js` (profile from a cookie, default `national`; env `prod`/`qa`/`dev` from `instance.env` and the domain). Interaction tracking lives in `webengine/scripts/data-layer/*-tracking.js` and calls `utag.link(...)`.
- Where: `webengine/views/loader:52-120`, `webengine/scripts/data-layer/link-tracking.js:66`.
- Use it when: adding trackable interactions. Extend the matching `*-tracking.js`; don't add inline tags.

### P-06: Bootstrap 5 utilities for layout

- What: spacing, flex and responsive layout come from Bootstrap utility classes; SCSS in `webengine/styles/modules/<module>.scss` covers only what utilities can't.
- Where: `webengine/views/components/header`, `webengine/styles/modules/`.

## Known debt (do not copy)

### D-01: Platform-owned layouts tracked in git

- What and where: `webengine/views/z/` (1,094 `z/pvl/7-*.{zhtml,json}` plus `z/layouts/*.json`) is Zesty Layout-editor output containing page content. It is tracked (added in `eb43c2d` init and `6390473` SYNC) and mapped in `zesty.config.json` (1,094 keys under `/z/`).
- Why it matters: the stage sync saves any mapped file in the push diff to the instance's dev version, overwriting CMS layout edits. A `full_sync` dispatch would overwrite all of them.
- Verified 2026-10-14 (`GET /v1/web/views`): the repo copy is a stale snapshot. **1,030 of the 1,094 `/z/` mappings point at ZUIDs the instance no longer has**, and the instance now has 1,398 `z/` views. The sync script throws on the first non-2xx response, so a diff or `full_sync` touching a stale `z/` file fails the whole deploy.
- The instance also has 3,144 `menu-config/*.json` views (platform menus). They aren't in git; keep it that way.
- Instead: never edit them. Planned fix (human): untrack, ignore `/webengine/views/z/` (and `/webengine/views/menu-config/`), and make the sync script skip `/z/` keys.

### D-02: Test and dummy views shipped to the instance

- What and where: `gb_test`, `api_test`, `blocks_test`, `test.html`, `gisele-test.json`, `hero-content-test.json`, `angel-tree-dummy-data.json`. All are mapped in `zesty.config.json`. `blocks_test` and `test.html` are no longer on the instance (stale mappings, see D-07); the rest still are.
- Instead: don't add test views to `webengine/`. Use the branch preview.

### D-03: Sync script changed-file scope breaks on Windows

- What and where: `scripts/sync-to-zesty.js:516` compares `git diff --name-only` paths (forward slashes) with `path.relative()` (backslashes on Windows), so a local diff-scoped `--report` run reports 0 changes. CI runs on Ubuntu and is unaffected.
- Instead: run `--report` locally from WSL/Git Bash with the paths normalized, or rely on the PR comment. Fixing the script is human-only.

### D-04: Debug output left in views and scripts

- What and where: 64 `console.log` / `$console` occurrences across 23 files outside `z/`, for example `{{$console = 'hit here services'}}` in `components/header:7`, 20 in `angel-tree-script` and 12 in `modules/map-angel-tree`.
- Instead: no debug output in new work. Remove it from lines you touch.

### D-05: View on disk but not mapped

- What and where: `webengine/views/service_area` (tracked since `eb43c2d`) has no entry in `zesty.config.json`, so the sync never manages it. It is extension-less, so it can't be auto-created either.
- Verified 2026-10-14: it exists on the instance as pageset view `11-fec5e6b0ac-rq17fs`, rendering model `service_areas`.
- Instead: a human adds that ZUID to `zesty.config.json` (after comparing the repo file with the instance code).

### D-07: Repo mappings out of step with the instance

- What and where (verified 2026-10-14):
  - **Wrong ZUIDs:** `volunteer_pages` is mapped to `11-f4f68486de-xlxcjm` but the instance view is `11-da9bc5ca92-wn33p3`. `services_seed_content` is mapped to `11-dcded1f1df-v7hj0v` but the instance view is `11-d4b78cddb9-nv88m7`. A push touching either file fails the deploy (PUT to a missing ZUID).
  - **Mapped but gone from the instance:** `informational_page_clone`, `blocks_test`, `test.html`, `modules/services-intro`, `-/block/info_block_4_column.html`, `-/block/text_left_with_cta_image_right.html`, `-/block/find_a_location___cta_box___ja.html`.
  - **On the instance but not in the repo:** `stuff_the_bus`, `tags`, `informational_pages_dev`, 6 blocks, and about 30 snippets and endpoints (`docs/templates.md`, "Views that exist only on the instance").
- Why it matters: the repo isn't the full source of truth. Admin edits never reach git, and repo edits to stale files fail or go nowhere.
- Instead: before changing a view, check its ZUID against the instance (read-only `GET /v1/web/views/{zuid}`). Reconciling mappings is a human task on a feature branch.

### D-06: Third-party API token in client-side code (security)

- What and where: a Quickbase user token is hardcoded in browser-delivered views: `webengine/views/modules/map-angel-tree:348` (`Authorization: QB-USER-TOKEN …`) and `webengine/views/angel-tree-script:4` (`CONFIG.QB_TOKEN`), realm `saapps.quickbase.com`. It's been in git history since `eb43c2d` (2026-06-23), and anyone can read it from the page source.
- Instead: never put credentials in `webengine/**`. Rotate the token; serve the data through a server-side endpoint or a read-only, scoped mechanism. Human decision required.

## Deploy and sync pipeline

| Trigger | Workflow | What `scripts/sync-to-zesty.js` does |
| --- | --- | --- |
| PR into `stage` | `zesty-pr-preview.yml` | `REPORT=true`: dry-run; comments "Stage sync preview" (new/changed vs dev) |
| PR into `production` | `zesty-pr-preview.yml` | `REPORT=true`: comments "Production publish preview" (dev → live) |
| Push to `stage` | `zesty-deploy.yml` | Creates new resources (extension views, styles, scripts), PUTs changed files to **dev**, never publishes; the bot commits new ZUIDs to `zesty.config.json` |
| Push to `production` | `zesty-deploy.yml` | Publishes the existing dev version → live for changed resources (`POST …/versions/<n>?purge_cache=true`); never writes content or creates |
| `workflow_dispatch` | `zesty-deploy.yml` | `publish` forces publish; `full_sync` processes every mapped file. **Human-only** |

- Scope: only files in `git diff DIFF_BASE..HEAD`, unless no base is known (first push, manual dispatch), which falls back to a full sync. Before writing, the script compares content to the instance and skips no-op saves.
- Safe for agents: `node scripts/sync-to-zesty.js --report` (or `--dry-run`), which is GET-only. Never run it without one of those flags.
- A resource only reaches production if it was saved to dev through `stage` first.

## Generated or protected files

| File | Written by | Rule |
| --- | --- | --- |
| `zesty.config.json` | sync script + `github-actions[bot]` ("chore: map new Zesty resource ZUIDs [skip ci]") | Never hand-edit; humans add ZUIDs for admin-created views |
| `webengine/views/z/**` | Zesty Layout editor (platform) | Never edit, stage or commit |
| `zesty-report.md` | sync script `--report` | Throwaway output; don't commit |
| `.github/workflows/*`, `scripts/sync-to-zesty.js` | humans | Human-only; describe proposed changes |

## Open Questions / Flags

- Are `datasets/mobile_editor/**` endpoints owned by this repo or by the mobile-editor app's team?
- Is `zesty-report.md` meant to be gitignored? It isn't today.
- A second instance ZUID, `8-da979ebeab-d59gnx`, appears in an image URL at `webengine/views/loader:24` (`…manager.zesty.io/zestyLogoOnlyGrey.svg`). Is that intentional?
- `loader:274` links editors to `salvationarmy.mobile.zesty.io/content/…` (the mobile editor app). Confirm it's still in use.

## Review status

Not yet reviewed.
